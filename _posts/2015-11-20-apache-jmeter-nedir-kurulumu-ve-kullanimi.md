---
layout: post
title: Apache JMeter Nedir Kurulumu ve Kullanımı
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [apache, java]
comments: true
---
Apache JMeter uygulaması yapısal olarak java ile kodlanmış website ve servisleri test, davranış ölçümü ve performans ölçümü gibi sonuçları gözlemlemek için kullanılan ayrıca sonuçları grafiğe dökebilen yetenekli bir stress yazılımdır. Uygulama hakkında detaylı bilgiye [https://jmeter.apache.org/](https://jmeter.apache.org/) adresinden ulaşabilirsiniz. Kurulumuna geçelim, ben Windows 10 üzerinden göstereceğim. Linux üzerinden belki ilerde bir makale ile bahsedebiliriz ki ubuntu üzerinde yazılım yükleme merkezinden bu işlem çok basit bir şekilde yapılabilir.
Tabi ilk önce yazılımı Windows ortamında çalıştırabilmek için Java kurulumu gerekmektedir.
Java’nın sitesine [https://java.com/tr/download/](https://java.com/tr/download/) bağlanıp kuruluma başlayalım. Ücretsiz Java İndirme linkine tıklayın.

![Crepe](assets/img/ajmeter/jmtr01.png)

Ardından Butonu tıklayın.

![Crepe](assets/img/ajmeter/jmtr02.png)

Daha sonra uygulamayı tıklayarak kuruluma başlayın ve ardından **Install** tıklayın.

![Crepe](assets/img/ajmeter/jmtr03.png)

Kurulum yapılıyor.

![Crepe](assets/img/ajmeter/jmtr04.png)

**Close** diyerek bitirin.

![Crepe](assets/img/ajmeter/jmtr05.png)

Ardından otomatik olarak tarayıcıda doğrulama sayfasına yönlendirecek.

![Crepe](assets/img/ajmeter/jmtr06.png)

**Run** tıklayın.

![Crepe](assets/img/ajmeter/jmtr07.png)

Ve Java sürümü doğrulandı.

![Crepe](assets/img/ajmeter/jmtr08.png)

Daha sonra Apache JMeter indirme sayfasına bağlanın [http://jmeter.apache.org/download_jmeter.cgi](http://jmeter.apache.org/download_jmeter.cgi) ve Windows için *.zip uzantılı olanı indirin.

![Crepe](assets/img/ajmeter/jmtr09.png)

Ardından inen dosyayı istediğiniz lokasyona zipten çıkartın.
Klasörü açıp **\apache-jmeter-2.13\apache-jmeter-2.13\bin** içine girin. **ApacheJMeter.jar** dosyası görünümü aşağıdaki gibiyse buraya kadar tüm işlemler başarılı olduğunu anlıyoruz ve o dosyayı çalıştırarak programın GUI arayüzüne erişebiliriz.

![Crepe](assets/img/ajmeter/jmtr10.png)

![Crepe](assets/img/ajmeter/jmtr11.png)

Hemen kısaca **http request** olarak kullanımına geçelim. Bu performans testleri ile önceden belirlenen performans hede erine ulaşabildiniz mi acaba? gibisinden servis alt yapısı ve kaynaklarının yeterliliğini(banwith-bant genişliği, donanım kaynakları yeterliliği vs.) max. derecede ölçmek için kullanabilirsiniz. Belirlediğimiz websitenin aynı anda kaç kullanıcıya hizmet verebildiğini ölçebilirsiniz. Ben 20 kullanıcı olarak test gerçekleştireceğim. User belirleyebilmek için yeni bir test planı grubu oluşturalım. **Test Plan** sağ click **Add-Threads(Users)-Thread** Group tıklayın.

![Crepe](assets/img/ajmeter/jmtr12.png)

Değişikliklerimi yaptım. **Name** kısmına isim verdim 20 user olarak belirledim, 5 saniyede bir sorgu gönderttim ayrıca **loop count** kısmını işlemin sürekli devam etmesi, tekrarlanması(sonsuz döngü gibi yani 20 kullanıcı sorgusu bittiğinde bir daha bir daha ve siz durdurana kadar bir daha sorgu gönderecek) için **Forever** işaretledim, siz isterseniz sayıyla belirtebilirsiniz.

![Crepe](assets/img/ajmeter/jmtr13.png)

Ardından Websiteyi belirlemek adına **HTTP Request Defaults** ekliyorum.

![Crepe](assets/img/ajmeter/jmtr14.png)

80 portundan istek göndereceğim.

![Crepe](assets/img/ajmeter/jmtr15.png)

Ardından http isteği için **HTTP Request** ekliyorum.

![Crepe](assets/img/ajmeter/jmtr16.png)

Sitedeki **path**’leri buraya ekleyebilirsiniz.

![Crepe](assets/img/ajmeter/jmtr17.png)

Ardından istek detaylarını görebilmek için **View Results** in **Table** ve **Tree**’yi ekliyorum.

![Crepe](assets/img/ajmeter/jmtr18.png)

Ardından işlemleri grafiksel olarak elde etmek için **Response Time Graph** ekliyorum.

![Crepe](assets/img/ajmeter/jmtr19.png)

Görüntü aşağıdaki gibidir.

![Crepe](assets/img/ajmeter/jmtr20.png)

Ardından tüm bu işlemlerin **config** dosyasını bin altına kaydediyorum.

![Crepe](assets/img/ajmeter/jmtr21.png)

![Crepe](assets/img/ajmeter/jmtr22.png)

Ardından tüm işlemler bittikten sonra testi başlatıyorum.

![Crepe](assets/img/ajmeter/jmtr23.png)

Daha sonrasında eklediğiniz dinleyicilerden sonuçlara ulaşabilirsiniz.
Kullanım Klavuzu detayları [https://jmeter.apache.org/usermanual/index.html](http://jmeter.apache.org/usermanual/index.html) <br>

