---
layout: post
title: Ubuntu 16.04 Bonding Yapılandırması (LACP ve LACP Olmadan)
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [ubuntu, linux, lacp, network]
comments: true
---
**Bonding nedir** : Birden fazla **ethernet** arayüzünü birleştirme işlemidir. **Network teaming** de denebilir. Şöyle ki birden fazla network kartının tek network kartı gibi davranması kartlardan birinin bağlantısının kopması durumunda diğer kart üzerinden kesintisiz bir şekilde bağlantının devam etmesi işlemidir.

**LACP nedir** : **Ether Channel** iki protokol ile yapılır. Bunlardan birincisi cisco protokolü olan **PAgP(Port Aggregation Protokol)** ikincisi ise **LACP(Link Aggregation Control Protokol) IEEE 802.3ad** olan standart protokolüdür. **Ether Channel** iki **switch** arasında 2 yada daha fazla kablo ile bağlantı sağlandığında **switch** in iki yada daha fazla kabloyu tek kablo gibi algılamasını sağlayan protokoldür. **Ether Channel** sayesinde hem yedeklilik(**redundancy**), hem **loadbalans**, hemde yüksek **bandwidth** sağlanmış olur.

**Bonding MOD’ları**;

- mode=0 >> Round-robin(balance-rr), arayüzlere sırası ile paketleri gönderir.
- mode=1 >> Aktif-yedek çalışır(active-backup). Sadece bir arayüz aktiftir.
- mode=2 >> [(Kaynak MAC adresi XOR hedef MAC adresi) % arayüz sayısı] (balance-xor algoritmasına göre paketleri gönderir.
- mode=3 >> Broadcast çeşididir(broadcast). Tüm paketleri tüm arayüzlerden gönderir.
- mode=4 >> IEEE 802.3ad Dynamic link aggregation(802.3ad), LACP. Aktif-aktif çalışır.
- mode=5 >> Toplam yük her arayüzün kendi yüküne göre paylaşılır(balance-tlb).
- mode=6 >> Uyarlamalı yük dengeleme modudur(balance-alb).

Senaryomuz aşağıda şekildeki gibi,

![Crepe](/assets/img/ubuntu16-lacp-co/ubun16-lacp-c01.png)

**1- LACP ile Bonding yapılandırması**,

Şimdi “**/etc/network/interfaces**” dosyasının içini aşağıdaki gibi yapılandırın. Siz kendi **nic** kart isimlerinize göre düzenleyebilirsiniz.

~~~
auto enp6s0f0
iface enp6s0f0 inet manual
    bond-master bond0

auto enp7s0f0
iface enp7s0f0 inet manual
    bond-master bond0

auto enp6s0f1
iface enp6s0f1 inet manual
    bond-master bond1

auto enp7s0f1
iface enp7s0f1 inet manual
    bond-master bond1

auto bond0
iface bond0 inet static
    address 192.168.1.10
    netmask 255.255.255.0
    gateway 192.168.1.1
    dns-nameservers 192.168.1.1
    bond-mode 4
    bond-miimon 100
    bond-lacp-rate fast
    bond-slaves enp6s0f0 enp7s0f0
    bond-downdelay 0
    bond-updelay 0
    bond-xmit_hash_policy 1

auto bond1
iface bond1 inet static
    address 192.168.2.20
    netmask 255.255.255.0
    gateway 192.168.2.1
    dns-nameservers 192.168.2.1
    bond-mode 4
    bond-miimon 100
    bond-lacp-rate fast
    bond-slaves enp6s0f1 enp7s0f1
    bond-downdelay 0
    bond-updelay 0
    bond-xmit_hash_policy 1
~~~

Ardından “**systemctl restart networking.service**” komutu ile network servisini yeniden başlatabilirsiniz, fakat maalesef **Ubuntu**'da network servisi henüz istenilen olgunluğa ulaşmadığı için OS i **reboot** etmeniz gerekecek. Bu yüzden direk sistemi **reboot** komutu ile yeniden başlatın.

Ardından “**ip a**” komutu ile yapılan işlemleri kontrol edebilirsiniz.

![Crepe](/assets/img/ubuntu16-lacp-co/ubun16-lacp-c02.png)

Ayrıca **Bonding** yapılandırmalarına ait detayları aşağıdaki komutla elde edebilirsiniz.

~~~
more /proc/net/bonding/bond0

more /proc/net/bonding/bond1
~~~

**NOT1** : **Bonding** yapılandırmasında **bond0** birinci **slave** olarak görünen nic’in fiziksel adresini devralır ve onu kullanır. Yani **MAC Address** durumu aşağıdaki örnekteki gibi olacaktır(screenshot’tan da teyit edebilirsiniz). Bu durum **mod5** ve **mod6** hariç geçerlidir. Ayrıca **mod1** de de **active-backup** çalışma prensibinden dolayı benzersiz **MAC Addres** kullanır.

~~~
bond0 Link encap:Ethernet HWaddr 50:6B:4B:23:1B:2C
enp6s0f0 Link encap:Ethernet HWaddr 50:6B:4B:23:1B:2C
enp7s0f0 Link encap:Ethernet HWaddr 50:6B:4B:23:1B:2C
~~~

**NOT2** : **OS** tarafında **Bonding**’leri **LACP** olarak yapılandırdığınız taktirde, **Switch** tarafında da karşılık gelen portlara **LACP** yapılandırmasını yapmalısınız. Örnek olarak **Switch** tarafında yapılması gereken ayarı aşağıda paylaşıyorum.

~~~
interface Ethernet X
channel-group 1 mode active
no shutdown

interface Po1
switchport mode trunk
mlag 1
~~~

**2- LACP olmadan normal Bonding yapılandırması**,

~~~
auto enp6s0f0
iface enp6s0f0 inet manual
    bond-master bond0

auto enp7s0f0
iface enp7s0f0 inet manual
    bond-master bond0

auto bond0
iface bond0 inet static
    address 192.168.1.10
    netmask 255.255.255.0
    gateway 192.168.1.1
    dns-nameservers 192.168.1.1
    bond-mode 1
    bond-miimon 100
    bond-slaves enp6s0f0 enp7s0f0
    bond-downdelay 0
    bond-updelay 0
    bond-xmit_hash_policy 1
~~~

**3- İp olmadan nic olarak bonding yapılandırması**,

~~~
auto enp6s0f0
iface enp6s0f0 inet manual
    bond-master bond0

auto enp7s0f0
iface enp7s0f0 inet manual
    bond-master bond0

auto bond0
iface bond0 inet manual
    bond-mode 4
    bond-miimon 100
    bond-lacp-rate fast
    bond-slaves none
    bond-downdelay 200
    bond-updelay 200
    bond-xmit_hash_policy 1
~~~