---
layout: post
title: Cephadm(containerized) ile Ceph(Pacific) Storage Kurulumu on Linux
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/25-26-20-uelinuxegitim/25-26-20le05.png
#share-img: /assets/img/path.jpg
tags: [ceph, object, container, linux, ubuntu, docker, podman, storage]
comments: true
---
# ![](/assets/img/cphadm/cephadm-dash.png)

**cephadm octopus** versiyonu ile gelmiş olup tamamen **container** yapıda **cluster** ayağa kaldrımaktadır.

**Ceph is the future of storage** : Açık kaynak kodlu, dağıtık, yazılım tabanlı storage sistemidir. **Object**, **block** ve **file-level storage** desteklemektedir. Geleneksel depolama sistemlerinin kısıtlarına karşın **Ceph** ölçeklenebilir, esnek ve donanım bağımsızlığı sayesinde ekonomikliliğide beraberinde getirir. Ayrıca dağıtık yapıda çalışması sayesinde olası bir arızayı karşın otomatik onarım işlemlerini başlatması, **exabyte** seviyelerine kadar çıkabilmesi ve **3-in-1** protokol desteği **Ceph**’i popüler hale getirmiştir.

Yapımız kısaca aşağıdaki gibi; **1 node Ceph servisleri**(**Monitör**, **Management**, **Rados Gateway**, **Admin client**) + **3 OSD node** toplam **4 node**. Her **OSD node**’sinde **storage cluster** için 3’er ayrı disk(/dev/sdb, dev/sdc, dev/sdd gibi). Siz kendi yapınız için yatayda **OSD** ve dikeyde disk yapınızı çoğaltabilirsiniz.

İlk olarak **docker** dahil gerekli paketleri indirip, kuralım.
**Bütün Node**'lerde çalıştırılmalıdır.
~~~
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update && sudo apt install chrony python3-pip python3 ca-certificates curl gnupg lsb-release lvm2 docker-ce docker-ce-cli containerd.io -y
~~~

~~~
sudo vi /etc/chrony/chrony.conf
~~~
**NTP_SERVER** değiştirilmelidir.
~~~
#pool ntp.ubuntu.com        iburst maxsources 4
#pool 0.ubuntu.pool.ntp.org iburst maxsources 1
#pool 1.ubuntu.pool.ntp.org iburst maxsources 1
#pool 2.ubuntu.pool.ntp.org iburst maxsources 2
server $NTP_SERVER iburst
~~~
~~~
sudo systemctl restart chrony.service
chronyc sources
~~~

~~~
echo "$USER ALL = (root) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/$USER
sudo chmod 0440 /etc/sudoers.d/$USER
~~~

~~~
sudo vi /etc/hosts
~~~
~~~
10.10.10.144 ceph-yonetici
10.10.10.145 ceph-osdx
10.10.10.146 ceph-osdy
10.10.10.147 ceph-osdz
~~~

**Admin Node**'de çalıştırılır(ceph-yonetici).
~~~
ssh-keygen
ssh-copy-id $USER@ceph-yonetici
ssh-copy-id $USER@ceph-osdx
ssh-copy-id $USER@ceph-osdy
ssh-copy-id $USER@ceph-osdz
~~~

~~~
vi ~/.ssh/config
~~~
**$USER** parametresi anlık kullanıcı ile değiştirin.
~~~
Host ceph-yonetici
   Hostname ceph-yonetici
   User $USER
Host ceph-osdx
   Hostname ceph-osdx
   User $USER
Host ceph-osdy
   Hostname ceph-osdy
   User $USER
Host ceph-osdz
   Hostname ceph-osdz
   User $USER
~~~

**cephadm**'yi kuralım.
~~~
curl --silent https://download.ceph.com/keys/release.asc | sudo apt-key add -
sudo apt-add-repository https://download.ceph.com/debian-pacific
sudo apt install -y cephadm
~~~

