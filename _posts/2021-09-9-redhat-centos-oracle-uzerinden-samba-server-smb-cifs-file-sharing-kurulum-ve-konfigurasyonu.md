---
layout: post
title: Redhat/CentOS/Oracle Üzerinden Samba Server(SMB/CIFS File Sharing) Kurulum ve Konfigürasyonu
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/25-26-20-uelinuxegitim/25-26-20le05.png
#share-img: /assets/img/path.jpg
tags: [samba, smb, cifs, linux, Redhat, CentOS, Oracle]
comments: true
---

Samba(**SMB/CIFS**) ağ üzerinden farklı işletim sistemleri arası dosya paylaşımına olanak tanıyan dosya paylaşım sistemidir. Bu klavuzda Linux üzerinde Samba server aracılığıyla paylaşılan belgelere diğer işletim sistemlerinin erişimi amaçlanmıştır.

**SUNUCU Tarafı**;

Servis kurulur
~~~
yum install samba -y
~~~

Servisler çalıştırılır
~~~
systemctl start smb.service
systemctl start nmb.service

systemctl enable smb.service
systemctl enable nmb.service
~~~

Kullanıcı oluşturalım. Aşağıdaki komut kullanıcının **home** dizinini de oluşturulur ve ben orayı kullanacağım. Ayrıca **/home** dizini kullanmak durumunda da değilsiniz. Kullanıcıları ayrı ayrı yönetmek açısından faydalı olabilir ama farklı dizin kullanırsanız tüm kullanıcıları o dizine de yönlendirebilirsiniz(hatta aşağıda göreceksiniz **valid user** kısmına **@group** ismide yapabilirsiniz sonra dahil edeceğiniz kullanıcıları ikincil grup olarak bu gruba ekleyebilirsiniz), kaldı ki orda da ayrı ayrı yönetebilirsiniz ve daha uygun da olabilir. Bu durumda(**/samba** dizini ele alınmıştır) ikinci satırdaki komutu çalıştırmanız daha uygun olur.

/home dizinini kullanmak için;
~~~
useradd -s /sbin/nologin winuser
~~~

Oluşturduğunuz farklı dizini kullanmak için;
~~~
useradd -M -d /samba/winuser -s /usr/sbin/nologin winuser
~~~

Samba bağlantısı için şifre atayalım, bilgiler “**/var/lib/samba**” altında saklanır.
~~~
smbpasswd -a winuser
~~~

ilgili dizine örnek bir paylaşacağım dosya oluşturuyorum
~~~
echo "merhaba smb/cifs paylaşımım" > /home/winuser/pyls.txt
~~~

Bu arada **SELinux** varsayılanda **home** dizin paylaşımını engelliyor, dosyaların haricinde **SELinux** açısından servisilerin de **boolen** tipi etiketleri var bu durumu aşağıdaki komutla gözlemleyebilirsiniz.
~~~
getsebool -a | egrep -i samba
~~~

Aşağıdaki komutla da aktif edebilirsiniz ev tekrar yukarıda komutla on olduğunu gözlemleyebilirsiniz.
~~~
setsebool -P samba_enable_home_dirs on
~~~

Daha sonrasında “**/etc/samba/smb.conf**”  dosyasına aşağıdaki gibi yapılandırıyoruz.
~~~
[global]
...
server min protocol = LANMAN1
...


[winuser]
        comment = Redhat Samba Server
        path = /home/winuser/
        valid users = winuser
        browseable = Yes
        writable = Yes
        read only = No
        inherit acls = Yes
~~~

