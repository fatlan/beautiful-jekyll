---
layout: post
title: DNS(Domain Name System-Alan Adı Sistemi)
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [dns]
comments: true
---
Bilindiği üzere bilgisayar sistemleri arka planda anlamlı numaraların birleşmesiyle oluşan değerlerle işlem yapar. Yani bit’lerden oluşan 0 ve 1 ile ve sadece toplama, çıkarma işlemlerini kullanarak sonuca ulaşır vs. çok fazla ayrıntıya girip kafa bulandırmayalım. Aynı şekilde bilgisayarlar ağ ve net ortamında da birbirleri arasında haberleşme sağlarken belli bir numara kalıbını kullanarak haberleşirler. Bu numara kalıbı herkesçe biline **IP(Internet Protokol)** adresleridir. Peki konumuzla olan alakası nedir? denecek olursa şöyle ki bizler tarayıcımız aracılığı ile biryere bağlanmak isterken adres kısmına; örneğin www.duckduckgo.com gibi ibareler kullanırız, fakat bilgisayar arka planda o adrese karşılık gelen ip adresi ‘46.51.197.89’ ile bağlanır
ve sayfayı karşımıza getirir. İşte bu işlemleri yapan arkadaş **DNS**’tir. Neden böyle bişeye ihtiyaç var derseniz tabi ki insan oğlunun sayılarla arası pek yoktur ve akılda tutulması, ezberlenmesi zordur. İşte bu yüzden bu işlemleri otomatik yapacak bir sistem geliştirilmek istenmiş ve DNS bu şekilde internetin babası olarak söylenen **ARPANET** tarafından ortaya çıkmıştır. Daha öncesinde, bilgisayarların bu kadar yaygın olmadığı zamanlarda **HOSTS.TXT**(Genelde C:\Windows\System32\drivers\etc) dosyası kullanılıyordu.

Adres-isim tanımlamalarını içeren **HOSTS.TXT** dosyası **SRI** tarafından **SRI-NIC(Stanford Research Institute – Network Information Center)** adında bir bilgisayar üzerinde tutulmaktaydı. Bu dosya her adrese bir isim karşılık gelecek şekilde düzenlenmişti. **Arpanet** üzerindeki yeni isim tanımlamaları ve değişiklikleri **SRI**’ya gönderilen e-postalar arcılığı ile yapılıyor ve **HOSTS.TXT**’in kopyası **FTP(File Transfer Protocol)** ile alınıyordu.

![Crepe](/assets/img/dns01/dnsb01.png)

![Crepe](/assets/img/dns01/dnsb02.png)

**İsim Çözümleme ve Sırası**

DNS sistemlerinde harhangi bir nesnenin Hostname’ine karşılık gelecek ip adresi olması gereklidir. **FQDN(Full Quali ed Domain Name)** ise bir alan adının tamamını ifade eder. Örneğin; duckduckgo.com bir domain name iken, www.duckduckgo.com **FQDN**’dir. Ayrıca fatlan.com bir domain’i ifade ederken forum.fatlan.com’daki forum **subdomain**(alt domain)’i ifade eder. Örneğin duckduckgo’nun ip adresini aşağıdaki komutu çalıştırarak öğrenelim.

~~~
ping duckduckgo.com
~~~

![Crepe](/assets/img/dns01/dnsb03.png)

**İsim çözümleme sırası ise;**

1. Client resolver Cache- DNS cache
2. Host dosyası
3. DNS
4. NetBIOS Name Cache
5. WINS
6. Broadcast
7. Lmhosts dosyası

Malum **DNS** çözümlemeleri için bir trafik oluşur ve bu işlem vakit alabilir. Buna çözüm olarakta cache’leme olayını geliştirmişler. Girilen siteler yada **HOST** dosyasına eklenen veriler daha hızlı erişim için cache’lenir. Tabi bu belli bir süreye bağlanmıştır yoksa kullanılan linklerdeki son değişiklikler gözlemlenemez. Bu süreye **TTL(Time To Live-Yaşam Süresi)** denir. Bu süreyi DNS tarafında düzenleyebilirsiniz.

![Crepe](/assets/img/dns01/dnsb04.png)

Aşağıdaki **CMD** çıktısındada bu değeri görebilirsiniz. **Client resolver Cache- DNS cache**’lerini görmek için komut satırından aşağıdaki komutu kullanabilirsiniz. Ben **HOST** dosyasına kayıt ekliyorum onuda görebileceğim.

![Crepe](/assets/img/dns01/dnsb05.png)

~~~
ipconfig /displaydns
~~~

![Crepe](/assets/img/dns01/dnsb06.png)

**Client resolver Cache -DNS cache**’lerini temizlemek için ise,
~~~
ipconfig /fushdns komutlarını kullanabilirsiniz.
~~~

Fakat **Command Promt**’u **Admin** modunda çalıştırmazsanız komutları çalıştırırken ‘**The requested operation requires elevation**’ uyarısı alabilirsiniz.

**NetBIOS** ; **Network Basic Input/Output System** için bir kısaltmadır. Bir yerel ağ üzerinde
bilgisayarların birbirleri ile iletişim kurmasını sağlayan ve isim çözümlesini gerçekleştiren bir **API**‘dir.

**WINS(Windows Internet Name Service)**; Hizmetinin başlıca amacı bilgisayarların **NetBIOS** isimlerini **IP** adreslerine çevirmektir.

**Broadcast**; Ağlar arası iletişimde gönderilen bilgi paketinin tüm cihazlar tarafından alınmasına verilen bir isimlendirmedir. Yani giden paket özel bir adrese değil tüm adreslere gönderilir. Bazen tüm ağ cihazlarına bağırma diye de tabirini duyabilirsiniz. Daha sonrasında bağırma işleminin ardından paket istemci tarafından kabul edilerek işlem yapılır. Bu şekilde bir çözümleme sistemidir. Tabi ilk bakışta ağ performansını yorması ve karmaşıklığı aklınıza gelebilir.

**LMHOSTS(LAN Manager Hosts)** ; **WINS** servisi gibi isim çözümlemede kullanılan gene **HOSTS** dosyasının **path**’inde bulunan bir veritabanı sistemidir.
