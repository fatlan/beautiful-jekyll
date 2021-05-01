---
layout: post
title: NETCAT - NC kullanarak veri transferi sağlamak
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [netcat, network, linux]
comments: true
---
![Crepe](assets/img/netcat-veri-trans/netcat-tran01.png)

**Netcat** ile iki bilgisayarlar arası veri transferi nasıl yapılır ona bakacağız. Bu işlemi makineler arasında **port** açarak gerçekleştireceğiz.
Alıcı bilgisayar(**receive**)’da aşağıdaki komut çalıştırılarak **port**(**3434**) aktif edilip **dosyam.txt** için tünel açılır.

~~~
nc -l -p 3434 > dosyam.txt
~~~

Gönderici(**sender**) makinede’de aşağıdaki komut çalıştırılarak **dosyam.txt** aktarılır.

~~~
nc 192.168.1.12 3434 < dosyam.txt
~~~