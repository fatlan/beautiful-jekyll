---
layout: post
title: ArgoCD Server Install(behind haproxy) and Configuration and Multi Cluster Add
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [linux, argocd, ubuntu, cloud, redhat, centos, cloudnative, kubernetes, argocd-server, container]
comments: true
---

Argocd kurulumu

[https://argo-cd.readthedocs.io/en/stable/getting_started/](https://argo-cd.readthedocs.io/en/stable/getting_started/)

Değiştirdiğim kısımlar aşağıdaki gibidir.
~~~
apiVersion: v1
kind: Service
metadata:
  labels:
    app.kubernetes.io/component: server
    app.kubernetes.io/name: argocd-server
    app.kubernetes.io/part-of: argocd
  name: argocd-server
spec:
  type: NodePort
  ports:
  - name: http
    port: 80
    protocol: TCP
    targetPort: 8080
#  - name: https
#    port: 443
#    protocol: TCP
#    targetPort: 8080
  selector:
    app.kubernetes.io/name: argocd-server
~~~

~~~
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app.kubernetes.io/component: server
    app.kubernetes.io/name: argocd-server
    app.kubernetes.io/part-of: argocd
  name: argocd-server

...
...
      containers:
      - args:
        - /usr/local/bin/argocd-server
        - --insecure    # <-- this parameter needs to be added, added for X-Forwarded-Proto
        env:
~~~


Argocd cli kurulumu

[https://argo-cd.readthedocs.io/en/stable/cli_installation/](https://argo-cd.readthedocs.io/en/stable/cli_installation/)

~~~
argocd login --loglevel debug argocd.fatlan.com --grpc-web

argocd logout --loglevel debug argocd.fatlan.com --grpc-web
~~~
~~~
echo $KUBECONFIG

export KUBECONFIG="${KUBECONFIG}:/home/fatlan/.kube/8nodecluster.txt:/home/fatlan/.kube/3nodecluster.txt"

kubectl config get-contexts -o name
~~~
~~~
vi .kube/8nodecluster.txt

#change below lines

#  name: kubernetes-admin@kubernetes8
#current-context: kubernetes-admin@kubernetes8

vi .kube/3nodecluster.txt

#change below lines

#  name: kubernetes-admin@kubernetes3
#current-context: kubernetes-admin@kubernetes3

kubectl config get-contexts -o name
~~~
~~~
argocd cluster add kubernetes-admin@kubernetes8

argocd cluster add kubernetes-admin@kubernetes3

argocd cluster list
~~~




