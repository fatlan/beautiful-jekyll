---
layout: post
title: Linux Apache ve Ngnix WordPress Web Server Üzerine Wildcard SSL Kurulumu
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [ssl, apache, nginx, web, server, linux, wildcard, sertififka]
comments: true
---
**Linux** üzerine **apache** olur, **nginx** olur ki globalde en çok kullanılan **web** servisleri bunlardır. Bu servislere **ssl** işi ilk etapta herkesi korkutmaktadır. Aslında bu iş çok kolay diye herkes aynı şeyi söyleyebilir, doğru kolay fakat işin mantığını anladıktan sonra kolaydır yoksa bilmeyen için zor, bilen için tabi ki kolaydır. Ben bu makalede **ssl** kurulumundan bahsedeceğim, hemde kendimde **ssl** kurarken yaşadığım deneyimlerden bahsedip konuyu ele almaya çalışacağım. **Web** server nedir, **apache** nedir, **nginx** nedir, **http** ve **https** nedir gibi konulara değinmeden, bildiğinizi farz ederek devam edeceğiz ki bu konulara girersek mesele haliyle çok uzayacaktır fakat **http** ve **https** mevzusunu tekrar bir gözden geçirmenizde fayda var. Her ihtimale karşı kısa kısa aşağıda konuya girmeden değinelim.

Tabi yine **ssl** kurulumuna girmeden önce sertifika nedir, **ssl** nedir, **ssl** çeşitleri ve uzantıları, **openssl** nedir, **csr** nedir gibi bir kaç terimden bahsedelim. **Standart** her yerde olan tanımlarına değinelim zaten konuya ilerledikçe ve yaptıkça daha da hakim olacağız.

**Web Server nedir** : **Hosting**(barındırma) işlemini İnternet protokolü üzerinden sunan bir sunucudur yada servistir desek daha doğru. Diğer bir ifade ile, bir web sitesinde yayınlanmak istenen sayfaların, resimlerin, videoların veya dokümanların globalde kullanıcılar tarafından erişebilmesini sağlayan sunucudur.

**Apache nedir** : Açık kaynak kodlu ve özgür bir web sunucu programıdır.

**Nginx nedir** : Rus yazılım mühendisi Igor Sysoev tarafından geliştirilen hafif, stabil, hızlı bir mail istemcisi olarak kodlanan daha sonraları geliştirilerek tüm sunucular için uygun hale getirilen bir web sunucusudur.

**Http nedir** : “**Hyper Text Transfer Protocol**” yani “**Hiper Metin Transfer Protokolü**“dür. 1990 yılından beri dünya çapında ağ üzerinde (**www-world web wide**) kullanılan bir iletişim protokolüdür. HTTP protokolü ağ üzerinden web sayfalarının görüntülenmesini sağlayan protokoldür.

**Https nedir** : “**Secure Hyper Text Transfer Protocol**” yani Türkçesiyle güvenli hiper metin aktarım iletişim kuralı anlamına gelmektedir. HTTP‘nin SSL sertifikası eklenerek daha kullanışlı ve güvenilir hale getirilmiş şeklidir.

**Domain nedir** : İnternet ortamında ip adresleri kolay hatırlanamayacağından onlara karşılık gelecek olan, sizin internette var olmanıza olanak tanıyan kimliğiniz diyebiliriz. Yani en basit deyimiyle internet sitelerinizin isimleri. Fatlan.com gibi, google.com gibi, youtube.com gibi gibi.

**Subdomain nedir** : Yukarıdaki domain tanıma bağlı olarak ilgili domain için kullanabileceği alt domainlerdir. Yani fatlan.com için www.fatlan.com, arge.fatlan.com, video.fatlan.com, mail.fatlan.com gibi gibi. Bu kısımda DNS ile ilgili yazdığım iki makaleyi gözden geçirmenizde fayda var.

**URL nedir** : “**Uniform Resource Locator**” teriminin baş harflerinden oluşan kısaltmadır. Aslında “adres” dediğimiz şeydir.

**URL Redirect(Yönlendirme) nedir** : Bir URL’ye gelen istekleri başka bir URL’ye iletme yani geçici veya kalıcı olarak yönlendirme işlemidir. URL yönlendirme birçok sebep için kullanılabilir.

