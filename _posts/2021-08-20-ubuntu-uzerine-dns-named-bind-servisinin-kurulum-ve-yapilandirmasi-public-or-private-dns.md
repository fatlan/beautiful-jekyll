---
layout: post
title: Ubuntu Üzerine DNS(Named - Bind) Servisinin Kurulum ve Yapılandırması - Public or Private DNS
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/25-26-20-uelinuxegitim/25-26-20le05.png
#share-img: /assets/img/path.jpg
tags: [dns, bind, named, linux, ubuntu]
comments: true
---

~~~
sudo apt install -y bind9 bind9utils bind9-doc dnsutils
~~~

Yapılandırma dosyaları **/etc/bind/** altındadır.

Oluşturacağımız **forward zone** ve **reverse zone** bu dosya içerisinde belirtiyoruz **/etc/bind/named.conf.local**

~~~
cd /etc/bind
~~~

~~~
sudo vi named.conf.options
~~~
Aşağıdaki yapılandırma satırlarını ortamınıza göre şekillendirebilirsiniz.
~~~

        allow-query { any; };
        listen-on port 53 { any; };
        allow-transfer { none; };
        recursion yes;
        querylog yes;
        allow-recursion { any; };
        version "Ubuntu DNS Server for fatlan.com";
        dnssec-validation auto;
        listen-on-v6 { any; };
        forwarders {
            8.8.8.8;
        };

~~~

~~~
sudo vi named.conf.local
~~~
~~~
zone "fatlan.com" IN {

     type master;

     file "/etc/bind/fatlan.com";

     allow-update { none; };
};


zone "0.0.127.in-addr.arpa" IN {

     type master;

     file "/etc/bind/reverse.fatlan.com";

     allow-update { none; };
};
~~~

Şimdi **fatlan.com** ve  **reverse.fatlan.com** dosyalarını oluşturalım.

~~~
sudo cp db.local  fatlan.com

sudo cp db.127  reverse.fatlan.com
~~~

~~~
sudo vi fatlan.com
~~~

127.0.0.1 no'lu ip'ler yerine sizin için ilgili ip'yi girmeniz önerilir.
~~~
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     ns.fatlan.com. root.ns.fatlan.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      ns.fatlan.com.
@       IN      A       127.0.0.1
@       IN      AAAA    ::1
ns      IN      A       127.0.0.1
~~~

~~~
sudo vi reverse.fatlan.com
~~~

127.0.0.1 no'lu ip'ler yerine sizin için ilgili ip'yi girmeniz önerilir.
~~~
;
; BIND reverse data file for local loopback interface
;
$TTL    604800
@       IN      SOA     ns.fatlan.com. root.ns.fatlan.com. (
                              1         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      ns.fatlan.com.
NS      IN      A       127.0.0.1
1       IN      PTR     ns.fatlan.com.
~~~

~~~
sudo named-checkconf
~~~
~~~
sudo named-checkzone fatlan.com /etc/bind/fatlan.com
~~~
~~~
sudo named-checkzone reverse.fatlan.com /etc/bind/reverse.fatlan.com
~~~

![Crepe](/assets/img/ubun-nmdbind/dns-nmdchck01.png)

~~~
sudo systemctl start bind9.service
~~~

Şimdi dns server'ımı /etc/resolv.cond dan bu dns gösterip aşağıdaki testleri yapıyorum.

![Crepe](/assets/img/ubun-nmdbind/dns-nmdchck02.png)

ilgili portları tespit için aşağıdaki komutları kullanabilirsiniz.

~~~
sudo ss -plnta | egrep -i named

sudo netstat -plnta | egrep -i named
~~~

**NoT:** Her kayıt eklendiğinde mutlaka **Serial** kısmı arttırılmalıdır.

**fatlan.com** için örnek kayıtlar...
~~~
fatlan.com.   IN     MX   10   mail.fatlan.com.

www     IN       A      127.0.0.1
mail    IN       A      127.0.0.1

ftp     IN      CNAME   www.fatlan.com.
~~~

**reverse.fatlan.com** için örnek kayıtlar...
~~~
3     IN      PTR    www.fatlan.com.
4     IN      PTR    mail.fatlan.com.
~~~

ref: https://ubuntu.com/server/docs/service-domain-name-service-dns


