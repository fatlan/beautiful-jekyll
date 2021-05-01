---
layout: post
title: Linux Sistemlerde Açık Portların Kontrolü - netstat - lsof
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [lsof, linux, netstat, port, network]
comments: true
---
İşletim sisteminde hizmet veren her servis bir **port** üzerinden hizmet verir. Bu nedenle sistem yöneticisinin sistem üzerinde bu portlara hakim olması gerekiyor. Farkında olunmadığı ya da gizli kalmış açık bir **port** bulunmamalıdır. Çünkü bu portlar aracılığı ile dış dünyaya yani internete servis sunulmaktadır. Bu nedenle internete açık bağlantıların belirlenmesi kritik bir görevdir ve öneme sahiptir. İşletim sisteminizde **tcp** ve **udp** olarak açık olan port ve servisler “**netstat**” ve “**lsof**” yardımıyla tespit edilebilir.

~~~
netstat -plntua
~~~

![Crepe](/assets/img/lsof-net-ps/ps-lin-nl01.png)

~~~
lsof -i -n | egrep -i listen
~~~

![Crepe](/assets/img/lsof-net-ps/ps-lin-nl02.png)