**URL Rewrite nedir** : “URL Yeniden Yazma” yani website URL’lerinin uzun ve karmaşık yapısını daha anlaşılır hale getirme işlemine verilen isimdir.

**Sertifika nedir** : Herhangi bir nesneye niteliğini ya da kendisine verilmiş olan bir hakkı belirten resmi belgedir.

**SSL yada SSL Sertifika nedir** : Server ile alıcı iletişimi esnasında verilerin şifrelenerek yapılması işlemidir. En bilinen kullanımı ise, web sitesindeki veri alışverişi esnasında, server ile internet tarayıcısı arasındaki iletişimi şifrelenme sidir.

SSL sertifika çeşitlerine gelince temelde iki daha sonra kendi aralarında üçe ayrılır.

1)Doğrulama türüne göre ssl sertifikası : yani ssl satın alırken istenen belgeler doğrultusunda değişen sertifika türüdür.

    a : DV (Domain Validation) SSL : Domain doğrulama tipi sertifikadır. Belge gerektirmemesi ve sertifikanın anında üretilmesi sebebiyle tercih edilir.
    b : OV (Organization Validation) SSL : Organizasyon doğrulama tipi sertifikadır. Onay süreci kuruma ait belgelerle birlikte tamamlanır.
    c : EV (Extended Validation) SSL : Genişletilmiş doğrulama tipi sertifikadır. En gelişmiş sertifika ve pahalı bir sertifika türüdür. DV ve OV SSL sertifikalarının tüm özelliklerini barındırır, EV SSL sertifikaları web tarayıcılarında yeşil bar içerisinde kuruma ait unvanın da kullanıcılara gösterilmesini sağlarlar. Örnekler aşağıda belirtilmiştir.

![Crepe](assets/img/linux-apache-nginx-ssl-certs/ssl-cer-lin01.png)

![Crepe](assets/img/linux-apache-nginx-ssl-certs/ssl-cer-lin02.png)

2)Kullanım Alanına (Domain Tipine) Göre SSL Seritifikaları : yani domain yada bir yada birden fazla subdomain kullanıma göre değişen sertifika türüdür.

    a : Tek Domain / Standart SSL Sertifikaları : Subdomainler olmadan tek bir domain için kullanabilir sertifika türüdür. Yani fatlan.com için olabilir yada www.fatlan.com gibi.
    b : Multi Domain SSL Sertifikaları : İlgili domain için belirlediğiniz birden fazla olarak subdomainle birlikte kullanabileceğiniz sertifika türüdür. Örneğin fatlan.com, www.fatlan.com, arge.fatlan.com, video.fatlan.com gibi.
    c : Wildcard SSL Sertifikaları : Sertifikanın kullanıldığı domainin tüm subdomainlerinin imzalanmasını sağlayan sertifika tipidir. Yani ilgili domain için “multi domain gibi önceden belirlemenize gerek kalmadan” +n tane subdomaini için
    kullanabileceğiniz sertifika türüdür.

Sertifika uzantı dosyalarını başta belirtelim ki kafada bir karışılık olmasın. Daha çok karşınıza çıkacak ***.crt**, ***.cer**, ***.der**, ***.pem**, ***.pfx**, ***.key** gibi gibi, daha farklı formatlar tabi ki mevcut ama konu ilgili olarak genelde bu uzantılı sertifikalar ile meşgul olacaksınız. Ama **cer/crt** ve **pem** den biraz bahsedelim. İlgili domain için birincil sertifika uzantısı **cer/crt** olacaktır. **Pem** dosyası ise sertifika dosyalarının birleştirilmiş kapsayıcısıdır. **Bundle** edilmiş halidir diyebiliriz. **PEM** sertifikası **Base64** ile kodlanmış **ASCII** dosyasıdır. Aşağıdaki örnek bir sertifika görüntüleyerek açıklamak gerekirse;

Mouse’la seçili olan alan *.google.com **wildcart ssl**’i **cer/crt**’ye karşılık gelen dosyadır.

**Sertifika hiyerarşisi** olarak kırmızı seçili alan;

- GeoTrust Global CA
    - Google Internet Authority G2
        - *.google.com ise pem dosyasına karşılık gelir.

![Crepe](assets/img/linux-apache-nginx-ssl-certs/ssl-cer-lin03.png)

