---
layout: post
title: Redhat/Centos 7 Üzerine DNS Server Kurulumu ve Yapılandırması – BİND(NAMED), NS(NAMESERVER) – Public DNS or Private DNS
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [bind, named, redhat, centos, linux]
comments: true
---
Daha önceki yazılarımda **DNS** ile alakalı olarak bir çok bilgi paylaştım. **DNS** nedir.?, **NS** nedir.? gibi birçok soruyu ilgili yazılarıma havele edip bu yazımızda direk konuya geçiş yapmak istiyorum.

**NoT** : Makalede kullanılacak olan domain **fatlan.com**’dur, ip'ler tamamen sallamasyondur.

**Linux** sistemlerde **DNS** sistemi kurup, aktif edebilmek için **Bind** paketini kurmalısınız. Servisin adı ise **Named** olarak bilinmektedir. **Named** ile servis yönetimini sağlayabilirsiniz.

İlk etapta bir **Centos7** sunucusu kurup ardından sabit bir ip ile **network**’ünü ayağa kaldırıyorum. Daha sonra **yum update** komutunu çalıştırıp sunucuyu **stabil** hale getirelim. Ardından “**/etc/hostname**” dosyasından sunucunun **hostname**’ini **ns1.fatlan.com** olarak değiştiriyorum. Bu değişikliği “**/etc/hosts**” dosyasında da yapıyorum. Bu arada **update** ve paketlerin kurulumundan sonra “**/etc/resolv.conf**” ve “**/etc/syscon g/network-scripts/ifcfg-ens0**” dosyalarından **dns** sunucusu olarak kendi kendini göstermeyi unutmayın.

Daha sonra aşağıdaki komut ile **DNS** için gerekli paketlerin kurulumunu yapalım.

~~~
yum install bind bind-utils
~~~

Şimdi asıl önemli olan yapılandırma kısmı ve ilk olarak “**/etc/named.conf**” dosyasını yapılandıracağız ama öncesinde “**named.conf**” dosyasının bir yedeğini alın. Varsayılan olarak “**named.conf**”un içeriği aşağıda ss gibidir.

![Crepe](/assets/img/re-ce-named-insandconf/named-insta-c01.png)

Şimdi bir editör yardımıyla dosyayı edit’lemeden önce neler değişecek ya da eklenecek onlardan
bahsedelim.
Varsayılan satırlar;

~~~
    -listen-on port 53 { 127.0.0.1; };
    -listen-on-v6 port 53 { ::1; };
    -allow-query { localhost; };
~~~

Değişecek şekli;

~~~
    -listen-on port 53 { any; };
    -listen-on-v6 port 53 { none; };
    -allow-query { any; };
~~~

Eklenecek satırlar;

~~~
    -forward only; //optional
    -forwarders { 8.8.8.8; }; //optional
    -fatlan.com’a ait zone ve reverse zone bilgileri bu dosyada belirtilir.
~~~

Son ekran görüntüsü aşağıdaki gibidir. (**203.34.100.in-addr.arpa**) ss ler hatalıdır.

![Crepe](/assets/img/re-ce-named-insandconf/named-insta-c02.png)

Şimdi sıra **zone** dosyalarını yani **dns** kayıtlarının barındılacağı dosyaları oluşturmaya geldi. Bu **zone** ve **revzone** dosyalarını “**/var/named/**” dizininin içinde ve “**/etc/named.conf**” dosyasında belirttiğimiz zone isimlerinde oluşturmalıyız.

İlk öce dizine gidelim.

~~~
cd /var/named
~~~

Ardından ilgili **zone** dosyalarını oluşturalım(ben **named.conf**’ta belirttiğim isimlere göre oluşturuyorum).

~~~
touch fatlan.com.zone
touch 100.34.203.revzone
~~~

Son olarak herhangi bir editör aracılığı ile “**fatlan.com.zone**” ve “**203.34.100.revzone**” zone dosyalarının içeriğini aşağıdaki ss görüldiği gibi dns kayıtlarını giriyorum.

**Reverse zone** kısmında ip adresi tersten ve son okted olmadan yazılmalıdır, bu yüzden ss ler hatalıdır(**203.34.100.in-addr.arpa**).

**NoT** : **A**, **CNAME** ve **PTR** kayıtlarını örnek olsun diye ekledim. Diğer yapılandırma şekilleri önemli, **zone** ve **revzone** da farklı olmasının sebebi her iki türlü de olabildiğine örnek olması için ekledim.

**fatlan.com.zone;**

![Crepe](/assets/img/re-ce-named-insandconf/named-insta-c03.png)

**203.34.100.revzone;**

![Crepe](/assets/img/re-ce-named-insandconf/named-insta-c04.png)

Şimdi buraya kadar herşey tamam gibi ama bundan sonra yapmamız gereken yapılan tüm bu ayarlamalarının doğruluğunu test etmek.

İlk önce “**/etc/named.conf**” dosyasını doğru yapılandırmış mıyız?, kontrol edelim. Sonucunda birşey dönmüyorsa doğru demektir.

~~~
named-checkconf
~~~

Şimdi de **fatlan.com.zone** dosyasını kontrol edelim.

~~~
named-checkzone fatlan.com /var/named/fatlan.com.zone
~~~

![Crepe](/assets/img/re-ce-named-insandconf/named-insta-c05.png)

Son olarakta **203.34.100.revzone** dosyasını kontrol edelim.

~~~
named-checkzone 100.34.203.in-addr.arpa /var/named/100.34.203.revzone
~~~

![Crepe](/assets/img/re-ce-named-insandconf/named-insta-c06.png)

Herşey yolunda olduğuna göre **named** servisini başlatalım.

~~~
systemctl start named.service
~~~

Son olarak **53** **port**unu dinlediğinden emin olalım. **Firewall**’dan **53 port**una erişim izinlerini sakın unutmayın.

~~~
netstat -plntua | egrep -i named
~~~

![Crepe](/assets/img/re-ce-named-insandconf/named-insta-c07.png)

Sunucu tarafında herşey bitti, şimdi bir kullanıcı makinasının **dns**’ini yeni sunucumuza yönlendirip kayıtları çözme durumunu kontrol edelim.

![Crepe](/assets/img/re-ce-named-insandconf/named-insta-c08.png)
