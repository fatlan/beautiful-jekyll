---
layout: post
title: Disk’iniz SSD mi yoksa HDD mi Öğrenmenin Yolları
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [disk, linux, ssd, hdd]
comments: true
---
SSD nedir yada HDD ile arasındaki fark'a çok girmeden direk sistem üzerinde bunu nasıl teyitli bir şekilde öğrenebiliriz bunun yollarını anlatmaya çalışacağım. İlk öncesinde şunu bilmek gerekiyor ki rotational denilen bir değer vardır ve bu değer sayesinde diskimizin SSD yada HDD mi olduğunu elde edeceğiz. Yani

- rotational(ROTA) değeri 1 ise HDD
- rotational(ROTA) değeri 0 ise SSD dir.

Bu değeri elde etmenin bir kaç yolu var ve sırasıyla bahsedeceğiz. Ben işlemleri iki makina üzerinde yapacağım biri gerçek makina SSD'li, diğeri sanal makina HDD'li olan ki hepsini görün ve aklınızda soru işareti kalmasın.

**Birinci yöntem** :

**lsblk** komutu, bu komutu aşağıdaki parametreler ile çağırdığınızda size **ROTA** bilgisini dönecektir.

~~~
lsblk -do name,rota
~~~

Sanal makina, **ROTA** değeri **1** döndüğünden anladık ki bu **HDD**

![Crepe](assets/img/linux-ssd-hdd/lin-ssd-hd01.png)

~~~
lsblk -do name,rota yada lsblk -d -o name,rota
~~~

Gerçek makina, **ROTA** değeri **0** döndüğünden anladık ki bu **SSD**

![Crepe](assets/img/linux-ssd-hdd/lin-ssd-hd02.png)

**İkinci yöntem** :

Gene **lsblk** komutu ile ama bu sefer “**t**” parametresi ile

~~~
lsblk -t
~~~

Sanal makina, **ROTA** değeri **1** döndüğünden anladık ki bu **HDD**

![Crepe](assets/img/linux-ssd-hdd/lin-ssd-hd03.png)

~~~
lsblk -t
~~~

Gerçek makina, **ROTA** değeri **0** döndüğünden anladık ki bu **SSD**

![Crepe](assets/img/linux-ssd-hdd/lin-ssd-hd04.png)

**Üçüncü yöntem** :

**Smartctl** komutu ile ki burada direk “**Rotation Rate:Solid State Device**” ibaresini görmelisiniz.

~~~
smartctl -a /dev/sda
~~~

Sanal makina, **Rotation Rate** karşılık ibare bulunmuyor, **SSD** DEĞİL

![Crepe](assets/img/linux-ssd-hdd/lin-ssd-hd05.png)

~~~
smartctl -a /dev/sda
~~~

Gerçek makina, **Rotation Rate** karşılık ibare var ve disk **SSD**

![Crepe](assets/img/linux-ssd-hdd/lin-ssd-hd06.png)

**Dördüncü yöntem** :

Bu yöntem ise **cat** komutu yardımıyla direk dosyadan **rotational** değerini elde ederek

~~~
cat /sys/block/sda
~~~

Sanal makina, **rotational** değeri **1** döndüğünden anladık ki bu **HDD**

![Crepe](assets/img/linux-ssd-hdd/lin-ssd-hd07.png)

~~~
cat /sys/block/sda
~~~

Gerçek makina, **rotational** değeri **0** döndüğünden anladık ki bu **SSD**

![Crepe](assets/img/linux-ssd-hdd/lin-ssd-hd08.png)

