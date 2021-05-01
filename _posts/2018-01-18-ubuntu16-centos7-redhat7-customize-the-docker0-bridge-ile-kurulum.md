---
layout: post
title: Ubuntu16/CentOS7/RedHat7 Customize The docker0 Bridge ile Kurulum
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [centos, redhat, docker, bridge, ubuntu, linux]
comments: true
---
Şimdiki makalemizde özelleştirilmiş network ile Docker kurulumunu ele alacağız. Öncesinde neden böyle birşeye ihtiyaç duyabileceğimizden bahsedeyim, hemde başımdan geçen durumu da anlatmış olurum.

Bildiğiniz gibi **Docker** kurulumda **docker0 bridge**‘inin **default ip range**‘i olarak **172.17.0.1/16**‘yı vererek kurulum gerçekleştirmektedir. Bu durumda sizin bağlantı sağladığınız yerel ağ tercihini hali hazırda 172'li **subnet** ise kurulum aşamasında Docker **Run** olduğunda bağlantınız koparacak ve bir daha uzaktan bağlantı sağlayamayacaksınız. Benim başımada böyle bir durum geldi, yerel ağım 172 li /24 subnet’indeydi ve /16 lı **subnet** benim **subnet**’i yuttuğundan, çakışma gerçekleşti ve bağlantımı kopardı. Bu durumda konsol aracılığı ile sizin kendi belirlediğiniz **subnet** ile kurulum yapmanız gerekli ya da **subnet**’i editlemelisiniz. Ben aşağıdaki göstereceğim yöntemle “**systemd**” kullanarak istediğim ortamı ayağa kaldırdım. Şimdi sırasıyla **Ubuntu16**’da ve **Centos7/RedHat7** de bunu nasıl yapabiliriz, buna değinelim.

***Ubuntu16 da customize subnet ile Docker kurulumu**

İlk önce “**etc/systemd/system/**” altına “**docker.service.d**” dizinini, “**mkdir**” komutunu kullanarak oluşturalım.

~~~
sudo mkdir /etc/systemd/system/docker.service.d
~~~

İkinci olarak “**/etc/systemd/system/docker.service.d/**” altına “**docker.conf**” dosyasını “**vi**” komutunu kullanarak oluşturalım ve içine aşağıdaki değerleri yazalım.

~~~
sudo vi /etc/systemd/system/docker.service.d/docker.conf

[Service]
ExecStart=
ExecStart=/usr/bin/dockerd -H fd:// --bip=10.10.100.1/24
~~~

![Crepe](/assets/img/u16-c7-r7-docker-cbridge/docker0-c-b01.png)

Gerekli systemd ayarlamalarını yaptık ve şimdi normal akışında kurulumu yaparak işlemleri tamamlayacağız.

Önce, resmi Docker repository için GPG anahtarını sisteme ekleyelim.

~~~
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
~~~

Ardında **Docker repo**’sunu ekleyelim.

~~~
sudo add-apt-repository “deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable”
~~~

Şimdi repolarımızı güncelleyelim.

~~~
sudo apt-get update
~~~

Ve şimdi aşağıdaki komutla **Docker**’ı kuralım.

~~~
sudo apt-get install -y docker-ce
~~~

Ardından gerekli kontrolleri sırasıyla yapıp, işlemlerimizi teyit edelim.

![Crepe](/assets/img/u16-c7-r7-docker-cbridge/docker0-c-b02.png)

![Crepe](/assets/img/u16-c7-r7-docker-cbridge/docker0-c-b03.png)

![Crepe](/assets/img/u16-c7-r7-docker-cbridge/docker0-c-b04.png)

![Crepe](/assets/img/u16-c7-r7-docker-cbridge/docker0-c-b05.png)


***CentOS7/RedHat7 da customize subnet ile Docker kurulumu**

Gene “**systemd**” yi kullanarak işlemleri gerçekleştireceğiz.

İlk önce “**etc/systemd/system/**” altına “**docker.service.d**” dizinini, “**mkdir**” komutunu kullanarak oluşturalım.

~~~
mkdir /etc/systemd/system/docker.service.d
~~~

İkinci olarak “**/etc/systemd/system/docker.service.d/**” altına “**docker.conf**” dizinini “vi” komutunu kullanarak oluşturalım.

~~~
vi /etc/systemd/system/docker.service.d/docker.conf

[Service]
ExecStart=
ExecStart=/usr/bin/dockerd --bip=10.10.100.1/24
~~~

![Crepe](/assets/img/u16-c7-r7-docker-cbridge/docker0-c-b06.png)

Gerekli systemd ayarlamalarını yaptık ve şimdi normal akışında kurulumu yaparak işlemleri tamamlayacağız.

**Docker**’ı kurmadan önce aşağıdaki komutla gerekli paketleri indirip, kuralım.

~~~
yum install yum-utils device-mapper-persistent-data lvm2 -y
~~~

Ardında **Docker repo**’sunu ekleyelim.

~~~
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
~~~

Şimdi aşağıdaki komutla **Docker**’ı kuralım.

~~~
yum install docker-ce -y
~~~

**Docker** kurulduktan sonra sevisi çalıştırmak ve **enable** etmek lazım. Aşağıdaki komut ile yapabilirsiniz.

~~~
systemctl enable docker.service
systemctl start docker.service
systemctl status docker.service
~~~

![Crepe](/assets/img/u16-c7-r7-docker-cbridge/docker0-c-b07.png)

Ardından gerekli kontrolleri sırasıyla yapıp, işlemlerimizi teyit edelim.

![Crepe](/assets/img/u16-c7-r7-docker-cbridge/docker0-c-b08.png)

![Crepe](/assets/img/u16-c7-r7-docker-cbridge/docker0-c-b09.png)

![Crepe](/assets/img/u16-c7-r7-docker-cbridge/docker0-c-b10.png)

![Crepe](/assets/img/u16-c7-r7-docker-cbridge/docker0-c-b11.png)

Bu arada servisi daha önceden kurduysanız “**systemd**” yapılandırmasını yaptıktan sonra aşağıdaki komutları çalıştırdığınızda sizin belirlediğiniz **subnet overwrite** olacak.

~~~
systemctl daemon-reload
systemctl restart docker.service
~~~


**COMMENT**;

**Custom bridge** için yukarıdaki **systemd** yapılandırmasındaki “**fd://**” kısmı “**unix://**” olarak güncellendi.

**Alternatif** yöntem de aşağıdaki gibidir.

Dosyayı oluşturun = **/etc/docker/daemon.json**

~~~
{
"bip": "192.168.34.1/24"
}
~~~