**$USER** parametresini anlık kullanıcı(şifresiz root yetkisine sahip olan bir kullanıcı) ile değiştirin, **--mon-ip** ceph-yonetici **ip**'si olmalıdır.
~~~
sudo cephadm bootstrap --mon-ip 10.10.10.144 --initial-dashboard-user admin --initial-dashboard-password 1234 --ssh-user $USER --cluster-network 10.10.10.0/24
~~~
**parametre** değerlerini anlamak adına https://docs.ceph.com/en/latest/cephadm/install/ linkini kontrol edebilirsiniz.

Servisleri gözlemleyebilirsiniz.
~~~
systemctl status ceph-* --no-pager
~~~

Ceph komutlarını yürütmek için yukarıdaki **bootstrap** çıktısında verilen "**You can access the Ceph CLI with:**" satırını kullanabilirsiniz. İlgili komut sizi direk **admin container**'a bağlayacaktır.
~~~
sudo /usr/sbin/cephadm shell --fsid d0232fa8-6d5b-11ec-8fba-23b487996ad5 -c /etc/ceph/ceph.conf -k /etc/ceph/ceph.client.admin.keyring
~~~

Ama biz direk **host** üzerinden de komutları yürütebilmek için aşağıdaki yöntem/leri izleyebiliriz.
~~~
alias sudo='sudo '
alias ceph='cephadm shell -- ceph'
alias radosgw-admin='cephadm shell -- radosgw-admin'
echo "alias sudo='sudo '" >> ~/.bashrc
echo "alias ceph='cephadm shell -- ceph'" >> ~/.bashrc
echo "alias radosgw-admin='cephadm shell -- radosgw-admin'" >> ~/.bashrc
~~~
Ya da
~~~
cephadm add-repo --release pacific
cephadm install ceph-common
~~~
Ya da
~~~
sudo apt install ceph-common
~~~

**ceph-yonetici host**'unu **_admin**'e ek olarak **mon** ve **mgr** olarak ta etiketleyelim çünkü servisler o **host** üzerinde ayağa kalktı.
~~~
sudo ceph orch host label add ceph-yonetici mon  [ya da (cephadm shell -- ceph orch host label add ceph-yonetici mon)]
sudo ceph orch host label add ceph-yonetici mgr  [ya da (cephadm shell -- ceph orch host label add ceph-yonetici mgr)]
~~~

Aşağıdaki komutlar ile **cluster**'ın durumunu inceleyebilirsiniz.
~~~
sudo ceph -s
sudo ceph orch host ls
sudo ceph orch device ls
sudo ceph orch ps
~~~

Şimdi diğer **host**'ları(OSD) **cluster**'a dahil edeceğiz ama öncesinde **public key**'i dağıtalım. **$USER** parametresi anlık kullanıcı ile değiştirin.
~~~
ssh-copy-id -f -i /etc/ceph/ceph.pub $USER@ceph-osdx
ssh-copy-id -f -i /etc/ceph/ceph.pub $USER@ceph-osdy
ssh-copy-id -f -i /etc/ceph/ceph.pub $USER@ceph-osdz
~~~

**Host**'ları dahil edelim.
~~~
sudo ceph orch host add ceph-osdx 10.10.10.145 osd
sudo ceph orch host add ceph-osdy 10.10.10.146 osd
sudo ceph orch host add ceph-osdz 10.10.10.147 osd
~~~

Şimdi tekrar **cluster**'ın durumunu inceleyin.
~~~
sudo ceph -s
sudo ceph orch host ls
sudo ceph orch device ls
sudo ceph orch ps
~~~

Şimdi ise **device ls** ile **OSD node**'lerinde listelediğimiz diskleri **OSD** olarak aynı anada ekleyelim.
~~~
sudo ceph orch apply osd --all-available-devices
~~~
Ya da özellikle işaret ederek de ekleyebiliriz.
~~~
sudo ceph orch daemon add osd <host>:<device-path>
~~~

**Object storage** kullanımı için **Rados GW**'yi aktif edelim.
**rgw default port'u 80'dir ve yine default'ta 2 daemon up** eder.
~~~
sudo ceph orch apply rgw ceph-yonetici [ya da sudo ceph orch apply rgw ceph-yonetici '--placement=label:rgw count-per-host:1' --port=7480]
sudo ceph orch host label add ceph-yonetici rgw
~~~

