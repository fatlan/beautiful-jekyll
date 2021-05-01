---
layout: post
title: Zen Load Balancer Open VM Tools Kurulumu - Bolum 2
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [zen, loadbalancer, proxy, linux, network]
comments: true
---
![Crepe](assets/img/zen-opvm-bolum2/zen-opvm-01.png)

Sisteme yetkili kullanıcı ile **login** olduktan sonra;

Herşeyden evvel sisteme aşağıdaki komut ile update çekin.

~~~
apt-get update
~~~

Ardından **Sources.list**(**/etc/apt/sources.list**) dosyasına aşağıdaki kaynakların ekleyin.

~~~
deb http://ftp.uk.debian.org/debian/ squeeze main
deb http://ftp.uk.debian.org/debian/ squeeze main contrib
~~~

Daha sonra aşağıdaki komut ile **Open Vm Tools** kurulumunu yapın.

~~~
apt-get install open-vm-tools
~~~

Daha sonra sistemi **reboot** edin.

~~~
reboot
~~~

**Zen Load Balancer İnterface**(**E1000**’den **VMXNET**’ geçiş) **Adapter** değişimi

**VM tools** kurulumundan sonra makinayı kapatıp **E1000** **interface adapter**’ı silin ve **New** diyerek **VMXNET** olarak yeniden **network interface**’yi ekleyin, ardından **OK** deyip çıkın, değişiklikler algılandıktan sonra makinayı çalıştırın ve kaldığınız yerden devam edebilirsiniz.