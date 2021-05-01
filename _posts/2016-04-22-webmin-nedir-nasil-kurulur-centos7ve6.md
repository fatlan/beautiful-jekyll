---
layout: post
title: Webmin nedir, nasıl kurulur - CentOS 7 ve 6
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [webmin, centos, linux, panel]
comments: true
---
Kısaca **Webmin**’den bahsetmek gerekirse CentOS üzerinde çalışan servis ve uygulamaları web arayüz bağlantısı aracılığı ile yönetmenize yarayan küçük ve basit bir uygulamadır.

Kurulumu ise; ilk önce **wget** aracılığı ile uygulamayı indirelim. Uygulamanın güncel versiyonuna resmi sitesinden [http://webmin.com/download.html](http://webmin.com/download.html) ulaşabilirsiniz.

~~~
wget http://prdownloads.sourceforge.net/webadmin/webmin-1.791-1.noarch.rpm
~~~

Daha sonra aşağıdaki ‘**rpm -U webmin-1.791-1.noarch.rpm**’ komut ile kurulumu yapıp sonlandıralım. Fakat şöyle ki ben **CentOS7** üzerinde kurulum gerçekleştiriyorum. O yüzden bu ’**rpm -U webmin-1.791-1.noarch.rpm**’ komutu direk çalıştırırsam hata dönecektir ‘**Header V3 DSA/SHA1 Signature key id 11f63c51 NOKEY**‘ ve hatanın screenshot çıktısı aşağıdaki gibidir.

![Crepe](assets/img/centos6-7-webmin/webmin01.png)

**CentOS7**’de bu hatayı almamak için aşağıdaki komutu çalıştırıyorum.

~~~
yum -y install perl perl-Net-SSLeay openssl perl-IO-Tty
~~~

Ardından aşağıdaki komut ile kurulumu yapıp bitirebilirsiniz. CentOS 6 ve öncesi için aşağıdaki komutu yazarsanız direk kurulumu yapacaktır.

~~~
rpm -U webmin-1.791-1.noarch.rpm
~~~

Ve kurulum tamamlandı. **/usr/libexec/webmin** dizininde klasör oluşacaktır. Gerekli işlemleri buradan yapabilirsiniz. **Default** olarak **user** ve **password** sunucu **login** bilgilerinizdir ve **https://SunucuIP:10000** ile **Webmin**’in arayüzüne bağlanabilirsiniz. Fakat **internal** yada **external** firewall kullanıyorsanız** 10000 port**una izin vermelisiniz.

**CentOS** internal firewall **10000 port**una izin vermek için (**iptables -I INPUT -p tcp --dport 10000 -j ACCEPT yada rewall-cmd --add-port=10000/tcp**) komutlarını çalıştırabilirsiniz.

![Crepe](assets/img/centos6-7-webmin/webmin02.png)

![Crepe](assets/img/centos6-7-webmin/webmin03.png)