Ek olarak **monitor**, **manager** ve **radosgw daemon** eklemek isterseniz,
~~~
# sudo ceph orch apply mon --placement="ceph-yonetici,ceph-osdx,ceph-osdy"
# sudo ceph orch apply mgr --placement="ceph-yonetici,ceph-osdx,ceph-osdy"
# sudo ceph orch apply rgw --placement="ceph-yonetici,ceph-osdx,ceph-osdy"
~~~

**Daemon** - **Servis** silmek isterseniz,
~~~
# sudo ceph orch rm rgw.ceph-yonetici
# sudo ceph orch daemon rm rgw.ceph-osdx
~~~

**Ceph Dashboard**'dan ve **API**'den **radosgw**'ye erişebilmek için kullanıcı oluşturabilirsiniz fakat **default**'ta var olarak gelir.
**Changed $ACCESS_KEY and $SECRET_KEY**
~~~
sudo ceph dashboard set-rgw-credentials
sudo radosgw-admin user create --uid="cephdash" --display-name="Ceph DashBoard2" --system
sudo /usr/sbin/cephadm shell --fsid d0232fa8-6d5b-11ec-8fba-23b487996ad5 -c /etc/ceph/ceph.conf -k /etc/ceph/ceph.client.admin.keyring
echo $ACCESS_KEY > /tmp/access_key
echo $SECRET_KEY > /tmp/secret_key
sudo ceph dashboard set-rgw-api-access-key -i /tmp/access_key
sudo ceph dashboard set-rgw-api-secret-key -i /tmp/secret_key
~~~

**Pool**'umuzu oluşturalım.
~~~
sudo ceph osd crush rule ls
sudo ceph osd pool create default.rgw.buckets.data 32 32 replicated replicated_rule
sudo ceph osd pool application enable default.rgw.buckets.data rgw
~~~

**Swift user** oluşturuyorum, siz isterseniz **s3 user** oluşturabilirsiniz.
~~~
sudo radosgw-admin --uid "ceph-demo" --display-name "Ceph Demo Kurulum Kullanıcısı" --subuser=ceph-demo:swift --key-type swift --access full user create
sudo radosgw-admin --uid "ceph-demo" --max-buckets=0 user modify
~~~

Uygulama bağlantısı için kullanılabilecek bilgiler
~~~
ceph.username = ceph-demo:swift
ceph.password = $SECRET_KEY
ceph.endpoint = http://10.10.10.144:7480/swift/v1
ceph.auth.url = http://10.10.10.144:7480/auth/1.0
~~~

Kullanışlı komutlar,
~~~
sudo ceph osd pool ls / sudo ceph osd pool ls detail
sudo ceph df detail
sudo ceph osd status
sudo ceph health detail
sudo ceph -s
sudo radosgw-admin user list
sudo radosgw-admin user info --uid=ceph-demo
~~~

~~~
  cluster:
    id:     d0232fa8-6d5b-11ec-8fba-23b487996ad5
    health: HEALTH_OK

  services:
    mon: 4 daemons, quorum ceph-yonetici,ceph-osdx,ceph-osdy,ceph-osdz (age 19h)
    mgr: ceph-yonetici.uwijtu(active, since 19h), standbys: ceph-osdx.znrgdv
    osd: 9 osds: 9 up (since 13h), 9 in (since 13h)
    rgw: 1 daemons active (2 hosts, 1 zones)

  data:
    pools:   6 pools, 137 pgs
    objects: 240 objects, 6.1 KiB
    usage:   892 MiB used, 89 GiB / 90 GiB avail
    pgs:     137 active+clean
~~~
<br>

ref:
https://docs.ceph.com/en/pacific/cephadm/install/#enable-ceph-cli <br>
https://chowdera.com/2021/04/20210411052419702x.html <br>
https://dev.to/akhal3d96/exploring-ceph-in-a-multi-node-setup-3c8h <br>
https://achchusnulchikam2.medium.com/deploy-ceph-cluster-with-cephadm-on-centos-8-257b300e7b42 <br>



