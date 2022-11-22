---
layout: post
title: 3 Farklı Yöntem ile Docker ve Docker Compose(docker-compose) Kurulumu on Ubuntu
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/25-26-20-uelinuxegitim/25-26-20le05.png
#share-img: /assets/img/path.jpg
tags: [ubuntu, docker, container, linux]
comments: true
---

**Docker** kurulumuna başlamadan **bridge0**'yu özelleştirebilirsiniz.
~~~
sudo mkdir /etc/docker

sudo vim /etc/docker/daemon.json


{
"bip": "192.168.81.1/24"
}
~~~


###`Güncel Yöntem`
**Docker** ve **Docker Compose** kuralım,
~~~
sudo apt install ca-certificates curl gnupg lsb-release -y

sudo mkdir -p /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update

sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin
~~~
Güncel durumda "**docker-compose**" yöntemi lazım ise eğer aşağıdaki yönergeyi izleyebilirsiniz.
~~~
sudo usermod -aG docker $USER
alias docker-compose="docker compose"

ya da

alias sudo='sudo '
alias docker-compose="docker compose"
~~~

###`Eski Yöntem`
**Docker** kuralım,
~~~
sudo apt install ca-certificates curl gnupg lsb-release -y

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update

sudo apt install docker-ce docker-ce-cli containerd.io -y
~~~

**Docker Compose** kuralım,
~~~
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose

sudo usermod -aG docker $USER
~~~


###`Son Yöntem - Genellikle Test ve Demolar için Tercih Edilebilir`
~~~
curl -fsSL https://get.docker.com -o get-docker.sh | sh
~~~