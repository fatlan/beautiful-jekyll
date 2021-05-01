---
layout: post
title: Linux DNS - BİND Versiyon Bilgisini Kapatma yada Özelleştirme
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [linux, dns, bind]
comments: true
---
![Crepe](assets/img/bind-verinf-ch/dns-vi-ch01.png)

Hacker mantalitesinin ilk adımlarından olan, servis/servisleri keşfetmek, bu keş yaparken versiyon bilgisini almaktır. Daha sonra bu versiyonların barındırdıkları zafiyetleri kullanarak içeri sızmaktır.

Doğal olarak Linux üzerinde kullanılacak olan **Dns** servisi **BİND-NAMED** servisinin versiyon bilgisini gizlemek ya da özelleştirmek yapılacak olan mantıklı hareketlerden biri olacaktır.

Versiyon bilgisini aşağıdaki 3 yöntem ile elde edebilirsiniz.

1.**NMAP**

~~~
nmap -sV -p 53 fatlan.com
~~~

2.**DIG**

~~~
dig @fatlan.com version.bind txt chaos
~~~

3.**NSLOOKUP**

~~~
nslookup -type=txt -class=chaos version.bind fatlan.com
~~~

Sıra geldi versiyon bilgisini gizlemeye. Bunun için “**/etc/named.conf**” dosyası içindeki “**option**” kısmındaki satırlara ek olarak aşağıdaki satırları da ekleyin. Ek olarak iserseniz **hostname** bilgisini de ekleyebilirsiniz.

~~~
options {
    version “DNS SERVER 2018”;
    hostname “DNS SERVER 2018”;
};
~~~