---
layout: post
title: Windows Server 2012 SNMP Kurulumu ve Ayarları
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [windows, snmp, network]
comments: true
---
Simple Network Management Protocol(Basit Ağ Yönetim Protokolü) Geniş yapıya sahip ağ yapılarında bulunan cihazları denetlemek için kullanılır. Cihaz üzerindeki sıcaklıktan, cihaza bağlı kullanıcılara, internet bağlantı hızından sistem çalışma süresine kadar çeşitli bilgiler SNMP’de tanımlanmış ağaç yapısı içinde tutulurlar.

**Server Manager**’dan, **Manage** kısmından **Add Roles and Features** tıklayarak başlayalım.

![Crepe](assets/img/ws12snmp/wsnmp01.png)

Ardından **Next** ile devam edelim.

![Crepe](assets/img/ws12snmp/wsnmp02.png)

Daha sonra **Role-based or feature-based installation** seçerek **Next** ile devam edelim.

![Crepe](assets/img/ws12snmp/wsnmp03.png)

Kurulacak olan Server‘ımızı seçtikten sonra **Next** ile devam edelim.

![Crepe](assets/img/ws12snmp/wsnmp04.png)

**Roles** kısmıından seçim yapmadan **Next** ile devam edelim.

![Crepe](assets/img/ws12snmp/wsnmp05.png)

**Features** menüsünden **SNMP Service **seçili duruma getirip, karşımıza gelen harici menüde **SNMP** servisinin çalışması için gereken features listesine **Add Faetures** tıklayıp devam edelim.

![Crepe](assets/img/ws12snmp/wsnmp06.png)

Görüntü aşağıdaki gibi olmalıdır. Ayrıca **SNMP WMI **sağlayısınıda aynı yöntemle sisteme entegre edebilirsiniz.

![Crepe](assets/img/ws12snmp/wsnmp07.png)

**Restart the destination server automatically if required** seçili yapın,

![Crepe](assets/img/ws12snmp/wsnmp08.png)

Aşağıdaki gibi uyarı alacaksınız, **Yes**‘leyip,

![Crepe](assets/img/ws12snmp/wsnmp09.png)

**Install** tıklayın.

![Crepe](assets/img/ws12snmp/wsnmp10.png)

Kurulum yapılıyor.

![Crepe](assets/img/ws12snmp/wsnmp11.png)

Yükleme bitti, **Close** deyip çıkın.

![Crepe](assets/img/ws12snmp/wsnmp12.png)

**Services.msc**‘yi çalıştırın.

![Crepe](assets/img/ws12snmp/wsnmp13.png)

**SNMP** servisini bulup çift tıklayıp servisi ayarlıyalım.

![Crepe](assets/img/ws12snmp/wsnmp14.png)

**SNMPv1** ve **SNMPv2** protokolleri bir topluluk anahtarı (**community string**) ile sorgulama yapar ve varsayılan olarak “**public**” ve “**private**” dir. **UDP** port **161** den çalışır.
**Security** tabına gelip **Community** kısmına **Add** diyelim.

![Crepe](assets/img/ws12snmp/wsnmp15.png)

Ben örnek teşkil etmesi açısından **community** **fatlan** diye isimlendirdim. Ardından **Security** tabından **community** **Add** diyerek **host**ları ekliyorum.

![Crepe](assets/img/ws12snmp/wsnmp16.png)

Örnek teşkil etmesi açısından aşağıdaki monitoring sistemlerini host olarak ekledim. Buraya direk ip adresi de belirtebilirsiniz.

![Crepe](assets/img/ws12snmp/wsnmp17.png)

Ardından servisi **restart** ediyorum ve işlem tamam.

![Crepe](assets/img/ws12snmp/wsnmp18.png)
