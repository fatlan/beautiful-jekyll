---
layout: post
title: NFS Server Installation on Ubuntu
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/25-26-20-uelinuxegitim/25-26-20le05.png
#share-img: /assets/img/path.jpg
tags: [linux, ubuntu, nfs, storage]
comments: true
---


`NFS Server Installation`
~~~
sudo apt install nfs-kernel-server nfs-common

sudo mkfs.ext4 /dev/sdb

sudo mkdir -p /mnt/nfs_kube_sc/

sudo chown -R nobody:nogroup /mnt/nfs_kube_sc/

sudo chmod 777 /mnt/nfs_kube_sc/

sudo mount /dev/sdb /mnt/nfs_kube_sc/
~~~
~~~
sudo vi /etc/exports
~~~
~~~
/mnt/nfs_kube_sc 192.168.25.0/24(rw,sync,no_subtree_check)
~~~
~~~
sudo exportfs

showmount -e localhost
~~~
~~~
sudo vi /etc/fstab
~~~
~~~
/dev/sdb	/mnt/nfs_kube_sc	ext4 defaults 0 1
~~~
~~~
sudo mount -a

sudo systemctl restart nfs-kernel-server.service
~~~


`NFS Client Connection Configuration`
~~~
sudo apt install nfs-common

showmount -e nfs_server_ip

mount nfs_server_ip:/mnt/nfs_kube_sc/ /tmp

mount

df -h
~~~
~~~
vi /etc/fstab
~~~
~~~
nfs_server_ip:/mnt/nfs_kube_sc    /tmp  nfs4    rw,sync 0 0
~~~
