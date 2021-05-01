---
layout: post
title: Linux Ubuntu/CentOS Server Üzerine Nginx Kurulumu
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [linux, centos, ubuntu, nginx]
comments: true
---
**Nginx nedir** : Rus yazılım mühendisi Igor Sysoev tarafından geliştirilen hafif, stabil, hızlı bir mail istemcisi olarak kodlanan daha sonraları geliştirilerek tüm sunucular için uygun hale getirilen bir web sunucusudur.

**Nginx** birçok amaç için kullanılabilir. Bunlar, ilk meydana çıkış sebebi olan **Mail** servisi, **Web** servisi, **Reverse Proxy**(ters vekil, bu özellik genelde web sayfalarını **cache**’lemek için kullanılır ve böylelikle sunucu yükünü azaltıp daha hızlı ve stabil çalışmasını sağlar) ve **Load Balancer** olarak sıralanabilir.

Sunucuya konsol yada **ssh** üzerinden **login**(root kullanıcısı değilseniz komutların başına “**sudo**” ekini eklemeyi unutmayın) olduktan sonra alttaki komutu çalıştırın.

~~~
sudo apt update
~~~

Daha sonra **Nginx** kurulumu için aşağıdaki komutu çalıştırın.

~~~
sudo apt install nginx
~~~

Şimdi **Nginx** kuruldu, aşağıdaki komut ile servisin durumunu kontrol edelim.

~~~
systemctl status nginx.service
~~~

![Crepe](/assets/img/c7u16-nginx-inst/nginx-int01.png)

Servisimiz çalışıyor ve etkin durumdadır.

Aksi durumlarda aşağıdaki komut setlerini kullanabilirsiniz. Nginx yönetim komutları.

~~~
sudo systemctl stop nginx.service           –> Nginx Servisini durdurur.

sudo systemctl start nginx.service          –> Nginx servisini başlatır.

sudo systemctl restart nginx.service        –> Nginx servisini yeniden başlatır.

sudo systemctl reload nginx.service         –> Nginx servisini yeniden yükler.

sudo systemctl enable nginx.service         –> Nginx servisini etkin duruma getirir.

sudo systemctl disable nginx.service        –> Nginx servisini devredışı bırakır.
~~~

Bu arada **Nginx**’i web server olarak çalıştırmak için sistem içerisinde Güvenlik duvarı etkin ise **firewall** yapılandırmasını yapmalısınız, **http(80)** ve çalışacaksa **https(443)** **port**’larını açmalısınız.

~~~
sudo ufw app list                           –> Güvenlik duvarında yapılandırılmış uygulamaları bu komutla görebilirsiniz.
sudo ufw allow/deny ‘Nginx HTTP’            –> Bu komutla güvenlik duvarında uygulamaya izin verebilirsiniz yada kapatabilirsiniz.
~~~

**Nginx** ayarlamaları için dosya ve dizinler aşağıdaki gibidir;

~~~
/var/www/html                               –> Web sayfalarının dosyalarının barındırıldığı default dizin yolu, isterseniz kendiniz belirlediği bir dizin yoluda belirleyebilirsiniz.
/etc/nginx                                  –> Nginx servisinin yapılandırması için kullanılan dosyaların tutulduğu temel dizin yoludur.
/etc/nginx/nginx.conf                       –> Nginx servisinin yapılandırma dosyasıdır.
/etc/nginx/sites-available/                 –> Her bir site için kullanılacak olan yapılandırma dosyalarının saklandığı dizindir.
/etc/nginx/sites-enabled/                   –> Her bir site için kullanılacak olan yapılandırma dosyalarının çalışması(etkin olması) için “**/etc/nginx/sites-available/**” den buraya link’leneceği dizin yoludur.
/etc/nginx/snippets                         –> Nginx servisi için farklı yapılandırmalar için kullanılabilecek yapılandırma dosyasıdır.
~~~

**Nginx Log**’larının tutulduğu dizin ve dosyalar da aşağıdaki gibidir;

~~~
/var/log/nginx/access.log                   –> Tüm isteklerin log’landığı dosyadır.
/var/log/nginx/error.log                    –> Tüm hataların log’landığı dosyadır.
~~~

Ayrıca bu link’i inceleyebilirsiniz [https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-16-04](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-16-04) <br>

Evet, **Nginx** servisinin ne olduğunu anladık ve kurulumu gerçekleştirmeyi öğrendik. Bundan sonra amaca yönelik olarak yapılandırıp kullanmak kalıyor. Örneğin site ekleme, yapılandırıp, aktif çalışmasını sağlama gibi, bir sonraki makalelerde yeri geldikçe bahsedeceğiz.