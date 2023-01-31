---
layout: post
title: Kubernetes Multiple Kubeconfigs Configurations
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [linux, bash, kubernetes, docker, container, kubeconfig, kubectl, multiple, k8s]
comments: true
---

Let's start:)
~~~
mkdir -p $HOME/.kube
~~~

Copy **Cluster kube config files** in **client machine**(for example cluster1, cluster2)
~~~
export KUBECONFIG=$HOME/.kube/cluster1:$HOME/.kube/cluster2
~~~

**Show bundle config files**
~~~
kubectl config view
~~~

**All file in one file**(optional and best practice)
~~~
kubectl config view --flatten > .kube/config
~~~

**Set permission**
~~~
sudo chown $(id -u):$(id -g) $HOME/.kube/config
~~~

~~~
export KUBECONFIG=$HOME/.kube/config
~~~

**Show active cluster**
~~~
kubectl config get-contexts
~~~
~~~
┌[fatihaslan@lnx] [/dev/pts/1]
└[~]>  kubectl config get-contexts
CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
          kubernetes-admin@kubernetes   kubernetes   kubernetes-admin
*         minikube                      minikube     minikube
~~~

**Change cluster envr**
~~~
kubectl config use-context kubernetes-admin@kubernetes
~~~

**Show change**
~~~
kubectl config get-contexts
~~~

**Test**
~~~
kubectl get nodes
~~~

<br>
ref: [mastering-kubeconfig](https://ahmet.im/blog/mastering-kubeconfig/)
