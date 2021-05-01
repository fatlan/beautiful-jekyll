---
layout: post
title: Windows Server 2012R2 Active Directory (Domain Controller – DC) Kurulumu
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [windows, dc, server]
comments: true
---
Daha sonra aşağıda da görüldüğü gibi start tuşuna basıp dcpromo yada **Windows+R** tuş kombinasyonuna bastıktan sonra **dcpromo** yazarak kurulumu başlatalım.

![Crepe](assets/img/ws2012ins/ws12in01.png)

Ve aşağıdaki hatanı geldiğini göreceksiniz. Kısaca bu yöntem eskidi artık **Server Manager** üzerinden yükleme yapmalısınız, daha fazla ayrıntı için linki inceleyin demektedir.

![Crepe](assets/img/ws2012ins/ws12in02.png)

**Server Manager **sekmesini tıklayın. **Add Roles and Features** diyerek işlemlere başlıyoruz.

![Crepe](assets/img/ws2012ins/ws12in03.png)

Ardından **Next** ile devam edin.

![Crepe](assets/img/ws2012ins/ws12in04.png)

**Role-Based or Feature-Based** seçerek **Next** ile devam edin.

![Crepe](assets/img/ws2012ins/ws12in05.png)

Server seçimini yaptıktan sonra **Next** ile devam edin.

![Crepe](assets/img/ws2012ins/ws12in06.png)

**Active Directory Domain Service**s’i seçtikten sonra **Next** ile devam edin.

![Crepe](assets/img/ws2012ins/ws12in07.png)

Yüklenmesi gereken **Features**‘ları otomatik olarak listeliyor. **Add Features** tıklayın.

![Crepe](assets/img/ws2012ins/ws12in08.png)

**Next** ile devam edin.

![Crepe](assets/img/ws2012ins/ws12in09.png)

**Next** ile devam edin.

![Crepe](assets/img/ws2012ins/ws12in10.png)

**Next** ile devam edin.

![Crepe](assets/img/ws2012ins/ws12in11.png)

Gerekiyorsa **Restart** etmesi için seçeneği aktif edip **Yes** tıklayın.

![Crepe](assets/img/ws2012ins/ws12in12.png)

Install diyerek kuruluma başlayın.

![Crepe](assets/img/ws2012ins/ws12in13.png)

Kurulum yapılıyor.

![Crepe](assets/img/ws2012ins/ws12in14.png)

**Promote this server to a domain controller** diyerek işlemlere devam edin.

![Crepe](assets/img/ws2012ins/ws12in15.png)

Ortama ilk **domain controller** kuracağımız için **Add a new forest** seçeneğini seçerek **Root domain** name belirleyip **Next** ile devam edin.

![Crepe](assets/img/ws2012ins/ws12in16.png)

**Forest ve Domain Functional Level** belirledikten sonra **DNS** server seçeneği onaylı olarak gelecektir, değiştirmeden **Directory** servisinin kurtarma şifresini belirleyin ve **Next** ile devam edin. **Functional** seviyesini ortamdaki en düşük seviyeye belirlemenizde fayda var yoksa servislerin iletişimde problem olabilir. Seviyeyi ilerde arttırabilirsiniz fakat düşürme imkanı sonradan olmuyor.

![Crepe](assets/img/ws2012ins/ws12in17.png)

**Next** ile devam edin.

![Crepe](assets/img/ws2012ins/ws12in18.png)

**Netbios name** olduğu gibi bırakıp **Next** ile devam edin.

![Crepe](assets/img/ws2012ins/ws12in19.png)

**NTDS** ve **SYSVOL** dosyalarının kaydedileceği lokasyon bilgilerinin olduğu pencere. Bence burada dikkat edilmesi gereken bir konu profesyonel ortamlarda çalışırken recovery açısından database dosyasıyla log dosyanın aynı lokasyonda olmamasına dikkat edin. Tabi bu ayarlamalar spesifiktir, isteğinize ve yapınıza göre kon gure edebilirsiniz. Biz test ortamında olduğumuz için olduğu gibi bırakıp **Next** ile devam ediyoruz.

![Crepe](assets/img/ws2012ins/ws12in20.png)

Burada yapmış olduğumuz tüm ayarlamaların bilgileri yer almaktadır. İsterseniz **export** edebilirsiniz. Ben ***.txt** olarak kaydettim aşağıda screenshot’la belirttim. **Next** ile devam edin.

![Crepe](assets/img/ws2012ins/ws12in21.png)

![Crepe](assets/img/ws2012ins/ws12in22.png)

Özet kısmı sizde **All Prerequisite check passed succesfully**. **Click install to begin installation** gelecektir, **Install** diyerek devam edin.

![Crepe](assets/img/ws2012ins/ws12in23.png)

Ve kurulum yapılıyor.

![Crepe](assets/img/ws2012ins/ws12in24.png)

Sunucu **Restart** olduktan sonra **Local admin**, **Domain admin** olarak karşımıza çıkıyor, giriş yapın.

![Crepe](assets/img/ws2012ins/ws12in25.png)

**Yapıyı ve servisleri kontrol edelim**

![Crepe](assets/img/ws2012ins/ws12in26.png)

![Crepe](assets/img/ws2012ins/ws12in27.png)

![Crepe](assets/img/ws2012ins/ws12in28.png)

![Crepe](assets/img/ws2012ins/ws12in29.png)

![Crepe](assets/img/ws2012ins/ws12in30.png)

![Crepe](assets/img/ws2012ins/ws12in31.png)

![Crepe](assets/img/ws2012ins/ws12in32.png)

![Crepe](assets/img/ws2012ins/ws12in33.png)

![Crepe](assets/img/ws2012ins/ws12in34.png)

![Crepe](assets/img/ws2012ins/ws12in35.png)
