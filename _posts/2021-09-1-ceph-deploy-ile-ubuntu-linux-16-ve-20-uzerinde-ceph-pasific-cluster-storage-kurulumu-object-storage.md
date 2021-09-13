---
layout: post
title: ceph-deploy ile Ubuntu Linux 16 ve 20 Üzerinde Ceph Pasific Cluster Storage Kurulumu – Object Storage
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/25-26-20-uelinuxegitim/25-26-20le05.png
#share-img: /assets/img/path.jpg
tags: [ceph, storage, linux, ubuntu, cluster, sds, object]
comments: true
---
**Ceph is the future of storage** : Açık kaynak kodlu, dağıtık, yazılım tabanlı storage sistemidir. **Object**, **block** ve **file-level** storage desteklemektedir. Geleneksel depolama sistemlerinin kısıtlarına karşın Ceph **ölçeklenebilir**, **esnek** ve **donanım bağımsız**lığı sayesinde **ekonomik**liliğide beraberinde getirir. Ayrıca dağıtık yapıda çalışması sayesinde olası bir arızayı karşın otomatik onarım işlemlerini başlatması, exabyte seviyelerine kadar çıkabilmesi ve **3-in-1 protokol** desteği Ceph'i bu denli zevkle kullanılabilir ve göz bebeği haline getirmiştir.

Yapımız kısaca aşağıdaki gibi;
1 node Ceph servisleri(**Monitör**, **Management**, **Rados Gateway**, **Admin client**) + 3 OSD node toplam 4 node.
Her OSD node’sinde storage cluster için 3’er ayrı disk(/dev/sdb, dev/sdc, dev/sdd gibi).
Siz kendi yapınız için yatayda OSD ve dikeyde disk yapınızı çoğaltabilirsiniz.

