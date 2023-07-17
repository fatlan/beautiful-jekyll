---
layout: post
title: OpenVPN Server Install on Ubuntu 22 LTS
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [linux, openvpn, ubuntu, ca, vpn, ovpn, vpnclient, openvpnserver, openvpnclient]
comments: true
---

OpenVPN Server Install and Configuration

~~~
sudo apt update && sudo apt upgrade -y

sudo apt -y install openvpn easy-rsa iptables
~~~

Create CA and Certificates.
~~~
cd /usr/share/easy-rsa

sudo ./easyrsa init-pki

sudo ./easyrsa build-ca #set_any_name, me Server-CA

sudo ./easyrsa build-server-full server nopass

sudo ./easyrsa build-client-full client nopass

sudo ./easyrsa gen-dh

sudo openvpn --genkey secret ./pki/ta.key

sudo cp -pR /usr/share/easy-rsa/pki/{issued,private,ca.crt,dh.pem,ta.key} /etc/openvpn/server/
~~~

Configure OpenVPN Server.
~~~
sudo cp /usr/share/doc/openvpn/examples/sample-config-files/server.conf /etc/openvpn/server/

sudo vi /etc/openvpn/server/server.conf
~~~
~~~
# line 32
port 1194

# line 36
proto udp

# line 53
dev tun

# line 78
ca ca.crt
cert issued/server.crt
key private/server.key

# line 85
dh dh.pem

# line 101
# I set it like this
server 10.9.0.0 255.255.255.0

# line 109
ifconfig-pool-persist /var/log/openvpn/ipp.txt

# line 142
# My pushed(route table) subnet, you change
push "route 172.16.100.0 255.255.255.0"

# line 224
duplicate-cn

# line 231
keepalive 10 120

# line 244
tls-auth ta.key 0

# line 281
persist-key
persist-tun

# line 289
status /var/log/openvpn/openvpn-status.log

# line 298
log /var/log/openvpn/openvpn.log

# line 306
verb 3
~~~

~~~
sudo vi /etc/openvpn/server/add-bridge.sh
~~~
~~~
#!/bin/bash
# network interface which can connect to local network
IF=enp1s0
# interface VPN tunnel uses
# for the case of this example like specifying [tun] on the config, generally this param is [tun0]
VPNIF=tun0

echo 1 > /proc/sys/net/ipv4/ip_forward
iptables -A FORWARD -i ${VPNIF} -j ACCEPT
iptables -t nat -A POSTROUTING -o ${IF} -j MASQUERADE

#-------------ALTERNATIVE--------------
#/sbin/sysctl -n net.ipv4.conf.all.forwarding > /var/log/openvpn/net.ipv4.conf.all.forwarding.bak
#/sbin/sysctl net.ipv4.conf.all.forwarding=1
#/sbin/iptables-save > /var/log/openvpn/iptables.save
#/sbin/iptables -t nat -F
#/sbin/iptables -t nat -A POSTROUTING -s 10.9.0.0/24 -j MASQUERADE
~~~

~~~
sudo vi /etc/openvpn/server/remove-bridge.sh
~~~
~~~
#!/bin/bash
IF=enp1s0
VPNIF=tun0

echo 0 > /proc/sys/net/ipv4/ip_forward
iptables -D FORWARD -i  ${VPNIF} -j ACCEPT
iptables -t nat -D POSTROUTING -o ${IF} -j MASQUERADE

#-------------ALTERNATIVE--------------
#FORWARDING=$(cat /var/log/openvpn/net.ipv4.conf.all.forwarding.bak)
#echo "restoring net.ipv4.conf.all.forwarding=$FORWARDING"
#/sbin/sysctl net.ipv4.conf.all.forwarding=$FORWARDING
#/etc/openvpn/fw.stop
#echo "Restoring iptables"
#/sbin/iptables-restore < /var/log/openvpn/iptables.save
~~~

~~~
sudo chmod 700 /etc/openvpn/server/{add-bridge.sh,remove-bridge.sh}
~~~
~~~
sudo vi /lib/systemd/system/openvpn-server@.service
~~~
~~~
# add into [Service] section
[Service]
.....
.....
ExecStartPost=/bin/sh -c "/etc/openvpn/server/add-bridge.sh"
ExecStopPost=/bin/sh -c "/etc/openvpn/server/remove-bridge.sh"
~~~

~~~
sudo systemctl daemon-reload

sudo systemctl enable --now openvpn-server@server
~~~


Transfer certs follows you generated to Client Host you'd like to connect with VPN
~~~
・/etc/openvpn/server/ca.crt
・/etc/openvpn/server/ta.key
・/etc/openvpn/server/issued/client.crt
・/etc/openvpn/server/private/client.key
~~~

---
**OpenVPN Client Connection Config**

- ca.crt
- ta.key
- client1.crt
- client1.key
- client.ovpn

**client.ovpn** file
~~~
client
dev tun
proto udp
remote REMOTE_IP 1194
resolv-retry infinite
nobind
persist-key
persist-tun
ca ca.crt
cert client1.crt
key client1.key
tls-auth ta.key 1
verb 3
~~~


ref: https://www.server-world.info/en/note?os=Ubuntu_22.04&p=openvpn&f=1

ref: https://www.server-world.info/en/note?os=Ubuntu_22.04&p=openvpn&f=2







