---
layout: post
title: İnternet Hız Değeri ve Anlamı
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [network, internet]
comments: true
---

![Crepe](/assets/img/net-sp/net-sp01.png)

İlk önce BYTE(bayt) birimlerine bakalım

~~~
Bit
Byte
Kilobyte
Megabyte
Gigabyte
Terabyte
Petabyte
Eksabyte
Zettabyte
Yottabyte
~~~

**Not**: **b=bit B=byte** anlamına gelmektedir, ifade edilirken ona göre ifade edilir. Yani küçük **b** ile büyük **B** arasında anlamlandırırken böyle bir fark vardır.

Kapasite ölçümleri aşağıdaki gibidir;

~~~
8bit=1Byte
1024Byte=1KB
1024KB=1MB
1024MB=1GB
1024Gb=1TB
1024TB=1PB
1024PB=1EB
1024EB=1ZB
1024ZB=1YB
~~~

İnternet hızı Kbps(Kilo bit per second) yada Mbps(Mega bit per second) yada Gbps(Giga bit per second) diye anlandırılır. Yani saniyedeki kilobit yada megabit yada gigabit değeridir.
1Mbps’deki net değeri hesaplanırken;
1Mbps = 1/8bit ile MB cinsinden sonuç çıkar 0,125 MB/s’tir. Sıfırdan kurtulmak va daha anlaşılır bir değer elde etmek için 1024KB ile çarpılır(1/8*1024), buna göre 1 Mbps net hızı saniyede max 128KB/s değere ulaşır.

~~~
2Mbps = (2/8)*1024= 256KB/s
4Mbps = (4/8)*1024 = 512KB/s
6Mbps = (6/8)*1024 = 768KB/s
8mbps = 8/8 = 1MB/s (Sonuç ilk örnekte olduğu gibi MB cinsinden olduğu için tam sayı çıkacaktır.Bir daha 1024 ile çarpmaya gerek yok. KB’ye çevirmek isterseniz çarpabilirsiniz.)
16Mbps = 16/8 = 2MB/s
32Mbps = 32/8 = 4MB/s
64Mbps = 64/8 = 8MB/s
100Mbps = 100/8 = 12,5MB’tır. Teorikte ki yaklaşık hesaplar bu şekildedir.
~~~

Pratikte hattın yoğunluğu, alt yapı kalitesi, santrallere olan uzaklık, yapılan temel kon gurasyonlar(adil kota, asimetrik hat gibi) vs. işin içine girince değişmeler gözlemlenebilir.
Kaldıki ADSL gibi asimetrik hatlar da upload(veri gönderme), Download(veri indirme) birbirine etki eder, yani birbirinden bağımsız değildir. Örneğin içerdeki tüm çalışanlar aynı anda dışarıya veri gönderdiğinde upload hattı sature eder, buda direk download’a etki eder, o zaman Mbps cinsinden hattınız pek bişey ifade etmez. Zaten asimetrik hatlarda, örneğin aldığınız 8Mbps indirme hızı size olduğu gibi yansımaz. Fakat noktadan noktaya(P2P) gibi simetrik hatlarda Upload ve Download birbirinden bağımsız olup, hat sature olmaz ayrıca aldığınız hızdaki hizmet size olduğu gibi yansır, tam kapasite ile çalışırsınız.

