---
layout: post
title: Crictl Endpoints Warning
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/25-26-20-uelinuxegitim/25-26-20le05.png
#share-img: /assets/img/path.jpg
tags: [container, docker, containerd, endpoint, crictl, crio]
comments: true
---


This Warning
~~~
WARN[0000] image connect using default endpoints: [unix:///var/run/dockershim.sock unix:///run/containerd/containerd.sock unix:///run/crio/crio.sock]. As the default settings are now deprecated, you should set the endpoint instead. 
ERRO[0002] connect endpoint 'unix:///var/run/dockershim.sock', make sure you are running as root and the endpoint has been started: context deadline exceeded
~~~

My kubernetes container runtime is containerd

sudo vi /etc/crictl.yaml
~~~
runtime-endpoint: unix:///run/containerd/containerd.sock
image-endpoint: unix:///run/containerd/containerd.sock
timeout: 2
debug: true
pull-image-on-create: false
~~~

You can set the debug parameter to false and pull-image-on-create parameter to true

ref:
https://github.com/kubernetes-sigs/cri-tools/blob/master/docs/crictl.md
https://kubernetes.io/docs/tasks/debug/debug-cluster/crictl/
