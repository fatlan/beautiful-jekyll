---
layout: post
title: Zen Load Balancer Interfaces Ayarları - Bölüm 3
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [zen, loadbalancer, proxy, network, linux]
comments: true
---
Herhangi bir tarayıcı yardımı ile yönetim paneline bağlandıktan sonra **Settings-Interfaces** kısmından **Zenload Balancer** için ekli olan interface’ler için ip ayarları görebilir ve yapılandırabiliriz.

![Crepe](assets/img/zen-interface-bolum3/ze-int-01.png)

Aşağıda görüldüğü gibi kurulum aşamasında **eth0** dış ip olarak yapılandırdım ve bunu **ZenLoad Balancer** yönetimi için kullanacağım. Şimdi **farm** için **eth0:1** **request**’leri alacak bir sanal dış ip daha atayacağım, ardından **eth1**’i iç ip ile gene **farm** için içerdeki sunuculara ulaşması için yapılandıracağım.

![Crepe](assets/img/zen-interface-bolum3/ze-int-02.png)

İlk önce **eth1**’i aşağıda görüldüğü gibi **edit network interface** diyerek yapılandırıyorum,

![Crepe](assets/img/zen-interface-bolum3/ze-int-03.png)

Gerekli bilgileri girdikten sonra **Save & Up!** ile işlemleri kaydedip, sonlandırıyorum.

![Crepe](assets/img/zen-interface-bolum3/ze-int-04.png)

Şimdi yukarda da bahsettiğim gibi gelen istekleri alabilmesi için **eth0** üzerinden sanal **interface** yapılandıralım **eth0:1**, **add virtual network interface** diyerek bu işlemi yapabiliriz.

![Crepe](assets/img/zen-interface-bolum3/ze-int-05.png)

Gerekli bilgileri girdikten sonra **save virtual interface** diyerek işlemleri kaydedip sonlandırabilirsiniz.

![Crepe](assets/img/zen-interface-bolum3/ze-int-06.png)

Ve tüm işlemlerin ardından durum aşağıdaki gibidir. İki **interface** biri iç biri dış, dış interface’de bir de sanal dış interface ve bir tane **gateway** mevcut. Tablo kafanızı karıştırmasın dış ip’ler aynı **subnet**’te olduğu için aynı **gateway** yazılabilir, aynı **gateway** üzerinden çıkacaktır. Ve üçüde **Up** durumdadır.

![Crepe](assets/img/zen-interface-bolum3/ze-int-07.png)
