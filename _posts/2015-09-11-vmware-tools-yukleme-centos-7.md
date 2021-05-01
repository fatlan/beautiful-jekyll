---
layout: post
title: Vmware Tools yükleme - CentOS 7
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [centos, linux, vmware]
comments: true
---
![Crepe](/assets/img/cent7-vmtools/vmt01.png)

Vmware Tools sanal sunucularımızın daha performanslı ve gerçek makinalar ile daha optimizasyonlu çalışması için yüklenmesi gereken yazılımdır. yukarıdaki görüntüden anlaşılacağı üzere sanal makinamız üzerinde Vmware tools yüklenmemiş ve çalışmamaktadır.
İlk önce bilgisayara **CD** takar gibi **Vmware Tools**’u sisteme **Install/Upgrade** tıklayarak entegre ediyoruz.

![Crepe](/assets/img/cent7-vmtools/vmt02.png)

Karşımıza gelen uyarıya **OK** dedikten sonra,

![Crepe](/assets/img/cent7-vmtools/vmt03.png)

Görüntünün aşağıdaki olması gerek.

![Crepe](/assets/img/cent7-vmtools/vmt04.png)

Şimdi sisteme girip kurulumu başlatabiliriz. Ama öncesinde Minimal yüklemelerde **Development Tools** seçeneğini aktif etmediyseniz Vmware tool’u yüklerken perl ile ilgili hata alacaksınız. Hatayı almamak için aşağıdaki komutu çalıştırın.

~~~
yum install perl -y
~~~

İlk önce aşağıdaki komut ile sisteme mount edelim.

~~~
mount /dev/cdrom /mnt/
~~~

Çıktı aşağıdaki gibi olmalıdır.

![Crepe](/assets/img/cent7-vmtools/vmt05.png)

Daha sonra mnt klasörüne girmek için aşağıdaki komutu çalıştırın.

~~~
cd /mnt
~~~

Çıktı aşağıdaki gibi olmalıdır.

![Crepe](/assets/img/cent7-vmtools/vmt06.png)

Daha sonra mnt klasörünün içindeki Vmware tools dosyasını tmp klasörüne aşağıdaki komut ile kopyalayın.

~~~
cp VMwareTools-9.4.10-2092844.tar.gz /tmp/
~~~

![Crepe](/assets/img/cent7-vmtools/vmt07.png)

Daha sonra tmp klasörünün içine girin.

![Crepe](/assets/img/cent7-vmtools/vmt08.png)

Ardından sıkıştırılmış Vmware dosyasını tmp klasörünün içine aşağıdaki komut ile açın.

~~~
tar -zxvf VMwareTools-9.4.10-2092844.tar.gz
~~~

Dosyalar hızlı bir şekilde aşağıya doğru listelenecektir. Aşağıdaki komut ile setup klasörünün içine girelim.

~~~
cd vmware-tools-distrib
~~~

![Crepe](/assets/img/cent7-vmtools/vmt09.png)

Şimdi aşağıdaki komut ile kurulum dosyamızı çalıştıralım.

~~~
./vmware-install.pl
~~~

![Crepe](/assets/img/cent7-vmtools/vmt10.png)

Daha sonrasında gelen uyarılara **enter** ve **yes** diyerek başarılı bir kurulum gerçekleştirebilirsiniz.

![Crepe](/assets/img/cent7-vmtools/vmt11.png)

Daha sonrasında sisteminiz **reboot** edin.

Ve aşağıdaki görüntüde Vmware Tools’un başarılı bir şekilde yüklendiğini görebilirsiniz.

![Crepe](/assets/img/cent7-vmtools/vmt12.png)


