---
layout: post
title: Redhat/Centos TimeZone Değiştirme
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [redhat, centos, linux]
comments: true
---
İlk önce “**date**” komutunu çalıştırıp, şuan ki saat bilgilerimizi görelim.

![Crepe](assets/img/redhat-timezone-c/red-tzc01.png)

İlk önce “**cp**” komutu ile “**localtime**” dosyasının yedeğini alalım.

~~~
cp /etc/localtime /etc/localtime.bak
~~~

Ardından dosyayı silelim

~~~
rm /etc/localtime
~~~

Ya da yukarıda olduğu gibi iki komut yerine aşağıdaki gibi “**mv**” komutuyla tek seferde işi çözebilirsiniz.

~~~
mv /etc/localtime /etc/localtime.bak
~~~

Şimdi “**ln -s**” komutu ile Türkiye’ye uygun olan timezone dosya kaynağını **localtime** dosyasına linkleyelim.

~~~
ln -s /usr/share/zoneinfo/Europa/Istanbul /etc/localtime
~~~

Son olarak “**date**” komutunu tekrar çalıştırıp, Türkiye **timezone**’nin geldiğini görelim.

![Crepe](assets/img/redhat-timezone-c/red-tzc02.png)
