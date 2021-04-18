---
layout: post
title: E-Unable To Locate Package
#subtitle: Each post also has a subtitle
gh-repo: fatlan/fatlan.github.io
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [debian, linux]
comments: true
---

![Crepe](assets/img/unable-to-locate-package/unable01.png)

Linux sistemlerde yüklemeye çalıştığımız paket sonrası bu hatayı alıyorsak eğer, ya paket ismini yanlış yazıyorsunuzdur ya da repository’den kaynaklanmaktadır. Güncel source kaynaklarına ulaşmak için google.com (http://www.google.com)‘dan ‘source list generator’ diye aratabilirsiniz veya sistem debian ise direk [debgen.simplylinux.ch](http://debgen.simplylinux.ch) <br> adresine girip aşağıdaki gibi size uygun seçenekleri aktif edip,

![Crepe](assets/img/unable-to-locate-package/unable02.png)

"Generate sources.list" butonuna tıkladıktan sonra,

![Crepe](assets/img/unable-to-locate-package/unable03.png)

deb’le başlayan kısmı kopyaladıktan sonra "/etc/apt/sources.list" içine diğer repo adreslerinin disable olması için satır başlarına # işaretini koyduktan sonra yapıştırın. Ardından "apt update" işlemini gerçekleştirin ve daha sonrasında tekrar paket kurulumunu gerçekleştirin. Paket ısrarla kurulmak istemiyorsa sistemdeki paket kurulum ismi farklıdır.

