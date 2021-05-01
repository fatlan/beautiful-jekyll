---
layout: post
title: RedHat/CentOS Bonding Yapılandırması (LACP ve LACP Olmadan)
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [redhat, centos, network, lacp, linux]
comments: true
---
**Bonding nedir**.? : Birden fazla ethernet arayüzünü birleştirme işlemidir. Network teaming de denebilir. Şöyle ki birden fazla network kartının tek network kartı gibi davranması kartlardan birinin bağlantısının kopması durumunda diğer kart üzerinden kesintisiz bir şekilde bağlantının devam etmesi işlemidir.

**LACP nedir**.? : **Ether Channel** iki **protokol** ile yapılır. Bunlardan birincisi **cisco** protokolü olan **PAgP(Port Aggregation Protokol)** ikincisi ise **LACP(Link Aggregation Control Protokol)** **IEEE 802.3ad** olan **standart** protokolüdür. **Ether Channel** iki **switch** arasında 2 yada daha fazla kablo ile bağlantı sağlandığında **switch** in iki yada daha fazla kabloyu tek kablo gibi algılamasını sağlayan protokoldür. **Ether Channel** sayesinde hem yedeklilik(**redundancy**), hem **loadbalans**, hemde yüksek **bandwidth** sağlanmış olur.

**Bonding MOD’ları**

- mode=0 >> Round-robin(balance-rr), arayüzlere sırası ile paketleri gönderir.
- mode=1 >> Aktif-yedek çalışır(active-backup). Sadece bir arayüz aktiftir.
- mode=2 >> [(Kaynak MAC adresi XOR hedef MAC adresi) % arayüz sayısı] (balance-xor) algoritmasına göre paketleri gönderir.
- mode=3 >> Broadcast çeşididir(broadcast). Tüm paketleri tüm arayüzlerden gönderir.
- mode=4 >> IEEE 802.3ad Dynamic link aggregation(802.3ad), LACP. Aktif-aktif çalışır.
- mode=5 >> Toplam yük her arayüzün kendi yüküne göre paylaşılır(balance-tlb).
- mode=6 >> Uyarlamalı yük dengeleme modudur(balance-alb)

Senaryomuz aşağıda şekildeki gibi,

![Crepe](assets/img/red-ceos-lacp-con/lacp-rc01.png)

1- **LACP** ile **Bonding** yapılandırması, **cd** komutu ile “**/etc/syscon g/network-scripts/**” **directory**'sine gidin. Ardından **vi** yada başka bir editör aracılığı ile **uplink** gelen “**ifcfg-eth0 ve ifcfg-eth1**” konfig dosyalarının içini aşağıdaki gibi yapılandırın.

~~~
[root@localhost network-scripts]# vi ifcfg-eth0


DEVICE=eth0
TYPE=Ethernet
BOOTPROTO=none
ONBOOT=yes
NM_CONTROLLED=no
IPV6INIT=no
MASTER=bond0
SLAVE=yes
~~~

~~~
[root@localhost network-scripts]# vi ifcfg-eth1

DEVICE=eth1
TYPE=Ethernet
BOOTPROTO=none
ONBOOT=yes
NM_CONTROLLED=no
IPV6INIT=no
MASTER=bond0
SLAVE=yes
~~~

Daha sonra aynı dizin içinde “**ifcfg-bond0**” dosyasını oluşturup aşağıdaki gibi yapılandırın.

~~~
[root@localhost network-scripts]# vi ifcfg-bond0

DEVICE=bond0
TYPE=bond
NAME=bond0
BONDING_MASTER=yes
BOOTPROTO=none
ONBOOT=yes
IPADDR=192.168.1.10
PREFIX=24
GATEWAY=192.168.1.1
BONDING_OPTS="mode=4 miimon=100 lacp_rate=1"
~~~

Son olarak aşağıdaki komutla **network** servisini yeniden başlatalım ve işlemleri tamamlayalım. Servisi yeniden başlatırken uyarı/uyarılar alabilirsiniz. Üst üste iki kere de servisi yeniden başlatabilirsiniz yada sunucuyu **reboot** edebilirsiniz.

**Redhat/CentOS 6.x için,**

~~~
/etc/init.d/network restart ya da service network restart
~~~

**Redhat/CentOS 7.x için,**

~~~
systemctl restart network.service
~~~

Ardından “**ip a**” komutu ile yapılan işlemleri kontrol edebilirsiniz.

![Crepe](assets/img/red-ceos-lacp-con/lacp-rc02.png)

Ayrıca **Bonding** yapılandırmalarına ait detayları aşağıdaki komutla elde edebilirsiniz.

~~~
more /proc/net/bonding/bond0
~~~

**NOT1** : **Bonding** yapılandırmasında **bond0** birinci **slave** olarak görünen nic’in ziksel adresini devralır ve onu kullanır. Yani **MAC Address** durumu aşağıdaki örnekteki gibi olacaktır(screenshot’tan da teyit edebilirsiniz). Bu durum **mod5** ve **mod6** hariç geçerlidir. Ayrıca **mod1** de de **active-backup** çalışma prensibinden dolayı benzersiz **MAC Addres** kullanır.

~~~
bond0 Link encap:Ethernet HWaddr 04:09:73:ca:a6:70
eth0 Link encap:Ethernet HWaddr 04:09:73:ca:a6:70
eth1 Link encap:Ethernet HWaddr 04:09:73:ca:a6:70
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

**2- LACP olmadan Bonding yapılandırması,**

“**ifcfg-eth0**” ve “**ifcfg-eth1**” aynen yukarıdaki gibi yapılandırdıktan sonra, sadece “**ifcfg-bond0**” aşağıdaki gibi yapılandırın(**‘mode**’ değişikliği yapıldı).

~~~
[root@localhost network-scripts]# vi ifcfg-bond0

DEVICE=bond0
TYPE=bond
NAME=bond0
BONDING_MASTER=yes
BOOTPROTO=none
ONBOOT=yes
IPADDR=192.168.1.10
PREFIX=24
GATEWAY=192.168.1.1
BONDING_OPTS="mode=2 miimon=100"
~~~
