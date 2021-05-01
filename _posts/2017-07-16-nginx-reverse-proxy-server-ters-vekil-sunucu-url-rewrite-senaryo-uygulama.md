---
layout: post
title: Nginx Reverse Proxy Server (Ters Vekil Sunucu) URL Rewrite Senaryo - Uygulama
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [nginx, proxy, linux, reverse]
comments: true
---
![Crepe](/assets/img/nginx-rev-prox-uy/nginx-rev-proxy01.png)

**Nginx nedir** : Rus yazılım mühendisi Igor Sysoev tarafından geliştirilen hafif, stabil, hızlı bir mail istemcisi olarak kodlanan daha sonraları geliştirilerek tüm sunucular için uygun hale getirilen bir web sunucusudur.

**Nginx** birçok amaç için kullanılabilir. Bunlar, ilk meydana çıkış sebebi olan **Mail** servisi, **Web** servisi, **Reverse Proxy**(bu özellik genelde web sayfalarını cache’lemek için kullanılır ve böylelikle sunucu yükünü azaltıp daha hızlı ve stabil çalışmasını sağlar) ve Load Balancer olarak sıralanabilir.

Daha fazla detay için [https://nginx.org/en/](https://nginx.org/en/) linkini ziyaret edebilirsiniz.

**Proxy** Server(Vekil Sunucu) mantığını açıklamak gerekirse, istemci tarafında bulunan ve web sunuculara çıkış için yani bağlanırken arada bir vekil görevini görmesidir. **Reverse Proxy**(Ters Vekil) ise sunucu tarafında bulunan ve istemcilerin isteklerine cevap verip web sunuculara bağlanıp istemcilere iletmesidir.

Biz şimdi uygulama senaryomuzda **Nginx**’i **URL Rewrite** yapması için kullanacağız. İleride uygulamayı yaptıkça kafanızda daha da oturacaktır. **Url** nedir aşağıda değinelim.

**URL nedir** : “**Uniform Resource Locator**” teriminin baş harflerinden oluşan kısaltmadır. Aslında “adres” dediğimiz şeydir.

**URL Rewrite nedir** : “**Url** Yeniden Yazma” yani **website Url**’lerinin uzun ve karmaşık yapısını daha anlaşılır hale getirme işlemine verilen isimdir.

**URL Redirect**(Yönlendirme) nedir : Bir URL’ye gelen istekleri başka bir URL’ye iletme yani geçici veya kalıcı olarak yönlendirme işlemidir. URL yönlendirme birçok sebep için kullanılabilir.

Şimdi uygulama senaryomuzu yazalım ve niye böyle bir senaryoya ihtiyaç duyarız bunu belirtelim. Herşeyden evvel ben tüm işlemler için 3 adet sanal makina(**CentOS**, **Ubuntu**, **Fedora**) hazırladım ve o sanallar üzerinden senaryoyu gerçekleştireceğiz. Test **domain**’imiz **webmin.test** olarak kullanacağız.

**1. Adım** : **Web Servers** olarak kullanacağımız **CentOS** Sunucu kurulumu gerçekleştirilecek. IP yapılandırması (192.168.1.100). Şimdi bu sunucu üzerinde **Apache** ile webden hizmet veren birden fazla servis çalışıyor olabilir ve bu servisler **80** değil de hepsi kendine özgü port’lardan erişiliyor olabilir. Ben test için bu sunucu üzerine **10000** portundan çalışan **Webmin** servisini kuracağım. Siz aynı sunucu üzerine farklı portlardan çalışan birden fazla servisi kurabilirsiniz. **Jira**, **confluence** vs gibi yada manuel yapılandırmış servisler gibi gibi.

**2. Adım** : **Reverse Proxy Server** için **Ubuntu Server** kurulumu gerçekleştirilecek. IP yapılandırması (192.168.1.101). Bu sunucu üzerine Nginx kurulumu gerçekleştirilip **URL Rewrite** için yapılandırılacak.

**3. Adım** : Tüm bu işlemleri test edeceğimiz Client kullanıcı olarak Fedora kurulumu gerçekleştirilecek.

Peki niye böyle bir şeye ihtiyaç var kısmını açıklamaya gelecek olursak. **1. Adımda** farklı portlardan çalışıp webden hizmet sunan birden fazla servis olabilir. Tabi biz bu servislere erişimi **Ip** üzerinden değilde **DNS**’te kaydını oluşturacağımız **domain** üzerinden yapacağız FAKAT servis/servisler **farklı port**lardan hizmet verdiği için bilindiği gibi **http://domain_name:port**_numarası şeklinde erişmelidir ki yoksa **web** servisi default **web** sayfasını yansıtacaktır. Tabi kullanıcılara **port** ezberletip şöyle böyle erişin demek mümkün olmayacağı için **2. Adımdaki Reverse Proxy** servisini kurup sonra yapılandırıp o işi arka planda **Nginx**’e yaptıracağız. Tabi böylelikle **Web Server**’ın yükünü de hafifletmiş olacağız.

**Hemen senaryonu adım adım gerçekleştirelim.**

**1. Adım** :

**CentOS** sunucu kurulumu gerçekleştirilecek. Daha sonra Ip yapılandırmasını senaryodaki gibi yapılandırın. Ardından ben test için **10000** portundan çalışan **Webmin** servisini kuracağım. Siz isterseniz kendi bildiğiniz farklı porttan çalışan webden hizmet veren bir yada birden fazla servisi kurup çalışır vaziyete getirebilirsiniz. Yada **manuel** olarak **Apache** kurup onun üzerine bina edebilirsiniz.

**2. Adım **:

**Ubuntu Server** kurulumu gerçekleştirilecek. Daha sonra Ip yapılandırmasını senaryodaki gibi yapılandırın. Şimdi **Nginx** kurulumu gerçekleştirin. Ve şimdi **Nginx**’i **Reverse Proxy**(Ters Vekil) olarak yapılandıralım.

~~~
cd /etc/nginx/sites-available/

sudo vi webmin.test

server {
        listen 80;
        server_name webmin.test;
        return 301 https://192.168.1.100:10000;     —> Bu kısım webmin https(443) ten çalıştığı için webmin servisine özel eklenmiştir. Http(80) çalışan servislerde bu satırı eklemeyiniz.
location / {
        proxy_pass https://192.168.1.100:10000/;
        proxy_set_header X-Forwarded-Host $host;
        proxy_set_header X-Forwarded-Server $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
~~~

Ardından yapılandırmamızın etkin olabilmesi için **sembolik link** oluşturalım.

~~~
sudo ln -s /etc/nginx/sites-available/webmin.test /etc/nginx/sites-enabled/webmin.test

ls -l /etc/nginx/sites-enabled/webmin.test

systemctl restart nginx.service
~~~

![Crepe](/assets/img/nginx-rev-prox-uy/nginx-rev-proxy02.png)

**3. Adım** :

Herşey tamam, şimdi sıra test etme aşamasında. Bunun için **webmin.test** domaini için **dns**’te kaydını oluşturup, istekleri **nginx**’e göndereceğiz, her şey doğru yapılandırıldıysa zaten arka planda **nginx request**’i işleyip **web** sayfasını kullanıcıya gönderecektir. Tabi biz tüm bu işlemler için yerel **dns** olan **hosts** dosyasını kullanacağız.

İlk olarak aşağıdaki komutla kaydımızı oluşturuyoruz. Dosyayı kaydedip, çıkıyoruz

~~~
vi /etc/hosts
~~~

![Crepe](/assets/img/nginx-rev-prox-uy/nginx-rev-proxy03.png)

~~~
ping webmin.test
~~~

![Crepe](/assets/img/nginx-rev-prox-uy/nginx-rev-proxy04.png)

Şimdi sıra geldi tarayıcı üzerinden hizmete erişim. **Adres** çubuğuna **webmin.test/** yazıp **enter**’a basalım ve başarılı bir şekilde bağlandığımızı görelim.

![Crepe](/assets/img/nginx-rev-prox-uy/nginx-rev-proxy05.png)

Normalde işlem tamam ve istediğimiz senaryo gerçekleşti fakat burada bilgi amaçlı olarak şundan da bahsedelim **Webmin https(443)** üzerinden çalıştığı için kullanıcı adı ve şifreyi yazdığınız zaman **login** olamayacaksınız ve aşağıdaki uyarıyı alacaksınız.

![Crepe](/assets/img/nginx-rev-prox-uy/nginx-rev-proxy06.png)

Bu uyarıyı almamak ve **login** olabilmek için biz yukarıda “**return 301 https://192.168.1.100:10000;**” satırını ekledik. Yani bu satır eklenince **Webmin**’e login olunabiliniyor. Yani **Http(80)**’de çalışan bir servis olsaydı ne bu sorun olacaktı ne de bu satırı ekleyecektik. Aslında burada bir trik var **Nginx**’te **virtualhost** dosyasına **2. Adımda** yukarıdaki kadar satır eklemek yerine **Webmin** servisi zaten **443**’ten çalıştığı için “**return 301 https://192.168.1.100:10000**” **SADECE** bu satırla bile işimizi çözüyor olacaktık fakat adres çubuğunda, bağlantı gerçekleştiğinde **domain adı** değil de ip bilgileri yer alıyor olurdu. Aşağıdaki gibi. Halbuki **101** makinesine istek attık.

![Crepe](/assets/img/nginx-rev-prox-uy/nginx-rev-proxy07.png)

Daha fazla bilgi için [https://www.nginx.com/blog/creating-nginx-rewrite-rules/](https://www.nginx.com/blog/creating-nginx-rewrite-rules/) linkini ziyaret edebilirsiniz.