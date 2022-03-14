---
layout: post
title: Harbor Registry Kurulumu on Ubuntu20LTS
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/25-26-20-uelinuxegitim/25-26-20le05.png
#share-img: /assets/img/path.jpg
tags: [harbor, linux, helm, container, docker, kubernetes, ubuntu, registry]
comments: true
---
**Harbor**: Open source cloud native registry projesidir.

# ![](/assets/img/hrbr-ins/harbor-ins.png)

Biz direk **ubuntu** üzerinde **docker container**'ler olarak kurulumu başlatacağız.

**Docker** kurulumuna başlamadan **bridge0**'yu özelleştirebilirsiniz.
~~~
sudo mkdir /etc/docker

sudo vim /etc/docker/daemon.json


{
"bip": "192.168.81.1/24"
}
~~~

**Docker** kuralım,
~~~
sudo apt install ca-certificates curl gnupg lsb-release -y

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update

udo apt install docker-ce docker-ce-cli containerd.io -y
~~~

**Docker Compose** kuralım,
~~~
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose

sudo usermod -aG docker $USER
~~~

Kurulum yapacağımız **harbor** versiyonunu indirelim,
~~~
wget https://github.com/goharbor/harbor/releases/download/v2.4.1/harbor-offline-installer-v2.4.1.tgz

tar xzf harbor-offline-installer-v2.4.1.tgz

cd harbor/

cp harbor.yml.tmpl harbor.yml
~~~

**Harbor** kurulum öncesi kopyaladığmız "**harbor.yml**" dosyası üzerinde bir takım değişiklikler yapmanız gerekli, bu değişiklikleri ortamınıza uygun şekilde yapmanız gerekir,
~~~
vim harbor.yml

hostname: harbor.fatlan.com
http:
  port: 80
https:
  port: 443
  certificate: /home/fatlan/ssl/fatlancom.crt
  private_key: /home/fatlan/ssl/fatlancom.key
harbor_admin_password: Harbor12345
database:
  password: root123
data_volume: /data
~~~

Son olarak **harbor** kurulum **script**'imizi başlatalım.
~~~
./install.sh
~~~

Akabinde başarılı olan kurulumumuz sonrası **ui** aracılığıyla da **login** olduğumuz **registry**'imizi **cli** ile test edelim.

**Docker image** testi,
~~~
docker tag weaveworks/scope harbor.fatlan.com/library/weaveworks-scope

docker login https://harbor.fatlan.com

docker push harbor.fatlan.com/library/weaveworks-scope

docker pull harbor.fatlan.com/library/weaveworks-scope
~~~


**Helm Chart** testi,
~~~
helm create fatihcharts (after changed)

touch index.yaml

helm repo index . --url https://harbor.fatlan.com/mycharts

helm package fatihcharts {yada cd fatihcharts, helm package .}

helm registry login https://harbor.fatlan.com {export HELM_EXPERIMENTAL_OCI=1 gerekebilir}

helm push fatihcharts-0.1.0.tgz oci://harbor.fatlan.com/mycharts/

helm pull oci://harbor.fatlan.com/mycharts/fatihcharts
~~~

