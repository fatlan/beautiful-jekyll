---
layout: post
title: Mikrotik Kullanarak Dışardan İçeriye NAT-PAT İşlemleri
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [mikrotik, nat, pat, network, linux, port]
comments: true
---
Bu yazımızda ise **NAT-PAT** hakkında bilgi sahibi olacağız ve **Mikrotik** bir cihaz üzerinden dış dünyadan içeriye yönlendireceğimiz hizmetlerin kullanılmasını sağlayacağız. Bu işlemler sınırlı olup, dışardan içeriye tüm networke erişim değil sadece **port** yönlendirmesi araclığı ile belirdeğimiz hizmetlerin kullanılmasını sağlayacağız. Nedir.? bu hizmetler derseniz **NAT-PAT** yardımıyla bu yazımızda dışardan içeriye **HTTP** ve **RDP** hizmetlerinin kullanılmasını sağlayacağız. Fakat **HTTP** servisini default olarak belirli **80** portu üzerinden, **RDP** hizmetini ise **3389** değilde **3344** portu üzerinden kullanılmasını sağlayacağız. Bunu böyle yapmamım sebebi ise konuyu biraz daha iyi anlayıp, daha çok hakim olup kavramaktır. İşlemlere başlamadan önce **NAT-PAT** kavramlarına bir göz atalım.

**NAT** : **Network Address Translation**(Ağ Adresi Dönüştürme) demektir. Yani iç bacak interface ve o ağdaki ip’lerin internete çıkarken dış bacak **interface**’deki ip adresine dönüşüp nete çıkması diyebiliriz. Daha geniş bilgiye [viki](https://tr.wikipedia.org/wiki/NAT) adresinden ulaşabilirsiniz.

**PAT** : **Port Address Translation**(Port Yöndendirme) demektir. **NAT** ile birlikte kullanılır ama **PAT** dış ağdan gelen portu iç ağa aynı **port** olarak yada değişime uğratarak yönlendirme işlemidir. Daha geniş bilgiye [viki](https://tr.wikipedia.org/wiki/Port_address_translation) adresinden ulaşabilirsiniz.

Şimdi senaryomuzda **Mikrotik** cihazımızın arkasında **IIS** servisi çalışan bir **Windows** Sunucu bulunsun ve bu sunucu sadece iç (**private**) ağ içerisini dahil olsun ama dış dünyaya hizmet verebilsin. Yani dış dünyadan gelen **requestlere** cevap dönüp, **IIS** hizmetinin sonucu olan yapıyı kullanıcıya sunsun. Yani **Mikrotik**’in dış dünyaya açık olan **IP** sine **80** sorgusu gelecek ve biz o sorguyu alıp ilk önce **iç ağ**a çevireceğiz fakat iç ağa çevirirkende tüm **network** değil sadece **port** yönlendirmesi yapıp ordan **IIS** sunucusuna atıp, geri dönüş olarak tam tersi işlemlerle **index** sayfasını kullanıcıya ulaştıracağız.

Teknik olarak işlemlere başlamadan önce **default**’ta gelen **Mikrotik**’in Kon gürayon erişiminin sağlandığı **80** servisini Servisler kısmından aşağıdaki gibi kapatalım.

![Crepe](/assets/img/mikrotik-ingress-nat-pat/mikrotik-natpat01.png)

Ardından **IP-Firewall-NAT** sekmesinden **+** işaretine basın ve aşağıdaki gibi **konfigüre** edin.

![Crepe](/assets/img/mikrotik-ingress-nat-pat/mikrotik-natpat02.png)

Daha sonra görünüm bu şekilde olacak.

![Crepe](/assets/img/mikrotik-ingress-nat-pat/mikrotik-natpat03.png)

**Telnet** yada **Ssh** üzerinden de aşağıdaki komutu çalıştırarak yapabilirsiniz.

~~~
ip rewall nat add chain=dstnat in-interface=pppoe-out1 protocol=tcp dst-port=80 action=dst-nat to-addresses=YOUR SERVER ADDRESS to-ports=80 comment=”” disabled=no
~~~

Şimdi **Mikrotik**’in **dış** bacağına sorgu göndererek, **test** edip başarılı bir şekilde sonuç aldığımızı görelim.

![Crepe](/assets/img/mikrotik-ingress-nat-pat/mikrotik-natpat04.png)

Şimdi **RDP** senaryosunu gerçekleşrirelim. **IIS** senaryosuyla aynı olmakla beraber tek farkı **RDP portu 3389** değil **3344** olarak hizmet verecek. Yani dışardan sorgu gelince **3389** olarak alacağız fakat **RDP** hizmetini veren sunucuya **3344** olarak ileteceğiz. Geri dönüş olarak aynı işlemleri tersi olarak hizmet verecek.

Bunun için sunucu üzerinde ki default **RDP portunu 3344** ile değiştirmeniz gerekli. Bu işlemi yaptıktan sonra tekrar **Mikrotik’te IP-Firewall-NAT** sekmesinden **+** işaretine basın ve aşağıdaki gibi konfigüre edin. Buradaki fark gördüğünüz gibi **3389** ile gelen **port** isteğini arkada hizmeti veren sunucu için **3344** olarak belirleyip yönlendiriyoruz.

![Crepe](/assets/img/mikrotik-ingress-nat-pat/mikrotik-natpat05.png)

Ve kurallar tablosundaki son görünüm aşağıdaki gibi olacaktır.

![Crepe](/assets/img/mikrotik-ingress-nat-pat/mikrotik-natpat06.png)

Şimdi **Mikrotik**’in dış bacağına **RDP** için sorgu göndererek, **test** edip başarılı bir şekilde sonuç aldığımızı görelim.

![Crepe](/assets/img/mikrotik-ingress-nat-pat/mikrotik-natpat07.png)

