---
layout: post
title: Komut Satırı Üzerinden Gerçek(Public) Ip Öğrenme - Linux/Windows
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [ip, public, real, linux, windows]
comments: true
---
![Crepe](/assets/img/cli-public-ip-chk/cli-pub-ip-chk01.png)

Sistemler üzerinde (projeye ve ortama göre değişiklik gösterir) çoğunlukla **local ip**’ler ile çalışırız ve bunları **network** ayarlarından yada komut satırı üzerinden çeşitli komutlarla elde edebiliriz. Fakat gerçek ip’ler ile işlem yapmak için **router/modem** arayüz yada **online** olarak hizmet sunan (birden fazla site mevcuttur) **url** aracılığı ile **real ip**’yi elde edip daha sonra işlem yaparız. Yukarıda da **NCC** olan **ripe.net** örnek olarak verilmiştir. Girişte sağ tarafta yukarda sizin çıkış ip’nizi yansıtan bir bölüm bulunmaktadır.

Şimdi ise komut satırı üzerinden gerçek ip’mizi nasıl elde ederiz ona bir bakalım. **Linux**’ta üç alternatif komut, **Windows**’ta ise bir komut kullanacağım

**Linux Dig** komutu ile;

~~~
dig +short myip.opendns.com @resolver1.opendns.com
~~~

![Crepe](/assets/img/cli-public-ip-chk/cli-pub-ip-chk02.png)

**Linux Curl** komutu ile;

~~~
curl ipinfo.io/ip
~~~

![Crepe](/assets/img/cli-public-ip-chk/cli-pub-ip-chk03.png)

**Windows**’ta **Nslookup** ile; (**Nslookup** komutunu **Linux** için de kullanabilirsiniz.)

~~~
nslookup myip.opendns.com. resolver1.opendns.com
~~~

![Crepe](/assets/img/cli-public-ip-chk/cli-pub-ip-chk04.png)
