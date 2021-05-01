---
layout: post
title: PFSENSE İle Yerel Ağda DNS(53 Portu) Manipüle Etme
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [pfsense, freebsd, linux, dns, network]
comments: true
---
**OpenSource** proje olan **Pfsense** ile bağlantılı olduğunuz tüm ağlar için farklı **DNS** sunucu(**53 port**u) kullanmasını engellemek, daha doğrusu yönlendirmek ve bu yaptığınız işlemlerde son kullanıcıya müdahele edilmediği için haberleri dahi olmadan yapmak mümkün ve bir o kadar kolaydır. Dolayısıyla kullanıcılar kendi bilgisyarlarında **DNS** adresini değiştirse dahi **firewall**’dan sizin yapacağınız yönlendirme kuralına takılacak ve kendi yazdığı **DNS** ip’sine değil sizin yönlendirdiğiniz **DNS** sunucu/sunucular’ına gidecektir. Ayrıca bu durumdan haberi dahi olmayacaktır.

Bu işlemi ilgili interface, **subnet** yada ip için yapabilirsiniz.

**Pfsense** **login** olduktan sonra “**Firewall / NAT / Port Forward**” sekmesine tıklayın.

![Crepe](assets/img/pfse-dns-manpl/pf-dns-mpl01.png)

Ardından “Add” diyerek nat kuralını oluşturmaya başlayın.

![Crepe](assets/img/pfse-dns-manpl/pf-dns-mpl02.png)

Ardından aşağıdaki ss gibi kural yapısını oluşturun.

![Crepe](assets/img/pfse-dns-manpl/pf-dns-mpl03.png)

**NoT** : Yalnız burada bir şey belirtmem gerek. **DNS** sunucu yada sunucularınız kuralı işlettiğiniz **subnet**in içinde ise “**Invert match.**” seçeneğini aktif edip o ip yada ip’leri belirterek hariç tutmanız gerekmektedir. Çünkü ağın içindeki **DNS** sunucuları hariç tutmazsanız yönlendirme yaptıktan sonra bu **DNS** sunucularda dışırıya **53** portundan çıkmayacağı için ve kendinde olmayan kayıtları **root** ve diğer global **DNS** sunuculardan öğrenemeyeceği için kullanıcılara yansıyacak ve dolayısıyla iç **DNS** sunucularınız kendinde olmayan kayıtları çözemeyeceği için kullanıcılarda ip'ye karşılık gelen domainleri çözemeyecektir.

Aşağıda örnek gösterilmiştir.

![Crepe](assets/img/pfse-dns-manpl/pf-dns-mpl04.png)
