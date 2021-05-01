---
layout: post
title: Centos Clone Sonrası Yapılması Gereken İşlemler
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [centos, clone, cloud, linux, vmware]
comments: true
---
**VMWare ESX/ESXİ** ortamınızda **CentOS** sunucular için **Clone** aldığınızda aşağıda belirttiğim bir kaç basit adımı yapmanız gerekmektedir. Eğer bu işlemleri yapmayı atlarsanız **network adaptörü**nüz **up** duruma geçmeyecektir ve aşağıdaki hatayı alacaksınız. Diğer bir değişle aşağıdaki hatayı alıyorsanız bilinki o makine **clon** bir sunucudur ve muhtemelen aşağıdaki adımlar atlanmıştır.

**Hata** : “**bringing up interface eth0 device eth0 does not seem to be present delaying initialization**”

![Crepe](/assets/img/cen6-clone-after/cent-clon-a01.png)

Yapılması gerekenler ise;

1- **rm** komutu ile **70-persistent-net.rules** dosyasını silin. Bu dosya **ethetnet** kartınızın bilgilerinin saklandığı dosyadır. Örnek çıktı aşağıdaki gibidir.

~~~
// # PCI device 0x14e4:0x1680 (tg3)

SUBSYSTEM==”net”, ACTION==”add”, DRIVERS==”?*”, ATTR{address}==”b5:ac:6c:84:31:r5′′, ATTR{dev_id}==”0x0′′, ATTR{type}==”1′′, KERNEL==”eth*”, NAME=”eth0′′ //

rm /etc/udev/rules.d/70-persistent-net.rules
~~~

2- **vi** yada alternatif bir editör ile **network**(**ip**,**subnet**,**gateway**,**hwaddr**) ayarlarının bulunduğu dosyadan **HWADDRESS** adres satırını silin.

~~~
vi /etc/syscon g/network-scripts/ifcfg-eth0
~~~

3- Sunucunuzun **hostname**‘ini değiştirin

![Crepe](/assets/img/cen6-clone-after/cent-clon-a02.png)

Tüm bu işlemleri yaptıktan sonra sunucuyu **reboot** edin ve işlem tamamdır ve ayrıca hata da kaybolacaktır. Tabi **clone** sunucuda ip vs. değiştirdiğinizi kabul ediyorum. =)

**VirtualBox** kullananlar varsa eğer, **clone** alırken aşağıdaki gibi “**Reinitialize the MAC address of all network cards**” seçeneğini **check**(aktif) ederse **mac** adresi, **clone** sunucu için otomatik olarak değişecektir ve yukarıdaki **1** seçenek otomatik olarak yapılmış olacaktır. **VirtualBox** için **2** ve **3** seçeneği yapıp **restart** edebilirsiniz.

![Crepe](/assets/img/cen6-clone-after/cent-clon-a03.png)
