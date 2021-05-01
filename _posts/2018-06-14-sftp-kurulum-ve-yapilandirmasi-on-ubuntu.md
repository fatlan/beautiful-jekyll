---
layout: post
title: SFTP Kurulum ve Yapılandırması on Ubuntu
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [sftp, linux, ssh, ftp]
comments: true
---
**SFTP** nedir.? : **Secure FTP**, yani **SFTP**, **SSH** kullanarak dosya transferi yapan bir dosya aktarım protokolüdür. **SSH**‘ın sağladığı güvenlik özellikleri, **FTP**‘den farklı olarak **SFTP**‘yi güvenli hale getirir. **FTP**‘nin **RSA** ile güçlendirilmiş halidir.

İlk olarak tabi **OpenSSH** paketi kurulu olması gerekiyor. Eğer kurulu değilse aşağıdaki komutla kurulum yapabilirsiniz.

~~~
sudo apt install openssh-server -y
~~~

Ardından **ftp** kullanıcıları için **grup** oluşturalım.

~~~
sudo addgroup ftpaccess
~~~

![Crepe](/assets/img/ubuntu16-sftp-iac/u-sftp-c01.png)

Daha sonra herhangi bir editör yardımıyla “**/etc/ssh/sshd_config**” dosyasındaki “**Subsystem sftp /usr/lib/openssh/sftp-server**” satırını başına **#** koyarak yorum satırına çevirip, ek olarak aşağıdaki satırları dosyanın sonuna ekleyip, kaydedip çıkın.

![Crepe](/assets/img/ubuntu16-sftp-iac/u-sftp-c02.png)

~~~
Subsystem sftp internal-sftp
Match group ftpaccess
ChrootDirectory %h
X11Forwarding no
AllowTcpForwarding no
ForceCommand internal-sftp
~~~

![Crepe](/assets/img/ubuntu16-sftp-iac/u-sftp-c03.png)

Şimdi **SSH** servisini yeniden başlatın

~~~
sudo systemctl restart sshd.service
~~~

Ardından sunucuya **login** olamayacak ve **ftpaccess** gruba dahil bir kullanıcı oluşturalım. Bunu aşağıdaki komutla gerçekleştirebilirsiniz.

~~~
sudo useradd -m USERNAME -s /usr/sbin/nologin -G ftpaccess
~~~

Şimdi bu kullanıcıya şifre belirleyelim.

~~~
sudo passwd USERNAME
~~~

“**/home**” dizininde **USERNAME** için oluşan klasörün sahipliğini **root** yapalım.

~~~
sudo chown root:root /home/USERNAME
~~~

Ardından oluşturulan kullanıcının **ftp** işlemleri yapabilmesi için kullanılacak bir dizin oluşturalım.

~~~
sudo mkdir /home/USERNAME/data
~~~

Dizini oluşturduktan sonra sahipliğini aşağıdaki gibi değiştirelim.

~~~
sudo chown USERNAME:ftpaccess /home/USERNAME/data
~~~

Son olarak dışardan **sftp** bağlantısı deneyerek ister **cli**, ister **gui** olarak test edelim. Ben **cli** kullanacağım, siz **remmina**, **winscp** gibi araçlarla **gui** olarak da test edebilirsiniz.

![Crepe](/assets/img/ubuntu16-sftp-iac/u-sftp-c04.png)
