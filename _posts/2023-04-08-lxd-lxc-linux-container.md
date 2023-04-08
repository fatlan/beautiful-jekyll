---
layout: post
title: LXD/LXC Linux Containers
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [linux, namespace, kubernetes, docker, container, cgroups, ns, isolation lxc, lxd]
comments: true
---

Gerekli paketleri kuralım,
~~~
sudo apt  install lxd-installer -y
~~~

**Bash** completion'ı ayarlayalım,
~~~
sudo ln -s /snap/lxd/current/etc/bash_completion.d/snap.lxd.lxc /etc/bash_completion.d/lxc
~~~

Eğer kullanıcı **lxd** grubuna dahil değilse ekleyelim,
~~~
sudo usermod -aG lxd $USER
~~~

~~~
sudo lxd init
~~~
~~~
lxc list
lxc image list
lxc network list
lxc storage list
lxc snapshot list
lxc image list images:
lxc image list images: 'ubuntu'
lxc image copy images:<alpine/edge> local: --alias alpine
lxc init <ubuntu:22.04> {stop backround}
lxc launch <ubuntu:22.04> [<container>]
lxc launch images:<alpine/3.17>
lxc start <container>
lxc stop <container> [--force]
lxc restart <container> [--force]
lxc info <container>
lxc info --show-log <container>
lxc pause <container>
lxc delete <container>
lxc exec <container> <command>
lxc copy <src-container> <dst-container>
lxc move <src-container> <dst-container> {should be stop container}
lxc publish <image_name> --alias <new_image_name>
~~~


ref: [https://wiki.slank.dev/misc/lxd.html](https://wiki.slank.dev/misc/lxd.html)