---
layout: post
title: Ubuntu 14.04 LTS Network Ayarları
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [network, ubuntu, linux]
comments: true
---
Sabit ip yapılandırmak için,

~~~
vi /etc/network/interfaces
~~~

İnterface’ye girdikten sonra ilk interface olan eth0 aşağıdaki gibi yapılandırıyoruz.

~~~
auto eth0
iface eth0 inet static
    address 192.168.1.34
    netmask 255.255.255.0
    network 192.168.1.0
    broadcast 192.168.1.255
    gateway 192.168.1.1
    dns-nameservers 8.8.8.8
~~~

![Crepe](/assets/img/ubuntu14-net-conf/u14nc01.png)

Daha sonra servisi restart edelim.

~~~
ifdown eth0 && ifup eth0
~~~

**Dhcp** tarafından ip ataması için aşağıdaki gibi yapılandırabilirsiniz,

~~~
auto eth0
iface eth0 inet dhcp
~~~

**Dns** ayarları yukarıdaki görüldüğü gibi interface içine girilebileceği gibi, aşağıdaki gibi de yapılandırılabilir.

~~~
vi /etc/resolv.conf

nameserver 8.8.8.8
~~~
