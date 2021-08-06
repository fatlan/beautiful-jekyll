---
layout: post
title: Ubuntu Netplan Bonding ve Vlan Yapılandırması
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [ubuntu, linux, network, bonding, vlan]
comments: true
---

/etc/netplan/ altında ilgili config yaml dosyası içerisinde aşağıdaki gibi bond0 altında vlan 13 ve 14 yapılandırmasını kendi ortamlarınız için uyarlayabilirsiniz.


~~~yaml
network:
  version: 2
  bonds:
    bond0:
      dhcp4: no
      interfaces: [ens1f0, ens2f0]
      mtu: 9000
      parameters:
        lacp-rate: fast
        mode: 802.3ad
        transmit-hash-policy: layer2
        mii-monitor-interval: 100
  ethernets:
    ens1f0: {}
    ens2f0: {}
    eno1: {}
  vlans:
    bond0.13:
      id: 13
      link: bond0
      mtu: 9000
      addresses: [ "10.1.13.99/24" ]
    bond0.14:
      id: 14
      link: bond0
      mtu: 1500
      addresses: [ "10.1.14.99/24" ]
      gateway4: 10.1.14.1
      nameservers:
        search: [fatlan.com]
        addresses: [ "10.1.150.150", "10.1.160.160" ]
~~~

Akabinde yapılandırmanın geçerli olması için
~~~
sudo netplan apply
~~~

Aşağıdaki şekilde de bonding yapılandırmalarının doğruluğunu test edebilirsiniz
~~~
cat /proc/net/bonding/bond0

mii-tool
~~~

Her ihtimale karşı ubuntu linux için sunucuyu reboot etmek her zaman iyidir
~~~
sudo reboot
~~~

<br>
rf: [1](https://netplan.io/examples/)|[2](https://www.snel.com/support/how-to-set-up-lacp-bonding-on-ubuntu-18-04-with-netplan/)