**Pem** dosyasını biraz daha açacak olursak birden fazla ssl dosyasının, içine aktarıldığı kapsayıcı ssl dosyasıdır. **Pem**’de **ssl** zinciri oluşturmanın kuralı vardır ve o kurala göre oluşturulur. **Pem** dosyası herhangi bir editör uygulamasıyla düzenlenebilir. **SSL** zinciri temel olarak aşağıdaki kurala göre bundle edilir. Google örnek olarak ele alınmıştır.

a. Birincil Sertifika – YourDomainName.crt

b. Orta Sertifika – DigiCertCA.crt

c. Kök Sertifika – TrustedRoot.crt yani aşağıda;

~~~
—–BEGIN CERTIFICATE—–
(Birincil SSL sertifikası google.com.cer dosyasının içeriği)
—–END CERTIFICATE—–
—–BEGIN CERTIFICATE—–
(Sertifikanızı imzalayan Alt KÖK Makamı/Orta-Ara Sertifika googleinternetauhority.cer
dosyasının içeriği)
—–END CERTIFICATE—–
—–BEGIN CERTIFICATE—–
(Alt KÖK Makamını imzalayan KÖK Makamı/Kök Sertifika geotrustca.cer dosyasının içeriği)
—–END CERTIFICATE—–
~~~

