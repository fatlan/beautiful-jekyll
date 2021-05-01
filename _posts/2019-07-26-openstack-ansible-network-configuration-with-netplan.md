---
layout: post
title: OpenStack Ansible Network Configuration, Bonding, Mtu, Vlan, Br-vlan, Br-vxlan With NETPLAN On Ubuntu 18
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [openstack, network, linux, ubuntu]
comments: true
---
![Crepe](/assets/img/openstack-ansib-netplan-ubuntu/op-ub-net-conf01.png)

İlk önce aşağıdaki komutla apt reposunu güncelleyin.

~~~
apt update
~~~

Ardından “**bridge-utils**” paketini sisteme kurun.

~~~
apt install bridge-utils
~~~

Daha sonra “**/etc/netplan/50-cloud-init.yaml**” dosyasını aşağıdaki gibi yapılandırabilirsiniz. Tabi siz kullanacağınız şekilde yapılandırabilirsiniz. Ben aşağıdaki örnekte hem bonding, hem vlan, hem bridge vlan, hem bridge vxlan, hem de mtu size olarak jumbo frame kullandım. Ayrıca nfs ve iscsi yide ayrı vlan larla yapılandırdım. Siz ortamınızda gerekli olan yapılandırmaya göre şekillendirebilirsiniz.

~~~
#This le is generated from information provided by
#the datasource. Changes to it will not persist across an instance.
#To disable cloud-init’s network con guration capabilities, write a le
#/etc/cloud/cloud.cfg.d/99-disable-network-con g.cfg with the following:
#network: {con g: disabled}
network:
bonds:
bond0:
interfaces:
– enp6s0f0
– enp7s0f0
mtu: 9000
parameters:
lacp-rate: fast
mode: 802.3ad
transmit-hash-policy: layer2
bond1:
interfaces:
– enp6s0f1
– enp7s0f1
parameters:
lacp-rate: fast
mode: 802.3ad
transmit-hash-policy: layer2
ethernets:
eno1: {}
eno2: {}
enp0s20f0u1u6: {}
enp6s0f0:
mtu: 9000
enp6s0f1: {}
enp7s0f0:
mtu: 9000
enp7s0f1: {}
version: 2
vlans:
bond0.10:
id: 10
link: bond0
mtu: 9000
bond0.20:
id: 20
link: bond0
mtu: 9000
bond0.21:
id: 21
link: bond0
mtu: 9000
bond1.30:
id: 30
link: bond1
bond0.32:
id: 32
link: bond0
mtu: 9000
bridges:
br-ext:
interfaces: [bond0.32]
addresses:
– 10.1.10.34/24
gateway4: 10.1.10.1
nameservers:
addresses:
– 10.10.10.5
parameters:
stp: false
forward-delay: 0
mtu: 9000
br-mgmt:
interfaces: [bond0.10]
addresses:
– 172.29.236.180/22
parameters:
stp: false
forward-delay: 0
mtu: 9000
br-iscsi:
interfaces: [bond0.20]
addresses:
– 172.29.244.180/22
parameters:
stp: false
forward-delay: 0
mtu: 9000
br-nfs:
interfaces: [bond0.21]
addresses:
– 172.29.248.180/22
parameters:
stp: false
forward-delay: 0
mtu: 9000
br-vxlan:
interfaces: [bond1.30]
addresses:
– 172.29.240.180/24
parameters:
stp: false
forward-delay: 0
br-vlan:
interfaces: [bond1]
~~~

Son olarak aşağıdaki komutu çalıştırın fakat mtu değerlerinin geçerli olması için “**reboot**” olmalıdır.

~~~
netplan apply
~~~
