---
layout: post
title: Kubernetes Install With Kubespray on Ubuntu20LTS
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/25-26-20-uelinuxegitim/25-26-20le05.png
#share-img: /assets/img/path.jpg
tags: [kubernetes, kubespray, kubectl, linux, ubuntu]
comments: true
---

# ![](/assets/img/5nodekubspry/5nodeskube.png)

~~~
fatlan@kube-master01:~$ kubectl get nodes -o wide
NAME            STATUS   ROLES                  AGE     VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION     CONTAINER-RUNTIME
kube-master01   Ready    control-plane,master   5h51m   v1.22.2   10.10.10.191  <none>        Ubuntu 20.04.3 LTS   5.4.0-89-generic   docker://20.10.9
kube-master02   Ready    control-plane,master   5h51m   v1.22.2   10.10.10.192  <none>        Ubuntu 20.04.3 LTS   5.4.0-89-generic   docker://20.10.9
kube-master03   Ready    control-plane,master   5h51m   v1.22.2   10.10.10.193  <none>        Ubuntu 20.04.3 LTS   5.4.0-89-generic   docker://20.10.9
kube-worker01   Ready    <none>                 5h50m   v1.22.2   10.10.10.194  <none>        Ubuntu 20.04.3 LTS   5.4.0-89-generic   docker://20.10.9
kube-worker02   Ready    <none>                 5h50m   v1.22.2   10.10.10.195  <none>        Ubuntu 20.04.3 LTS   5.4.0-89-generic   docker://20.10.9
~~~

##### Tüm hostlarda çalıştırılır
##### $USER aktif kullanıcı ile değiştirilir
~~~
sudo echo "$USER ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/$USER
~~~

##### Ansible client çalıştırılmalı(10.10.10.191)
~~~
ssh-keygen
ssh-copy-id $USER@10.10.10.191
ssh-copy-id $USER@10.10.10.192
ssh-copy-id $USER@10.10.10.193
ssh-copy-id $USER@10.10.10.194
ssh-copy-id $USER@10.10.10.195
~~~

~~~
git clone https://github.com/kubernetes-sigs/kubespray.git
~~~

~~~
cd kubespray/

sudo apt install python3-pip

cat requirements.txt

sudo pip3 install -r requirements.txt

cp -rfp inventory/sample inventory/mycluster

declare -a IPS=(10.10.10.191 10.10.10.192 10.10.10.193 10.10.10.194 10.10.10.195)

CONFIG_FILE=inventory/mycluster/hosts.yaml python3 contrib/inventory_builder/inventory.py ${IPS[@]}
~~~

##### Aşağıdaki hostname'leri kendi yapınıza göre değiştirebilirsiniz
~~~
vi kubespray/inventory/mycluster/hosts.yaml
~~~
~~~
all:
  hosts:
    kube-master01:
      ansible_host: 10.10.10.191
      ip: 10.10.10.191
      access_ip: 10.10.10.191
    kube-master02:
      ansible_host: 10.10.10.192
      ip: 10.10.10.192
      access_ip: 10.10.10.192
    kube-master03:
      ansible_host: 10.10.10.193
      ip: 10.10.10.193
      access_ip: 10.10.10.193
    kube-worker01:
      ansible_host: 10.10.10.194
      ip: 10.10.10.194
      access_ip: 10.10.10.194
    kube-worker02:
      ansible_host: 10.10.10.195
      ip: 10.10.10.195
      access_ip: 10.10.10.195
  children:
    kube-master:
      hosts:
        kube-master01:
        kube-master02:
        kube-master03:
    kube-node:
      hosts:
        kube-master01:
        kube-master02:
        kube-master03:
        kube-worker01:
        kube-worker02:
    etcd:
      hosts:
        kube-master01:
        kube-master02:
        kube-master03:
    k8s-cluster:
      children:
        kube-master:
        kube-node:
    calico-rr:
      hosts: {}
~~~

~~~
cat kubespray/inventory/mycluster/group_vars/all/all.yml

cat kubespray/cluster.yml

cat kubespray/inventory/mycluster/group_vars/k8s_cluster/k8s-cluster.yml
~~~

~~~
ansible-playbook -i inventory/mycluster/hosts.yaml --become --become-user=root cluster.yml

# playbook'u sadece tek makinede çalıştırmak için bu parametreyi kullanabilirsiniz, --limit <hostname or ip>
~~~

##### $USER aktif kullanıcı ile değiştirilir
~~~
mkdir -p .kube

sudo cp /etc/kubernetes/admin.conf .kube/

sudo chown $USER:$USER .kube/admin.conf

export KUBECONFIG=$PWD/.kube/admin.conf

kubectl get nodes -o wide
~~~

##### Node'ler reboot olduktan sonra eğer swap alanı kullanılıyorsa, .profile ya da .bashrc düzenleyebilirsiniz
##### ve node'ler reboot olunca cron or rc.local ya da systemd servis script dosyası oluşturabilirsiniz
##### Tüm NODE
~~~
swapoff -a
export LC_ALL=C
~~~
##### Sadece MASTER/S NODE
~~~
swapoff -a
export KUBECONFIG=$PWD/.kube/admin.conf
export LC_ALL=C
~~~


ref: [1](https://github.com/kubernetes-sigs/kubespray)

