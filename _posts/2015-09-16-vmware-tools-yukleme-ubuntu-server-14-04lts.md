---
layout: post
title: Vmware Tools yükleme – Ubuntu Server 14.04 LTS
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [vmware, ubuntu]
comments: true
---
![Crepe](/assets/img/ubuntu14-vmtools/ubvmt01.png)

İlk önce bilgisayara CD takar gibi Vmware Tools’u sisteme **Install/Upgrade** tıklayarak entegre ediyoruz.

![Crepe](/assets/img/ubuntu14-vmtools/ubvmt02.png)

Karşımıza gelen uyarıya **OK** dedikten sonra,

![Crepe](/assets/img/ubuntu14-vmtools/ubvmt03.png)

Görüntünün aşağıdaki gibi olması gerek.

![Crepe](/assets/img/ubuntu14-vmtools/ubvmt04.png)

Herşeyden önce şunu belirtmek gerekir. Ubuntu’yu yeni kurduysanız root kullanıcısı pasif durumda gelecektir. Kurulum esnasında oluşturduğunuz user ise root haklarına sahip olmadığı için sisteme bu user ile login olduktan sonra bazı komutları kullanabilmek için komutların başına sudo(geçici root yetkisi verir) eklemeniz gerekmektedir. Eğer root kullanıcısı ile login olursanız komutların başındaki sudo ekini yazmadan komutları çalıştırmalısınız.
Daha sonra aşağıdaki komut ile sisteme mount edelim.

~~~
sudo mount /dev/cdrom /mnt/
~~~

Bu komutu çalıştırdıktan sonra ubuntu kullanıcısı için tekrar şifre girmenizi isteyecektir, girdikten sonra çıktı aşağıdaki gibi olmalıdır.

![Crepe](/assets/img/ubuntu14-vmtools/ubvmt05.png)

Daha sonra mnt klasörüne girmek için aşağıdaki komutu çalıştırın.

~~~
cd /mnt
~~~

Daha sonra mnt klasörünün içindeki **Vmware tools** dosyasını tmp klasörüne aşağıdaki komut ile kopyalayın.

~~~
sudo cp VMwareTools-9.4.10-2092844.tar.gz /tmp/
~~~

Daha sonra tmp klasörünün içine girin.

~~~
cd /tmp/
~~~

Ardından sıkıştırılmış Vmware dosyasını tmp klasörünün içine aşağıdaki komut ile açın.

~~~
tar -zxvf VMwareTools-9.4.10-2092844.tar.gz
~~~

Aşağıdaki komut ile setup klasörünün içine girelim.

~~~
cd vmware-tools-distrib
~~~

Şimdi aşağıdaki komut ile kurulum dosyamızı çalıştıralım.

~~~
sudo ./vmware-install.pl
~~~

Daha sonrasında gelen uyarılara göre enter, yes ve no diyerek başarılı bir kurulum gerçekleştirebilirsiniz.

![Crepe](/assets/img/ubuntu14-vmtools/ubvmt06.png)

Daha sonrasında sisteminiz **reboot** edin.
Sistem yeniden başladıktan sonra aşağıdaki görüntüde Vmware Tools’un başarılı bir şekilde yüklendiğini görebilirsiniz.

![Crepe](/assets/img/ubuntu14-vmtools/ubvmt07.png)

