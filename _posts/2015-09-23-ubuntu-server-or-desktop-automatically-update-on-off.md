---
layout: post
title: Ubuntu (Server or Desktop) Automatically Update On_Off
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [ubuntu, linux]
comments: true
---
Ubuntu otomatik update açıp/kapatmanın bir kaç yolundan bahsedeceğim.
İlk olarak katılımsız(kullanıcı müdahalesi olmadan, kendiliğinden yüklenen otomatik kurulumlardır.) yükseltme metodundan bahsedelim. Buraya genelde kullanıcı müdahelesi gerekmemektedir, daha çok uzman mod gibi desek yanlış olmaz. Ama biz genede bilgi olması
için bahsedelim. Sistem root ile login olduktan sonra(eğer **root** yetkisine sahip olmayan başka bir user ile login olduysanız komutların başına ‘**sudo**’ ekini ekleyin.) aşağıdaki komutu çalıştırın. Ben editör olarak vi komutunu kullandım, siz isterseniz başka bir favori komutunuzu(nano, vim gibi) kullanabilirsiniz.

~~~
vi /etc/apt/apt.conf.d/50unattended-upgrades
~~~

Çıktı aşağıdaki gibi olacaktır. Güvenlikle alakalı olanı hariç diğerleri devre dışıdır. Ayrıca bazı paketler karalisteye alınarak devredışı bırakılabilir(Sistem // işaretini yorum satırı olarak kabul edecektir.)

![Crepe](/assets/img/uauonoff/uauonoff01.png)

İkinci olarak işletim sistemi seviyesinde kullanıcıların müdahele edebileceği method olarak aşağıdaki komutu çalıştırın.

~~~
vi /etc/apt/apt.conf.d/10periodic
~~~

Çıktı aşağıdaki gibi olacaktır. Burada dikkat edilmesi gereken husus;
**APT::Periodic::Update-Package-Lists “1”** -> Otomatık Update Açık
**APT::Periodic::Update-Package-Lists “0”** -> Otomatık Update Kapalı yaptıktan sonra kaydedip çıkın ve işlem bu kadar.

![Crepe](/assets/img/uauonoff/uauonoff02.png)

Üçüncü yöntem ise **GUI(Grafik arayüzü)** ile nasıl yapılır bakalım ve bitirelim. Sisteme **GUI** ile login olduktan sonra, **System Settings** menüsüne girin ve **Software & Update** butonuna tıklayın.

Ardından **Update** tabından **Automatically check for updates** kısmını **Never** yapın.

![Crepe](/assets/img/uauonoff/uauonoff03.png)

Şifre bilgisini girdikten sonra, **Close** deyip çıkın ve işlem bu kadar.

![Crepe](/assets/img/uauonoff/uauonoff04.png)

![Crepe](/assets/img/uauonoff/uauonoff05.png)

![Crepe](/assets/img/uauonoff/uauonoff06.png)
