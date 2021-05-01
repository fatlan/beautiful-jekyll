---
layout: post
title: Ubuntu/Debian Sunucular için internet bağlantısı (INTRANET) olmadan, OFFLINE olarak APT/APT-GET kullanarak paket ya da servis kurma işlemleri
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [ubuntu, debian, offline, apt, intranet]
comments: true
---
Bu işi yapabilmek için en temel olarak elinizde servise ait paketin ve o pakete ait bağımlı paketlerin(**dependency**) hepsinin olması gerekiyor. Tabi bu bir işlem. İkinci aşama olarak elinizde paketler mevcut ise **dpkg** komutu ile kurulum sağlayabilirsiniz ama herhangi bir sebepten ötürü apt paket yöneticisine kullanarak kurmanız gerekiyorsa buda mümkün.

Bunun için ilk olarak internetli bir makineye ihtiyacımız var. Bu aşamada kurulum yapacağımız ilgili servis için paketi ve paket bağımlılıklarını(**dependency**) bulup, ilgili paketleri toparlamamız gerekiyor. Beraberinde **apt**’yi kullanabilmek için bir iki işlem yapacağız.

Neler yapacağız:


İlk olarak **internet**’li makinede;

1. Eğer kuracağımız paket varsayılan(**default**) olarak ubuntu ya da debian repo’sunda yoksa paket repo’su “**/etc/apt/source.list**” ya da **source.list.d** dizini altına eklenmelidir.
2. “**apt update/apt-get update**” komutu ile **source list index**’ini güncelleyeceğiz. İlgili dizin yolu(**directory**) **/var/lib/apt/lists/** altında yer almaktadır.
3. Eğer ilgili servise ait (örnek servis için nginx kullanacağız) tüm paketleri bağımlılıklarıyla beraber görmek isterseniz “**apt depends nginx**” komutunu kullanabilirsiniz.
4. Şimdi “**apt-get -qq -print-uris install nginx > nginx-paket-list**” komutu ile **nginx** servisine ait bağımlı paketlerle beraber tüm paketlerin listesini “**nginx-paket-list**” dosyasına yazdırdık.

    NoT: “**apt-get -qq -print-uris upgrade > upgrade-paket-list**” komutu ile **offline** olarak makineleri yükseltme işlemi bile yapabilirsiniz.
    
5. Şimdi yukarıdaki listeden de anlaşılacağı üzere listedeki paketleri ilgili linklerden otomatik olarak tek seferde indirebilmek için “**awk ‘{print “wget ” $1}’ < nginx-paket-list > nginx-paket-list.sh**” komutu çalıştırıyoruz.
6. Oluşturduğumuz dosyaya “**chmod u+x nginx-paket-list.sh**” komutla çalıştırma yetkisi verelim.
7. Son olarak sh dosyasının “**./nginx-paket-list.sh**” çalıştıralım. Ve tüm paketlerin indiğini görebilirsiniz.

![Crepe](assets/img/ub-deb-offline-inst/udoffinst01.png)

İkinci olarak internetsiz (**INTRANET**) makinede, **OFFLINE** olarak **apt** ile kurulum yapabilmek için taşıma işlemleri yapacağız. **İnternetli makineden, internetsiz makineye doğru** herhangi bir şekilde(**usb**, **kopyalama**, **cd** **vs**);

1. İnternetli makinede güncellediğimiz **source list index**’ini, internetsiz makinede ki “**/var/lib/apt/lists/**” altına taşıyoruz.
2. İnternetli makinedeki “**/etc/apt/source.list**” ve eklediyeseniz **source.list.d** dizini aynen internetsiz makineye taşıyorsunuz.
3. İnternetli makinede indirdiğiniz tüm ***.deb** paketlerini, internetsiz makinede ki “**/var/cache/apt/archives/**” dizini altına gönderiyorsunuz.

Daha sonra internet olamayan makinede yapmanız gereken tek şey “apt install nginx” işlem tamam. Böylelikle offline olarak apt’yi kullanarak paket yükleyip, servisleri aktif edebilirsiniz. Hatta makineleri yükseltebilirsiniz.

**DPKG** ile kurulum;

Hazır paketleri indirmişim, ne gerek var **apt**’ye bu kadar çileye derseniz, sadece paketleri taşıyıp “**dpkg -i** * ya da tüm_paket_isimleri” demeniz yeterli. =)


Referans: [debian.org.tr](https://www.debian.org.tr/Offline_Apt) <br>