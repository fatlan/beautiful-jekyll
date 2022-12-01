---
layout: post
title: Crictl Command Usage
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/25-26-20-uelinuxegitim/25-26-20le05.png
#share-img: /assets/img/path.jpg
tags: [ubuntu, containerd, container, linux, crictl]
comments: true
---

Existing image list
~~~
sudo crictl images
~~~

Global image pull
~~~
sudo crictl pull busybox:stable
~~~

Private repo image pull
~~~
sudo crictl pull --creds "robot$fatlan:HXjS87lcQu4mef3bBtV03tpwvijN0Zgd" harbor.fatlan.com/microservice/microservicetest:1.0.99
~~~

