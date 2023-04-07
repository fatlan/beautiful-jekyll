---
layout: post
title: Namespace
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [linux, namespace, kubernetes, docker, container, cgroups, ns, network, isolation, netns, network namespace]
comments: true
---

**Linux çekirdek** kaynaklarını her bir **processes**(işlem kümesi) için farklı kaynaklar olarak **izole** etmeye yarayan **Linux kernel** özelliğidir.

![Crepe](/assets/img/ntwrk-ns/ns-gl01.png)

**NS** Çeşitleri;

- **Mount Points (mnt)** : Mount pointleri kontrol eden ns’dir.
- **Process ID (pid)**  : Her bir ns’in processes’leri için bağımsız PIDs üretmek için kullanılan ns’dir.
- **Network (net - network devices, stacks, ports, etc.)** : Network stack’lerini sanallaştırmak için kulanılan ns’dir.
- **Inter-process Communication (ipc - System V IPC, POSIX message queues)** : Süreçleri SysV tarzı süreçler arası iletişimden yalıtmak için kullanılan ns’dir.
- **UTS (UNIX Time-Sharing - Hostname and NIS domain name)** : Tek bir sistemin farklı işlemlere farklı host ve domain names sahip gibi görünmesini sağlar.
- **User ID (User and group IDs)** : Birden çok işlem kümesinde hem ayrıcalık izolasyonu hem de kullanıcı kimliği ayrımı sağlayan bir özelliktir.
- **Control group (cgroup) Namespace** : Process'in üyesi olduğu control group’unu kimliğini gizler.
- **Time Namespace** : UTS ad alanına benzer bir şekilde farklı sistem zamanlarını görmek için kullanılan ns’dir.
- **Proposed namespaces(syslog namespace)**  : Huawei'de bir mühendis olan Rui Xiang tarafından önerildi, ancak linux çekirdeğiyle birleştirilmedi.

![Crepe](/assets/img/ntwrk-ns/ns-gl02.png)

![Crepe](/assets/img/ntwrk-ns/ns-gl02.png)

Aşağıdaki alıştırmalar **virtualbox** üzerinden sanal sistemlerde gerçekleştirlmiştir. Yukarıdaki ss'lerden de anlaşılacağı üzere "**Host Network Manager**"dan "**vboxnet0**" oluşturduktan sonra sanal makine üzerinde iki **nic** oluşturup "**Host-only Adapter**" tercih edilmiştir.

EXAM (**Namespace**’ler **persistent** değildir, **reboot**’ta kaybolurlar. **Script** yapılabilir)
~~~
lsns
~~~

**Net NS** ekleyelim(**host terminal**'den),
~~~
sudo ip netns add ns01
sudo ip netns add ns02
~~~

Listeleyelim,
~~~
sudo ip netns list

ls /var/run/netns/
~~~

**NS interfaces**’leri listeleyelim(**loopback interface**’lerini görmekteyiz)
~~~
sudo ip netns exec ns01 ip a (sudo ip -n ns01 ip a)
sudo ip netns exec ns02 ip a

sudo ip -all netns exec ip a
~~~

**2** yeni **terminal** açalım ve bu **terminal**leri **ns**’lere bağlayalım,
~~~
termianl1= sudo ip netns exec ns01 bash, ip a
terminal2= sudo ip netns exec ns02 bash, ip a
~~~

**Host**’taki **interfaces**’leri **ns**’lere atayalım(**ns bash**’inden),
~~~
nmcli d, sudo nmcli dev connect enp0s9, nmcli d
nmcli d, sudo nmcli dev connect enp0s10, nmcli d
~~~

**Host terminal**den,
~~~
sudo ip link set enp0s9 netns ns01
sudo ip link set enp0s10 netns ns02
~~~
~~~
sudo ip netns exec ns01 ip addr add 192.168.56.121/24 dev enp0s9
sudo ip netns exec ns02 ip addr add 192.168.56.122/24 dev enp0s10
~~~

**NS terminal**’lerinden,
~~~
ip a
ip a
~~~

**NS01 terminal**’inden,
~~~
ip link set lo up
ip link set enp0s9 up
~~~

**NS02 terminal**’inden,
~~~
ip link set lo up
ip link set enp0s10 up
~~~

**Host** makinesinden **ns interface**’lerine **ping** atılabilir,
~~~
ping 192.168.56.121
ping 192.168.56.122
~~~

Şimdi **enp0s9** ve **enp0s10** **interface**lerini **down** yapıp **ns**’lerden alıp tekrar **host**’a(**netns 1**) geri verelim,
~~~
ip netns exec ns01 ip link set enp0s9 down
ip netns exec ns02 ip link set enp0s10 down

ip netns exec ns01 ip link set enp0s9 netns 1
ip netns exec ns02 ip link set enp0s10 netns 1

ip a

nmcli d
~~~

**VETH(Virtual Ethernet) interface** tanımlayalım.

- veth tüp geçit olarak düşünülebilir.
- veth’in iki ucu tanımlandıktan sonra başlangıç ve bitiş noktası ayarlanmalıdır.
- NS’leri birbiriyle haberleşmesi için virtual switch(iki ucu olduğundan) tanımlanmalıdır, bridge ya da ovs.

**Virtual Switch**’de **default namspace**(**host** - **netns 1**) bağlı olacak,

**Veth nic** uçları tanımlayalım(**host terminal**’de),
~~~
sudo ip link add ns01-eth0 type veth peer name ns02-eth0
nmcli d

sudo ip link set ns01-eth0 netns ns01
sudo ip link set ns02-eth0 netns ns02
~~~

**NS01** term,
~~~
ip link
~~~

**NS02** term,
~~~
ip link
~~~

**IP**’leri atayalım(**host terminal**’de),
~~~
sudo ip netns exec ns01 ip addr add 192.168.56.141/24 dev ns01-eth0
sudo ip netns exec ns02 ip addr add 192.168.56.142/24 dev ns02-eth0
~~~

**NS01** term,
~~~
ip a
~~~

**NS02** term,
~~~
ip a
~~~

**Nic**’leri **up** yapalım(**host terminal**’de),
~~~
sudo ip netns exec ns01 ip link set ns01-eth0 up
sudo ip netns exec ns02 ip link set ns02-eth0 up
sudo ip -all netns exec ip a
~~~

**NS01 terminal**inden **NS02 IP**’sine **ping** atalım(**default** dan **ns01** ve **ns02** lere **ping** atamıyoruz zaten onu istemiştik).
~~~
ping 192.168.56.142
~~~





ref: [https://www.admin-magazine.com/Articles/The-practical-benefits-of-network-namespaces/(offset)/3](https://www.admin-magazine.com/Articles/The-practical-benefits-of-network-namespaces/(offset)/3)