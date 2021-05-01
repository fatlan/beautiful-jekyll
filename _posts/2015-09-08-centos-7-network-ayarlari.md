---
layout: post
title: CentOS 7 Network Ayarları
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [centos, linux, network]
comments: true
---
İlk öncesinde interface’leri listeleyip durumları hakkında detaylı bilgi alalım.

~~~
nmcli d
~~~

Benim 1 interface’m var eno ile başlayan ve bağlı olmadığı gözükmekte alttaki resimden anlaşıldığı üzere,

![Crepe](/assets/img/cent7-net-conf/cnw01.png)

Daha sonrasında işlemlere hızlıca başlayalım. İlk olarak interface ayarları için bir editör komutu kullanmak gerekir. Ben default’ta gelen vi komutunu kullanacağım, tavsiyem vim komutudur ama farklı editör komutlarıda kullanarak yapacağımız işlemleri gerçekleştirebilirsiniz. Tabi o komutları sisteme yükleyip entegre ettikten sonra kullanabilirsiniz, ismini bilmiyorsanız tab tuşu ile öğrenebilirsiniz.

~~~
vi /etc/sysconfig/network-scripts/ifcfg-eno1677984
~~~

Daha sonra açılan dosyaya aşağıdaki komutları **insert** tuşuna bastıktan sonra ekleyin.

~~~
BOOTPROTO=static
ONBOOT=yes
IPADDR=192.168.1.34
NETMASK=255.255.255.0
GATEWAY=192.168.1.1
~~~

Yukarıda Static yapıdan bahsediyoruz, eğer **DHCP** üzerinden çalışacaksanız, sadece aşağıdaki komutları girmeniz yeterlidir.

~~~
BOOTPROTO=dhcp
ONBOOT=yes
~~~

Neyse komutları ekledikten son **esc** tuşuna bastıktan sonra ‘**:x**’ enter tuşuna basın ve kaydedip çıkın. Aşağıdaki şekilde görüldüğü gibi.

![Crepe](/assets/img/cent7-net-conf/cnw02.png)

Daha sonrasında aşağıdaki komutu çalıştırarak servisi **restart** edin.

~~~
systemctl restart network
~~~

Şimdi **DNS** adreslerini girelim.
Bunun için yukardaki interface dosyasının içine dns adreslerini girerek yapılabilirdi. Bu birinci yöntem

~~~
DNS1=’dns ip adresi’
DNS2=’dns ip adresi’
~~~

İkinci yöntem aşağıdaki dosyasının içine kayıtları girerek yapabilirsiniz.

~~~
vi /etc/sysconfig/network
~~~

~~~
HOSTNAME=CentOS7.domainadı(makina domain ortamında çalışacaksa makina isminden sonra .domainadı’nı girebilirsiniz.)
DNS1=’dns ip adresi’
DNS2=’dns ip adresi’
SEARCH=Domainadı(makina domain ortamında çalışacaksa .domainadı’nı girebilirsiniz.)
~~~

Üçüncü yöntem, ben bunu kullanacağım

~~~
vi /etc/resolv.conf

nameserver 8.8.8.8
nameserver 8.8.4.4
~~~

![Crepe](/assets/img/cent7-net-conf/cnw03.png)

Daha sonra tüm ayarlarınız doğru olduğundan emin olmak için ve dns’in isim çözdüğünü görmek için herhangi bir domain’e **ping** atın.

![Crepe](/assets/img/cent7-net-conf/cnw04.png)

Daha sonra konunun başındaki ilk komutu tekrar çalıştırın ve bağlı olduğunu görün.

~~~
nmcli d
~~~

![Crepe](/assets/img/cent7-net-conf/cnw05.png)