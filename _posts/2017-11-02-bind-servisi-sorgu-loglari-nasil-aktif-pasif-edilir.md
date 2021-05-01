---
layout: post
title: BIND Servisi Sorgu Logları Nasıl Aktif-Pasif Edilir
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [bind, named, linux]
comments: true
---
Bunun için kontrol etmemiz gereken değişken **query logging** parametresidir. Yani bu parametre **On** ise **sorgu log**’u düşüyor, **O** ise düşmüyor.

İlk önce durumunu kontrol edelim. Bunun için çalıştıracağımız ilk komut **rndc status** komutudur. Ve bu komutu çalıştırıdığımızda aşağıdaki gibi** query logging is OFF** olduğunu görünüyoruz.

~~~
rndc status
~~~

![Crepe](assets/img/bind-na-ak-pasi/named-akpa01.png)

Aktif etmek için, yani log’ları alabilmek için **rndc querylog** komutunu çalıştırıyoruz.

~~~
rndc querylog yada (rndc querylog ON)
~~~

Tekrar **rndc status** ile durum kontrolü yapalım ve aşağıdaki gibi **ON** olduğunu görelim.

~~~
rndc status
~~~

![Crepe](assets/img/bind-na-ak-pasi/named-akpa02.png)

Tail komutu ile de çıktıyı alıp teyit edelim.

~~~
tail -f /var/log/messages
~~~

![Crepe](assets/img/bind-na-ak-pasi/named-akpa03.png)

**Burada tekrar OFF yapmak için rndc querylog yada (rndc querylog OFF)komutunu çalıştırabilirsiniz. Bu komutu her çalıştığında durumunu tersine çevirir. OFF ise ON, ON ise OFF yapar.**