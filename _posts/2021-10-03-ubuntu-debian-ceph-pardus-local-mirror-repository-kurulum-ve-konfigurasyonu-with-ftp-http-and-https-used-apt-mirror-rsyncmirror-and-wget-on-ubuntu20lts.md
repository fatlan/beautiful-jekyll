---
layout: post
title: Ubuntu/Debian/Ceph/Pardus Local Mirror, Repository Kurulum ve Konfigürasyonu with Ftp, Http and Https Used apt-mirror, Rsyncmirror and Wget on Ubuntu20LTS
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/25-26-20-uelinuxegitim/25-26-20le05.png
#share-img: /assets/img/path.jpg
tags: [repo, mirror, apt, linux, ubuntu, yansı, proftp, apache2, rsync, deb, repository, httpd]
comments: true
---

![Crepe](/assets/img/umirrepo/umirrepo01.png)

İnternet hızından, güvenlik sebebiyle ya da intranet ağınızdan dolayı **apt** için **local** **yansı** kurmanız gerekebilir.

Aşağıda ülke bazında yakın depoları kullanarak yansı oluşturabiliriz.

Ubuntu: [https://launchpad.net/ubuntu/+archivemirrors](https://launchpad.net/ubuntu/+archivemirrors) <br>
Debian: [https://wiki.debian.org/SourcesList#Repository_URL](https://wiki.debian.org/SourcesList#Repository_URL) | [https://debgen.simplylinux.ch/](https://debgen.simplylinux.ch/) <br>
Ceph: [https://download.ceph.com/](https://download.ceph.com/) <br>
Pardus: [https://belge.pardus.org.tr/display/PYMBB/Pardus+Repo+Mirrorlama](https://belge.pardus.org.tr/display/PYMBB/Pardus+Repo+Mirrorlama)

Bu işlem için ilgili makinede yeterince disk alanı(TB olması tavsiye edilir) ayrıca bonding **nic** ve **simetrik** yüksek seviyede net **bandwidth** olması tavsiye edilir.
~~~
sudo apt install apt-mirror proftpd-basic apache2 rsync wget git
~~~

####With apt-mirror

Konuya girmeden önce, **default**ta yüklenen **apt-mirror** paketinde bazı sorunlar mevcut bu yüzden aşağıdaki ilgili repodaki **apt-mirror**’u kendinize indirin ve **default** **apt-mirror** yerine aşğıdaki gibi güncelleyin.
~~~
git clone https://github.com/Stifler6996/apt-mirror.git
cd apt-mirror
cp apt-mirror /usr/bin/apt-mirror
~~~

~~~
sudo vi /etc/apt/mirror.list
~~~

Aşağıdaki satırları değiştiriyorum, verileri saklayacağı yeri **data**’nın altında ayarlıyorum ayrıca **Ubuntu 20** ve **Ceph** repolarını aşağıdaiki gibi ekliyorum.
~~~
set base_path    /data/apt-mirror


deb http://archive.ubuntu.com/ubuntu focal main restricted universe multiverse
deb-src http://archive.ubuntu.com/ubuntu focal main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu/ focal-updates main restricted universe multiverse
deb-src http://archive.ubuntu.com/ubuntu/ focal-updates main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu/ focal-security main restricted universe multiverse
deb-src http://archive.ubuntu.com/ubuntu/ focal-security main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu/ focal-backports main restricted universe multiverse
deb-src http://archive.ubuntu.com/ubuntu/ focal-backports main restricted universe multiverse
deb http://archive.canonical.com/ubuntu focal partner
deb-src http://archive.canonical.com/ubuntu focal partner
deb https://download.ceph.com/debian-pacific/ focal main restricted universe multiverse
~~~

Ve **mirror**’lamayı başlatıyoruz.
~~~
sudo apt-mirror
~~~

İşlem bitiminde ls ile kontrol edelim.
~~~
fatlan@u20-repo:~$ ls /data/apt-mirror/mirror/
archive.canonical.com  archive.ubuntu.com  download.ceph.com
~~~

Şimdi **Proftp**’yi hem **directory**’sini(**/srv/ftp**) hem de kullanıcıların(**anonymous** olarak) bağlanabilmesi için yapılandıralım.
~~~
sudo vi /etc/proftpd/conf.d/anonymous.conf
~~~

~~~
<Anonymous ~ftp>
   User                    ftp
   Group                nogroup
   UserAlias         anonymous ftp
   RequireValidShell        off
#   MaxClients                   10
   <Directory *>
     <Limit WRITE>
       DenyAll
     </Limit>
   </Directory>
 </Anonymous>
~~~

Servisi başlatalım
~~~
sudo systemctl restart proftpd.service
~~~

Şimdi **apt-mirror path**’lerini **ftp** için **mount** edelim.
~~~
sudo mount --bind /opt/apt-mirror/mirror/archive.ubuntu.com/  /srv/ftp/
~~~

**Ceph için**;
~~~
sudo mkdir -r /srv/ftp/ceph
sudo mount --bind /opt/apt-mirror/mirror/download.ceph.com/  /srv/ftp/ceph/
~~~

**Debian** ve **Pardus** için;
~~~
sudo mkdir -r /srv/ftp/debian
sudo mkdir -r /srv/ftp/pardus
sudo mount --bind /opt/apt-mirror/mirror/deb.debian.org/  /srv/ftp/debian/
sudo mount --bind /opt/apt-mirror/mirror/depo.pardus.org.tr/  /srv/ftp/pardus/
~~~

“**mount**” ve “**df -h**” komutlarıyla durumu gözlemleyebilirsiniz.

Şimdi **crontab**’ı yapılandıralım
~~~
crontab -e
~~~
~~~
@reboot sleep 5
@reboot sudo mount --bind /opt/apt-mirror/mirror/archive.ubuntu.com/  /srv/ftp/
@reboot sudo mount --bind /opt/apt-mirror/mirror/download.ceph.com/  /srv/ftp/ceph/
@reboot sudo mount --bind /opt/apt-mirror/mirror/deb.debian.org/  /srv/ftp/debian/
@reboot sudo mount --bind /opt/apt-mirror/mirror/depo.pardus.org.tr/  /srv/ftp/pardus/
0  2  *  *  *  sudo /usr/bin/apt-mirror >> /data/apt-mirror/mirror/archive.ubuntu.com/ubuntu/apt-mirror.log
~~~

Şimdi de **apache2**’yi yapılandıralım. Ben **default** config üzerinde sadece **80** olarak yapılandıracağım, siz isterseniz hem **özelleştirebilir** hem de **443**’ü aktif edebilirsiniz.
~~~
sudo vi /etc/apache2/sites-enabled/000-default.conf
~~~
~~~
<VirtualHost *:80>
        ServerAdmin webmaster@localhost
        DocumentRoot /srv/ftp/

        <Directory "/srv/ftp">
            Options Indexes MultiViews
            AllowOverride None
            Require all granted
        </Directory>

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>
~~~

####With rsync

Sadece **apt-mirror** yerinei dosyaları indirmek ve güncel tutmak için **rsync**’yi kullanacağız. **Proftp** ve **apache2** aynı kalacak, sadece dizin yolunu değiştirmeniz gerekebilir.

~~~
sudo mkdir -p /data/apt-mirror/ubuntu
rsync -a rsync://archive.ubuntu.com/ubuntu /data/apt-mirror/ubuntu

#rsync bant genişliği belirlemek için(bayt olarak ölçer) --bwlimit=256 ve indirmenin ilerlemesini görmek için “--progress” parametrelerini rsync komutuna ekleyebilirsiniz.
~~~

Yansıyı güncel tutmak içinse [https://wiki.ubuntu.com/Mirrors/Scripts](https://wiki.ubuntu.com/Mirrors/Scripts) adresindeki hazır scripti("**/usr/local/bin/sync_ubuntu_mirror.sh**") kullanabilirsinz.
~~~
crontab -e
~~~
~~~
0 2 * * * /usr/local/bin/sync-ubuntu-mirror.sh > /dev/null 2> /dev/null
~~~

####With wget

~~~
sudo mkdir -p /data/apt-mirror/ubuntu
cd /data/apt-mirror/ubuntu
wget -r -nH --no-parent http://archive.ubuntu.com/ubuntu/

#isterseniz indirilen kısımlardan belli dosya ve versiyonlu yapıları exclude edebilirsiniz. --reject="index.html*" --reject="*-dbg*" --reject="*13.2.0*"
~~~

Ama hem **repo**yu güncel tutmak için ve hem de her seferinde indirmeleri başa sarmamak(**bandwidth**’ten yememek) için yukardaki **mirror script**ini kullamalısınız.

###CLİENT TARAFI

~~~
sudo vi /etc/apt/sources.list
~~~
~~~
deb http://10.10.10.203/ubuntu/ focal-updates main restricted universe multiverse
deb-src http://10.10.10.203/ubuntu/ focal-updates main restricted universe multiverse

deb http://10.10.10.203/ubuntu/ focal-security main restricted universe multiverse
deb-src http://10.10.10.203/ubuntu/ focal-security main restricted universe multiverse

deb http://10.10.10.203/ubuntu/ focal-backports main restricted universe multiverse
deb-src http://10.10.10.203/ubuntu/ focal-backports main restricted universe multiverse

deb http://10.10.10.203/ubuntu focal partner
deb-src http://10.10.10.203/ubuntu focal partner

deb http://10.10.10.203/ubuntu focal main restricted universe multiverse
deb-src http://10.10.10.203/ubuntu focal main restricted universe multiverse

deb http://10.10.10.203/ceph/debian-pacific/ focal main
~~~

~~~
fatlan@u20-repo-client:~$ sudo apt update
Hit:1 http://10.10.10.203/ubuntu focal-updates InRelease
Hit:2 http://10.10.10.203/ubuntu focal-security InRelease
Hit:3 http://10.10.10.203/ubuntu focal-backports InRelease
Get:4 http://10.10.10.203/ubuntu focal InRelease [265 kB]
Hit:5 http://10.10.10.203/ceph/debian-pacific focal InRelease
Fetched 265 kB in 1s (345 kB/s)
Reading package lists... Done
Building dependency tree
Reading state information... Done
74 packages can be upgraded. Run 'apt list --upgradable' to see them.
~~~

ref: [1](https://www.tecmint.com/setup-local-repositories-in-ubuntu/)|[2](https://linuxconfig.org/how-to-create-a-ubuntu-repository-server)|[3](https://help.ubuntu.com/community/Rsyncmirror)|[4](https://www.linuxtechi.com/setup-local-apt-repository-server-ubuntu/)


