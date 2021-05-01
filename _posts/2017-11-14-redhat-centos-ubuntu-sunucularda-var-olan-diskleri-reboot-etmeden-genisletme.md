---
layout: post
title: RedHat/CentOS/Ubuntu Sunucularda Var Olan Diskleri Reboot Etmeden Genişletme
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [redhat, centos, linux, disk]
comments: true
---
Sanal sunucularda var olan diski **hypervisor** katmanında genişlettikten sonra değişikliklerin algılanması için **Linux OS** tarafında genelde **reboot** etmek zorunda kalınır. Şimdi biz burda **reboot** etmeden **hypervisor** tarafında var olan diskteki genişletilen o alanı, **linux operation system** katmanında scsi yolunu kullanarak disk aygıtlarını **rescan** ederek algılatacağız. Bu arada çalışmaları **Vmware Esxi** üzerinde yapacağım.

***İlk örneği RedHat/CentOS üzerinde yapalım;**

Resimde de görüldüğü gibi **1TB** diskim var **/boot**, **swap** ve **/** dizinlerim bu disk üzerinden partition edilmiş durumda. Şimdi bu var olan diski **reboot** etmeden **extend** edelim.

**Hypervisor** görüntüsü;

![Crepe](assets/img/linux-disk-add/los-dis-a01.png)

**OS** görüntüsü;

![Crepe](assets/img/linux-disk-add/los-dis-a02.png)

**Hypervisor** da genişletelim **1500GB** olarak ayarlıyorum.

![Crepe](assets/img/linux-disk-add/los-dis-a03.png)

Şimdi bu değişikliği **fdisk** komutu ile görebilmek için aşağıdaki komutu çalıştırın. **/sys/class/scsi_device/** dan sonra **tab** tuşuna basın **disk device**’leri görün, ona göre komutu çalıştırıp işlem yapın. Benim 10 ile başlayan olduğu için ordan yürüyeceğim.

![Crepe](assets/img/linux-disk-add/los-dis-a04.png)

Komutun en net hali aşağıdaki gibi olacak,

~~~
echo 1 > /sys/class/scsi_device/10\:0\:0\:0/device/rescan
~~~

![Crepe](assets/img/linux-disk-add/los-dis-a05.png)

Şimdi **fdisk -l** ile yaptığımız değişikliği görelim. Bu arada benim tek diskim olduğu için **fdisk**’i **-l** parametresi ile kullandım, sizin birden fazla diskiniz var ise **fdisk /dev/sd{a,b,c vs.}** gibi kullanabilirsiniz.

![Crepe](assets/img/linux-disk-add/los-dis-a06.png)

**Burdan sonra disk’i var olan partition’larda kullanabilir hale getirmek için başka işlemler yapmalısınız. Burda fdisk, ext4, xfs ve lvm gibi konular devreye giriyor. Bir başka makalede bu konulara değinmeye çalışacağız.**

***Şimdi ikinci örneği Ubuntu üzerinde yapalım;** senaryo aynı,

**Hypervisor** katmanında var olan diskim **50GB**,

![Crepe](assets/img/linux-disk-add/los-dis-a07.png)

fdisk görünümü,

![Crepe](assets/img/linux-disk-add/los-dis-a08.png)

**100GB**'ye **extend** ediyorum,

![Crepe](assets/img/linux-disk-add/los-dis-a09.png)

Aşağıdaki komutu çalıştırmadan önce **tab** tuşu ile **device**’leri görün, sonra komutun tamamını çalıştırın.

![Crepe](assets/img/linux-disk-add/los-dis-a10.png)

Fakat komutu çalıştırdıktan sonra aşağıdaki gibi **permission denied** uyarısını alacaksınız, bu komutu sadece **root** çalıştırabilir. **O** yüzden **su -** ile **root** kullanıcısına geçip komutu öyle çalıştırın.

![Crepe](assets/img/linux-disk-add/los-dis-a11.png)

~~~
echo 1 > /sys/class/scsi_device/32\:0\:0\:0/device/rescan
~~~

Daha sonra **fdisk -l** ile diskin genişlediğini görüntüleyin.

![Crepe](assets/img/linux-disk-add/los-dis-a12.png)

**Gene burdan sonra disk’i var olan partition’larda kullanabilir hale getirmek için başka işlemler yapmalısınız. Burda fdisk, ext4, xfs ve lvm gibi konular devreye giriyor. Bir başka makalede bu konulara değinmeye çalışacağız.**