Her **OSD** sunucusunda diskler herhangi bir yere mount edilmemiş ve lvm yapılmamış olmalıdır. Ben 100GB alan verdim.
~~~
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0  100G  0 disk 
|-sda1   8:1    0   99G  0 part /
|-sda2   8:2    0    1K  0 part 
`-sda5   8:5    0  975M  0 part [SWAP]
sdb      8:16   0   100G  0 disk 
sdc      8:32   0   100G  0 disk 
sdd      8:48   0   100G  0 disk 
sr0     11:0    1 1024M  0 rom  

~~~

**Bütün node**'lerde çalıştırılmalı;
~~~
sudo apt update
#ubuntu 16
sudo apt install chrony python-minimal -y
#ubuntu 20
sudo apt install chrony  python3-pip -y
~~~

~~~
sudo sed -i 's/pool 2.debian.pool.ntp.org offline iburst/server $NTP_ADDRESS iburst/g' /etc/chrony/chrony.conf
sudo systemctl restart chrony.service
~~~

~~~
sudo useradd -d /home/cephuser -m cephuser
sudo passwd cephuser
#enter a password: 'Complex_Pass'
echo "cephuser ALL = (root) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/cephuser
sudo chmod 0440 /etc/sudoers.d/cephuser
~~~

~~~
sudo vi /etc/hosts

10.10.10.201 ceph-ops
10.10.10.202 ceph-stgx
10.10.10.203 ceph-stgy
10.10.10.204 ceph-stgz
~~~

**Sadece Admin node**'de çalıştırılmalı(yani ceph servislerinin kurulacağı **ceph-ops node**'si);
~~~
ssh-keygen

ssh-copy-id cephuser@ceph-ops
ssh-copy-id cephuser@ceph-stgx
ssh-copy-id cephuser@ceph-stgy
ssh-copy-id cephuser@ceph-stgz
~~~

~~~
sudo vi ~/.ssh/config

Host ceph-ops
   Hostname ceph-ops
   User cephuser
Host ceph-stgx
   Hostname ceph-stgx
   User cephuser
Host ceph-stgy
   Hostname ceph-stgy
   User cephuser
Host ceph-stgz
   Hostname ceph-stgz
   User cephuser
~~~

~~~
#ubuntu 16
wget -q -O- 'https://download.ceph.com/keys/release.asc' | sudo apt-key add -
echo deb https://download.ceph.com/debian-mimic/ $(lsb_release -sc) main | sudo tee /etc/apt/sources.list.d/ceph.list

#ubuntu 20
#NoT: U20 için repoya source'ye eklerken sources.list.d'ye eklemek yerine direkt olarak "/etc/apt/source.list" altına eklemeniz daha uygun olabilir.
#Çünkü özellikle bu kurulum sırasında pasific kurmak isterken ceph sources.list.d'nin altındaki ceph.list source'sini octopus'a çekiyor.
#Fakat source.list altına eklediğinizde, burda yetkisi olmadığından ve değiştiremediğinden pacific versiyonunda kurulum sağlayabiliyorsunuz. "deb https://download.ceph.com/debian-pacific/ focal main"

wget -q -O- 'https://download.ceph.com/keys/release.asc' | sudo apt-key add -
echo deb https://download.ceph.com/debian-octopus/ $(lsb_release -sc) main | sudo tee /etc/apt/sources.list.d/ceph.list

sudo apt update

#ubuntu 16
sudo apt install ceph-deploy -y

#ubuntu 20
pip3 install git+https://github.com/ceph/ceph-deploy.git
PATH=$PATH:/home/$USER/.local/bin

mkdir mycluster
cd mycluster
ceph-deploy new ceph-ops
~~~

~~~
vi ceph.conf

osd pool default pg num = 8
osd pool default pgp num = 8
mon_initial_members = ceph-ops
mon_host = 10.10.10.201
public network = 10.10.10.0/24
cluster network = 10.10.20.0/24
rgw_override_bucket_index_max_shards = 500
mon_pg_warn_max_object_skew = 0
~~~

~~~
ceph-deploy install ceph-ops ceph-stgx ceph-stgy ceph-stgz
ceph-deploy mon create-initial
ceph-deploy admin ceph-ops
ceph-deploy mgr create ceph-ops
ceph-deploy rgw create ceph-ops
~~~

~~~
#Fiziksel osd'lerde öncesinde yapılandırlmış LVM varsa tespit edilip, temizlenmelidir.
ceph-deploy osd create ceph-stgx --data /dev/sdb
ceph-deploy osd create ceph-stgx --data /dev/sdc
ceph-deploy osd create ceph-stgx --data /dev/sdd

ceph-deploy osd create ceph-stgy --data /dev/sdb
ceph-deploy osd create ceph-stgy --data /dev/sdc
ceph-deploy osd create ceph-stgy --data /dev/sdd

ceph-deploy osd create ceph-stgz --data /dev/sdb
ceph-deploy osd create ceph-stgz --data /dev/sdc
ceph-deploy osd create ceph-stgz --data /dev/sdd
~~~

~~~
#Ceph kurulumlarında OSD başına 50 ila 100 arasında PG düşmeli, bundan dolayı OSD sayısıyla 50 ve 100 çarpılarak minumum ve maksimum PG değerleri elde edilir.
#Örnek olarak 9 OSD'li bir kurulumda açılacak pool'ların PG değerlerinin toplamı 9x50=450 ile 9x100=900 arasında olmalıdır.
#default.rgw.buckets.data pool'una 450 ile 900 arasındaki 2'nin katı olan 512 değeri verilir, böylece ceph'in açma potansiyeli olan diğer pool'lar için verilen 8 default pg num(ceph.conf'da verildi) değeriyle bu eşik aşılmamış olur.

sudo ceph osd pool create default.rgw.buckets.data 512 512 replicated replicated_rule 20000000
~~~

~~~
#Swift user oluşturuyorum, siz isterseniz s3 user oluşturabilirsiniz.
sudo radosgw-admin --uid "ceph-demo" --display-name "Ceph Demo Kurulum Kullanıcısı" --subuser=ceph-demo:swift --key-type swift --access full user create
sudo radosgw-admin --uid "ceph-demo" --max-buckets=0 user modify

#Uygulama bağlantısı için kullanılabilecek bilgiler
ceph.username = ceph-demo:swift
ceph.password = W2ocBJ4WwWvHfsARsO9PtD9HbFmJNKI4CYEZFACQ
ceph.endpoint = http://10.10.10.201:7480/swift/v1
ceph.auth.url = http://10.10.10.201:7480/auth/1.0
~~~

Ceph dashboard'ı aktif edebilirsiniz.
~~~
#ubuntu 16
sudo ceph mgr module enable dashboard
sudo ceph dashboard create-self-signed-cert
sudo ceph config set mgr mgr/dashboard/server_addr ceph-ops
sudo ceph config set mgr mgr/dashboard/server_port 8443
sudo ceph mgr services #service url check
sudo ceph dashboard set-login-credentials admin admin

#ubuntu 20
sudo apt install ceph-mgr-dashboard
sudo ceph mgr module enable dashboard
sudo ceph dashboard create-self-signed-cert
sudo ceph config set mgr mgr/dashboard/server_addr ceph-ops
sudo ceph config set mgr mgr/dashboard/server_port 8443
sudo ceph mgr services #service url check
#Set the login credentials. Password read from -i <file>
sudo ceph dashboard set-login-credentials admin -i admin.txt
~~~

Aşağıdaki komutlarla da Ceph'i kontol edebilirsiniz.
~~~
sudo ceph osd pool ls / sudo ceph osd pool ls detail
sudo ceph df detail
sudo ceph osd status
sudo ceph health detail
sudo ceph -s
~~~

~~~
  cluster:
    id:     9c2bdf0d-528a-47c0-b466-4ed3b404162c
    health: HEALTH_OK
 
  services:
    mon: 1 daemons, quorum ceph-ops (age 40m)
    mgr: ceph-ops(active, since 4m)
    osd: 9 osds: 9 up (since 37m), 9 in (since 37m)
    rgw: 1 daemon active (ceph-ops)
 
  task status:
 
  data:
    pools:   6 pools, 138 pgs
    objects: 189 objects, 5.1 KiB
    usage:   11 GiB used, 79 GiB / 278 GiB avail
    pgs:     137 active+clean

~~~

![Crepe](/assets/img/cph-dply-u/cph-octps01.png)
