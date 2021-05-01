---
layout: post
title: OpenStack Ansible Network Configuration, Bonding, Mtu, Vlan, Br-vlan, Br-vxlan - Ubuntu 16.04
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [openstack, cloud, linux, network, vlan, ubuntu]
comments: true
---
![Crepe](/assets/img/u16-opnstcnet-conf/opnstck-net-c01.png)

İlk önce aşağıdaki komutla **apt repo**sunu güncelleyin.

~~~
apt update
~~~

Ardından “**bridge-utils**” paketini sisteme kurun.

~~~
apt install bridge-utils
~~~

Daha sonra “**/etc/network/interfaces**” dosyasını aşağıdaki gibi yapılandırabilirsiniz. Tabi siz kullanacağınız şekilde yapılandırabilirsiniz. Ben aşağıdaki örnekte hem **bonding**, hem b, hem **bridge** **vlan**, hem **bridge** **vxlan**, hem de **mtu size** olarak **jumbo frame **kullandım. Ayrıca **nfs** ve **iscsi** yide ayrı **vlan**'larla yapılandırdım. Siz ortamınızda gerekli olan yapılandırmaya göre şekillendirebilirsiniz.

~~~
auto enp6s0f0
iface enp6s0f0 inet manual
    bond-master bond0
    mtu 9000

auto enp7s0f0
iface enp7s0f0 inet manual
    bond-master bond0
    mtu 9000

auto enp6s0f1
iface enp6s0f1 inet manual
    bond-master bond1

auto enp7s0f1
iface enp7s0f1 inet manual
    bond-master bond1

auto bond0
iface bond0 inet static
    address 10.10.10.160
    netmask 255.255.255.0
    gateway 10.10.10.1
    dns-nameservers 10.10.10.1
    mtu 9000
    bond-mode 4
    bond-miimon 100
    bond-slaves none
    bond-downdelay 200
    bond-updelay 200
    bond-xmit_hash_policy 1

auto bond1
iface bond1 inet manual
    bond-mode 4
    bond-miimon 100
    bond-slaves none
    bond-downdelay 200
    bond-updelay 200
    bond-xmit_hash_policy 1

#Container/Host management VLAN interface
auto bond0.10
iface bond0.10 inet manual
    mtu 9000
    vlan-raw-device bond0

#Openstack iscsi Storage network VLAN interface (optional)
auto bond0.20
iface bond0.20 inet manual
    mtu 9000
    vlan-raw-device bond0

#Openstack nfs Storage network VLAN interface (optional)
auto bond0.21
iface bond0.21 inet manual
    mtu 9000
    vlan-raw-device bond0

#OpenStack Networking VXLAN (tunnel/overlay) VLAN interface
auto bond1.30
iface bond1.30 inet manual
    vlan-raw-device bond1

#Container/Host management bridge
auto br-mgmt
iface br-mgmt inet static
    bridge_stp off
    bridge_waitport 0
    bridge_fd 0
    bridge_ports bond0.10
    mtu 9000
    address 192.168.236.160
    netmask 255.255.252.0

#OpenStack Networking VXLAN (tunnel/overlay) bridge
#
#Only the COMPUTE and NETWORK nodes must have an IP address
#on this bridge. When used by infrastructure nodes, the
#IP addresses are assigned to containers which use this
#bridge.
#
auto br-vxlan
iface br-vxlan inet static
    bridge_stp off
    bridge_waitport 0
    bridge_fd 0
    bridge_ports bond1.30
    address 192.168.240.160
    netmask 255.255.252.0

#OpenStack Networking VLAN bridge
auto br-vlan
iface br-vlan inet manual
    bridge_stp off
    bridge_waitport 0
    bridge_fd 0
    bridge_ports bond1

#compute1 Network VLAN bridge
#auto br-vlan
#iface br-vlan inet manual
#bridge_stp off
#bridge_waitport 0
#bridge_fd 0
#
#For tenant vlan support, create a veth pair to be used when the neutron
#agent is not containerized on the compute hosts. ‘eth12’ is the value used on
#the host_bind_override parameter of the br-vlan network section of the
#openstack_user_con g example le. The veth peer name must match the value
#speci ed on the host_bind_override parameter.
#
#When the neutron agent is containerized it will use the container_interface
#value of the br-vlan network, which is also the same ‘eth12’ value.
#
#Create veth pair, do not abort if already exists
#
#pre-up ip link add br-vlan-veth type veth peer name eth12 || true
#Set both ends UP
#pre-up ip link set br-vlan-veth up
#pre-up ip link set eth12 up
#Delete veth pair on DOWN
#post-down ip link del br-vlan-veth || true
#bridge_ports bond1 br-vlan-veth
#Storage bridge (optional)
#
#Only the COMPUTE and STORAGE nodes must have an IP address
#on this bridge. When used by infrastructure nodes, the
#IP addresses are assigned to containers which use this
#bridge.
#

#Storage ISCSI bridge
auto br-iscsi
iface br-iscsi inet static
    bridge_stp off
    bridge_waitport 0
    bridge_fd 0
    mtu 9000
    bridge_ports bond0.20
    address 192.168.244.160
    netmask 255.255.252.0

#Storage NFS bridge
auto br-nfs
iface br-nfs inet static
    bridge_stp o
    bridge_waitport 0
    bridge_fd 0
    mtu 9000
    bridge_ports bond0.21
    address 192.168.248.160
    netmask 255.255.252.0
~~~


Kaynak : [https://docs.openstack.org/project-deploy-guide/openstack-ansible/newton/app-config-prod.html](https://docs.openstack.org/project-deploy-guide/openstack-ansible/newton/app-config-prod.html)