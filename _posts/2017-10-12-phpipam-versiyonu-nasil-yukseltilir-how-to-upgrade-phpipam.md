---
layout: post
title: PhpIpam Versiyonu Nasıl Yükseltilir - How to Upgrade PhpIpam
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [phpipam, linux, upgrade, git]
comments: true
---
**Php Ipam**’ı git üzerinden indirip kurmuştum, upgrade işlemi de git ile daha basit olacağı için aşağıdaki şekilde upgrade gerçekleştirebiliriz. Tabi panele bağlandığımızda versiyonu yükseltmemiz gerektiği uyarısını önceden alıyorduk.

Sunucuya **login** olduktan sonra aşağıdaki komutla “**/var/www/phpipam/**” gidiyoruz.

~~~
cd /var/www/phpipam/
~~~

Daha sonra **git** komutu ile hali hazırdaki **phpipam** versiyonunu kontrol edelim.

~~~
git describe --tags
~~~

![Crepe](assets/img/phpipam-uprade-c/ipam-upg01.png)

Ardından versiyonu yükseltmek için **checkout** yapıyoruz fakat aşağıdaki gibi uyarı aldık.

~~~
git checkout 1.3
~~~

![Crepe](assets/img/phpipam-uprade-c/ipam-upg02.png)

İşlemi gerçekleştirmek için **f (force)** parametresini kullanıyoruz.

~~~
git checkout -f 1.3
~~~

![Crepe](assets/img/phpipam-uprade-c/ipam-upg03.png)

Daha sonra tekrar versiyon kontrolü yapıp teyit ediyoruz.

~~~
git describe --tags
~~~

![Crepe](assets/img/phpipam-uprade-c/ipam-upg04.png)

Ardından **panele** bağlanıp işlemleri olgunlaştırıp bitirelim. **Upgrade PhpIpam Database** tıklıyoruz.

![Crepe](assets/img/phpipam-uprade-c/ipam-upg05.png)

En son olarak işlemler başarılı olunca aşağıdaki gibi **Dashboard** tıklayıp yeni vesiyonlu sisteme **login** oluyoruz.

![Crepe](assets/img/phpipam-uprade-c/ipam-upg06.png)

Ve versiyonumuzu **1.3** e yükselttik.

![Crepe](assets/img/phpipam-uprade-c/ipam-upg07.png)


Kaynak : [https://phpipam.net/documents/upgrade/](https://phpipam.net/documents/upgrade/)