**Pem** dosyasını anlamak adına, ayrıntılı bilgi için [https://www.digicert.com/ssl-support/pem-ssl-creation.htm](https://www.digicert.com/ssl-support/pem-ssl-creation.htm) linkini mutlaka inceleyin.

Gene burada bir püf nokta paylaşayım. **WildCard SSL**’i satın alırken diyelim ***.fatlan.com** olarak talep ettiğinizde **fatlan.com**’u a**lternatif name**’i **WildCard SSL**’inizin içermesi gerekiyor, buna dikkat edin. Aksi taktirde **COMMON NAME** hata mesajını alacaksınız. Ki bu **common name** uyarısını alırsanız ya **domain name** düzgün konfigure edilmemiştir yada **ssl** sertifikanız **common name** kısmı ile alakalı olarak **DomainName**’inizi desteklemiyordur. Bu hatayı aldığınızda, düzeltmek için **Server Name**’inizi yada **HostName**’inizi değiştirmeyin, sorun düzelmeyecektir. Aşağıdaki linki incelemenizi tavsiye ederim. Bu adreste **RFC**‘ye de referans verilmiş.

[https://serverfault.com/questions/310530/should-a-wildcard-ssl-certificate-secure-both-the-root-domain-as-well-as-the-sub](https://serverfault.com/questions/310530/should-a-wildcard-ssl-certificate-secure-both-the-root-domain-as-well-as-the-sub)

**SSL** den bu kadar bahsettik şimdi gelelim bir ssl’imizin olabilmesi için yani bir **ssl** satın alabilmek için yapılması gerekenlere, en başta bir ***.csr** dosyamızın ve ***.key private key** dosyamızın olması lazım. Bu işleme **crs** oluşturma, **csr generate** etme yada **csr request** de diyebiliriz. Yani her şeyden evvel bir ssl satın alabilmek için web sunucusunda **ssl csr** isteği oluşturmak gerekli. Bu işlem için **OpenSSL**’i kullanacağız.

**Csr nedir** : **Certificate Signing Request** web sunucunuz tarafından üretilen, içinde web sunucunuza ve **SSL** sertifikasını kullanacağınız web sitesine dair bir takım veriler içeren koddur. **SSL csr** dosyasıyla satın alınacaktır.

**Private Key nedir** : **CSR (Certificate Signing Request)** oluşturma sırasında kullanılan ve **SSL** Sertifikasıyla uyumlu bir anahtardır. **Private Key** dosyasını güvenlik nedeniyle muhafaza ediniz ve 3. şahıslarla paylaşmayınız.

OpenSSL nedir : SSL ve TLS protokollerinin açık kaynak kodlu uygulamasıdır.

Dediğimiz gibi **CSR** isteğini **OpenSSL** ile oluşturacağız bunun için sunucuda **openssl** uygulaması kurulu olması gerekiyor. Kurulumuna hiç girmeyeceğim konu dağılmasın. Kurulumunu başka bir makalede ele alırsak burada link verebiliriz. Her şeyden önce **CSR** üretme ile ilgili olarak ortak platformların ve işletim sistemlerinin yer aldığı [https://www.digicert.com/csr-creation.htm](https://www.digicert.com/csr-creation.htm) linkini mutlaka inceleyin.

Bu arada sunucuda birden fazla web sitesi barındırıyorsanız komutlarda oluşturduğunuz ilgili domain’in ismini kullanın. **Csr** oluşturulması interaktif bir şekilde olacaktır. Sizden bir takım bilgiler isteyecektir (**Common name**, **State**, **Organization** **Name** vs gibi), bu bilgileri elde edince gerekli dosya oluşacaktır.

~~~
openssl genrsa -out fatlan.com.key 2048   –> *.key private key dosyası oluşur.

openssl req -new -key fatlan.com.key -out fatlan.com.csr   –> Ssl için *.key dosyasını kullanarak *.csr dosyası oluşur.

ya da böyle iki komut girmek yerine aşağıdaki gibi tek komutla da halladebilirsiniz.

openssl req -new -newkey rsa:2048 -nodes -keyout fatlan.com.key -out fatlan.com.csr
~~~

Tabi komuttan sonra istenilen bilgileri girdikten sonra(**Common name**, **State**, **Organization Name** vs gibi).


Tüm bunlara alternatif olarak da interaktif olarak yani komuttan sonra tek tek bilgi girmeden de bu işlemleri tek seferde toptan yapabilirsiniz. Yani aslında istediği tüm bilgileri tek bir komutta toptan verebilirsiniz.

Bunun için ilk önce [https://www.digicert.com/easy-csr/openssl.htm](https://www.digicert.com/easy-csr/openssl.htm) link’ini açıp, aşağıdaki resimden de görüldüğü üzere Certificate Details kısmını doldurup, Generate butonuna basıyorsunuz. Daha sonra **Information** bölümünde kullanacağınız komut seti oluşturuluyor. Siz bunu direk kopyalayıp ilgili sunucuda çalıştırdıktan sonra hem ***.key** hem ***. csr** dosyanız otomatik olarak oluşuyor. Tüm işlem bu kadar.

![Crepe](assets/img/linux-apache-nginx-ssl-certs/ssl-cer-lin04.png)

Şimdi asıl konumuza geri dönecek olursak elimizde bir **Linux** Sunucu var ve bunda **Apache** yada **Nginx** kurulu ve içerik olarak **WordPress** kullanıyoruz (**WordPress** kısmı önemli bir kaç şeyi onun üzerinden değiştireceğiz yoksa ilgili kodda değişiklik yapmak gerekebilir) ve bu sunucuda ilgili **Domain** için **CSR** oluştu ve o **CSR** ile, ilgili domain için istediğimiz **SSL** Sertifikayı aldık ve Sunucuda **$DizinYolu/SSL** diye bir dizin oluşturup(siz farklı bir dizin de oluşturabilirsiniz) bu sertifikaları bu dizin içine kopyaladık varsayıyorum.

Bu arada **Url**’inizi adres çubuğunda **fatlan.com** diye çağırdığınızda **www.fatlan.com** olarak gelmesini sağlamalısınız. Zaten bu bir standarttır. Yani screenshot’lardan anlaşılacağı üzere adres çubuğuna

![Crepe](assets/img/linux-apache-nginx-ssl-certs/ssl-cer-lin05.png)

![Crepe](assets/img/linux-apache-nginx-ssl-certs/ssl-cer-lin06.png)

Bunun ayarı aşağıdaki screenshot’tan anlaşılacağı üzere **WordPress**’te **Ayarlar(Settings)- Genel(General)** sekmesine giderek aşağıdaki şekilde yapılabilir. Siteniz **WordPress** değilse kod tarafında bu değişikliği yapmalısınız. Daha sonra burada başka bir ayar daha yapacağız (**http- https url redirect**) onuda ilerde bahsedeceğim.

![Crepe](assets/img/linux-apache-nginx-ssl-certs/ssl-cer-lin07.png)

**Apache için SSL kurulumu**;

**Ssl** aldık ve sunucuda diyelim **/root/ssl** klasörünün içine attık, zaten apache için hali hazırda **http(80)** üzerinden çalıştığı için **VirtualHost** tanımı mevcut olmalıdır. Şimdi biz ise **VirtualHost** tanımını içine **https(443)**‘ü aşağıdaki görüldüğü üzere **ssl**’li bir şekilde ekleyeceğiz.

Domain’imizin fatlan.com olduğunu varsayalım.

**etc/httpd/conf-available.d/fatlan.com**(ki bu dosyanın **/etc/httpd/conf.d/110-fatlan.com.conf** gibi olarak linklenmesi olayına hiç girmiyorum) dosyasının içerisindeki **VirtualHost** tanımına;

**Default http(80) üzerinden çalışırken**;

~~~
<VirtualHost *:80>
    ServerAdmin fatlan@fatlan.com       –> Server Yönetici Email Adresi(opsiyonel)
    ServerName fatlan.com               –> İlgili Domain/AlanAdı Adresi
    ServerAlias www.fatlan.com          –> İlgili Domain/AlanAdı www Kayıtlarınında Karşılanacağı(opsiyonel)
    DocumentRoot /var/www/fatlan.com/   –> Web Dosyalarımızın Bulunduğu Klasör Yolu, isterseniz farklı dizin yolu kullanabilirsiniz
</VirtualHost>
~~~

Daha sonra **SSL** ile beraber **https(443)** için satırların eklenmiş hali;

~~~
<VirtualHost *:80>
    ServerAdmin fatlan@fatlan.com
    ServerName fatlan.com
    ServerAlias www.fatlan.com
    Redirect permanent / https://www.fatlan.com/        –> Bu kısımda ise tüm istekleri https(443)’ye yönlendiriyoruz
    DocumentRoot /var/www/fatlan.com/
</VirtualHost>

<VirtualHost _default_:443>
    ServerAdmin fatlan@fatlan.com
    ServerName www.fatlan.com
    DocumentRoot /var/www/fatlan.com/
    SSLEngine On                                        –> SSL’i aktif ediyoruz
    SSLCertificateFile /root/ssl/fatlan.com.cer         –> İlgili Domain sertifikamızı belirtiyoruz
    SSLCertificateKeyFile /root/ssl/fatlan.com.key      –> İlgili Domain için key dosyamızı belirtiyoruz
    SSLCertificateChainFile /root/ssl/fatlan.com.pem    –> İlgili domain için bundle edilmiş sertifikamızı belirtiyoruz
    SSLProtocol all -SSLv2 -SSLv3                       –> Güvenlik sebebi için SSLv2 ve v3 pasif duruma getiriyoruz
</VirtualHost>
~~~

**Tamda bu aşamada iki önemli bilgi daha paylaşayım**;

**İlki** : Sunucuda birden fazla web site barındırıyor iseniz, **< VirtualHost *:80 >** yada **< VirtualHost _default_:443 >** tanımları için kullanılan ***** ve **_default_** yerine sunucunuzun ip adresini yazarsanız domain sunucuyu kendine tahsis edecektir yani diğer mevcut web sayfalarını açmak istediğinizde açılmayacak, yerine bu sayfa açılacaktır.

**İkincisi** : **< VirtualHost *:80 >** tanımındaki Redirect permanent satırı yani **80**’den **443**’e yönlendirme satırı, evet bu istediğimiz işlemi yapacaktır fakat bunun yerine site yapımız **WordPress** olduğu için bu kısmı **WordPress**’te Ayarlar(**Settings**)-Genel(**General**) sekmesine giderek aşağıdaki şekilde yapabiliriz ve aslında **< VirtualHost *:80 >** tanımını da VirtualHost dosyasından komple kaldırabiliriz. Zaten **WordPress**’te bu ayarlama yapıldığı vakit o işlemi **WordPress** yapacaktır. Ben her iki ayarı da aktif şekilde kullandım.

![Crepe](assets/img/linux-apache-nginx-ssl-certs/ssl-cer-lin08.png)

**Nginx için SSL kurulumu**;

**Ssl** aldık ve sunucuda diyelim **/root/ssl** klasörünün içine attık, zaten **nginx** için hali hazırda **http(80)** üzerinden çalıştığı için **VirtulHost** tanımı mevcut olmalıdır. Şimdi biz ise **VirtualHost** tanımı içine **https(443)** tanımını aşağıdaki şekilde **ssl**’li bir şekilde yapacağız.

Domainimizin fatlan.com olduğunu varsayalım.

**/etc/nginx/sites-available/fatlan.com.conf**(ki bu dosyanın **/etc/nginx/sites-enabled/fatlan.com.conf** olarak **link**’lenmesi olayına hiç girmiyorum) dosyasının içerisindeki **VirtualHost** tanımına;

**Default http(80) üzerinden çalışırken**;

~~~
server {
        listen 80;                              —> Bu kısım istek tipi
        server_name fatlan.com;                 —> İlgili Domain/AlanAdı Adresi
location / {
        try_files $uri $uri/ $uri$args $uri$args/ /index.html;
        root /var/www/fatlan.com;               —> Web Dosyalarımızın Bulunduğu Klasör Yolu
        index index.html index.htm;
    }
}
~~~

**Daha sonra SSL ile beraber https(443) için satırların eklenmiş hali**;

~~~
server {
        listen 80;
        server_name fatlan.com;
        return 301 https://www.fatlan.com;                  —> Bu satırda istekleri https(443) yönlendirir, yani URL Redirect
location / {
        try_files $uri $uri/ $uri$args $uri$args/ /index.html;
        root /var/www/fatlan.com;
    index index.html index.htm;
    }
}
server {
        listen 443;
        server_name fatlan.com;
        ssl on;                                             —> SSL’i aktif ediyoruz
        ssl_certificate /root/ssl/fatlan.com.pem;           —> İlgili Domain sertifikamızı belirtiyoruz
        ssl_certificate_key /root/ssl/fatlan.com.key;       —> İlgili Domain için key dosyamızı belirtiyoruz
location / {
        try_files $uri $uri/ /index.html;
        root /var/www/fatlan.com;
        index index.html index.htm;
    }
}
~~~

Hepsi bu kadar, Tüm tanımlarımız tamamsa test edebiliriz. Buraya kadar her şey doğru ise siteye bağlanırken adres çubuğuna **fatlan.com** yazıp enter’a bastığımız zaman **https://www.fatlan.com** adresine bağlanıp sertifikayı aktif etmelidir ve adres çubuğunda **https**’ye kadar olan kısım **yeşil** görünmelidir. Aşağıdaki gibi.

![Crepe](assets/img/linux-apache-nginx-ssl-certs/ssl-cer-lin09.png)

Her şey tamam fakat **yeşil** görünmüyorsa bir yerde bir şey vardır. Alttaki resimden de anlaşılacağı üzere, seçili alanda uyarıyı verecektir.

![Crepe](assets/img/linux-apache-nginx-ssl-certs/ssl-cer-lin10.png)

Hatanın bir sebebi de **https(443)** olarak getirilen sitede sayfa içerisinde **http(80)** ile çağrılmaya çalışılan **url**’ler varsa bundan kaynaklı, sayfa **Insecure** görünüp **yeşil** gösterilmeyebilir. Bu linkleri düzeltip **https**’ye çevirmelisiniz.

Bunu için sayfayı açtıktan sonra alttaki screenshot’tan da anlaşılacağı üzere;

**Mozilla Firefox** ise : sağ click – **Inspect Element**

**Google Chrome** ise : sağ click – **Inspect** tıklayıp **Console** sekmesine geliyoruz. Evet bu bölümde **https(443)** ile çağırılan sayfada **http(80)** ile çağrılmaya çalışılan linkleri kızarık bir şekilde belirtecektir. Bu linkleri **https(443)** çevirdikten sonra yani kızarıklıkları yok ettikten sonra sayfada **ssl** kısmı **secure** görünüp **yeşil**‘lenecektir.

![Crepe](assets/img/linux-apache-nginx-ssl-certs/ssl-cer-lin11.png)

Yukarda belirtmiştim tekrar belirtmekte fayda var **Wildcard** satın alırken örneğin ***.fatlan.com** için aldığınız **SSL**’e **alternatif** **name** olarak **fatlan.com**’unda bulunması gerekli yoksa şöyle bir şey söz konusu olabilir. Sitenize ilk defa bağlanan biri yada tüm çerezleri silip inatla manuel olarak **https://fatlan.com** yazarak siteye bağlanmaya çalıştığında **ssl https://www.fatlan.com** yönlendirirken çakılıp **fatlan.com comman name**’i bulamayacağı için **Insecure Connection** hatası verecektir. Aşağıdaki görselden de anlaşılabilir.

![Crepe](assets/img/linux-apache-nginx-ssl-certs/ssl-cer-lin12.png)

Aklıma gelenler bunlar, umarım faydalı olur. Eksik yada hatalı gördüğünüz kısımları belirtirseniz sevinirim ve düzeltmeye çalışırım.