**NoT:** Belli dosyaların yüklenmesini engellemek(kısıtlamak), ip bloklarını kısıtlamak yada izin vermek için aşağıdaki parametreleri ekleyebilirsiniz.
~~~
#tüm paylaşımlar için global,
[global]
veto files = /*.avi/*.mp3/
delete veto files = yes

#belirli paylaşımlar için,
[share]
path = /path/share
veto files = /*.avi/*.mp3/
delete veto files = yes

#ip blokları kısıtlamak ya da izin vermek
[share]
  hosts allow = 172.16.24.0/24
  hosts deny = 172.16.25.0/24

#disable anonymous and guest
[share]
  restrict anonymous = 2
  usershare allow guests = no
~~~

Servisler yeniden başlatılır
~~~
systemctl restart smb.service
systemctl restart nmb.service
~~~

Ayrıca local **firewall** aktifse
~~~
firewall-cmd --zone=public --add-service=samba --permanent
~~~

**Check smb.conf**
~~~
testparm
~~~

**Current Samba connection**
~~~
smbstatus
~~~


**KULLANICI** Tarafı;


**Linux üzerinden**,

İlk etapta aşağıdaki paket kurulmalıdır.
~~~
yum install samba-client -y
~~~

Bağlantı testi için,
~~~
smbclient -L 10.10.10.96 -U winuser
~~~

Smbclient ile login olup gözlemleyelim.
~~~
smbclient //ip_address/shared -U smb_user%smb_pass

smbclient //10.10.10.96/winuser -U winuser%winuser
~~~

~~~
smb: \> ?
?              allinfo        altname        archive        backup         
blocksize      cancel         case_sensitive cd             chmod          
chown          close          del            deltree        dir            
du             echo           exit           get            getfacl        
geteas         hardlink       help           history        iosize         
lcd            link           lock           lowercase      ls             
l              mask           md             mget           mkdir          
more           mput           newer          notify         open           
posix          posix_encrypt  posix_open     posix_mkdir    posix_rmdir    
posix_unlink   posix_whoami   print          prompt         put            
pwd            q              queue          quit           readlink       
rd             recurse        reget          rename         reput          
rm             rmdir          showacls       setea          setmode        
scopy          stat           symlink        tar            tarmode        
timeout        translate      unlock         volume         vuid           
wdel           logon          listconnect    showconnect    tcon           
tdis           tid            utimes         logoff         ..             
!
smb: \> l
  .                                   D        0  Sun Aug 22 16:34:48 2021
  ..                                  D        0  Sun Aug 22 16:30:27 2021
  .bash_logout                        H       18  Thu May 27 18:09:35 2021
  .bash_profile                       H      141  Thu May 27 18:09:35 2021
  .bashrc                             H      376  Thu May 27 18:09:35 2021
  pyls.txt                            N       24  Sun Aug 22 16:34:48 2021
~~~

Aşağıdaki gibi mount işlemini yapabilirsiniz.
~~~
mount -t cifs -o username=winuser,pass=winuser //10.10.10.96/winuser /mnt
~~~

Aşağıdaki komutla da son mount durumlarını gözlemleyebilirsiniz.
~~~
df -h

mount
~~~

Mount işleminin kalıcı hale gelebilmesi için /etc/fstab içine yazmayı unutmayın. Dilerseniz user, pass bilgilerini dosyadan da okutabilirsiniz. İlgili [link](https://askubuntu.com/questions/157128/proper-fstab-entry-to-mount-a-samba-share-on-boot) size yardımcı olacaktır.
~~~
//10.10.10.96/winuser                      /mnt                    cifs 	username=winuser,pass=winuser	0 0
~~~

Ayrıca “**smb://10.10.10.96/winuser**” ile de hem **TUI** hem de **GUI** bağlanabilirsiniz.


**Windows** üzerinden,

Network – Map network drive...

![Crepe](/assets/img/smb-c8/smb-c801.png)

![Crepe](/assets/img/smb-c8/smb-c802.png)



ref:

[computernetworkingnotes](https://www.computernetworkingnotes.com/linux-tutorials/how-to-configure-samba-server-in-redhat-linux.html) | [ubuntu](https://ubuntu.com/tutorials/install-and-configure-samba#1-overview)

