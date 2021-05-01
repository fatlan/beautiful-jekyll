---
layout: post
title: KVM - qcow2 Disk Dosyası Boyutunu Küçültme (Reduce - Shrink Qcow2 Disk Files)
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [kvm, virtualization, linux]
comments: true
---
Şimdiki yazımızda **KVM hipervisor** katmanında **guest**’ler(vm - sanal makineler) için başınıza gelebilecek bir durumdan bahsedeceğiz. Malumunuz **KVM** hostlarınızda disk dosyalarınız(**qcow2**) diskte belli bir alanı kaplamaktadır. Fakat zamanla guestteki disk alanı hosttaki barındığı disk alanını doldurup hatta üzerinede çıkabilmektedir. Aslında **VM** diski lesystem tabanında düzenli alanlarla kullanmadığı yada dağınık yazdığı için yani bu durum adreslemeyle alakalı; dolu göstermektedir. Aslında windows’ta defrag yöntemi gibi diyebiliriz. Bu durumda sanal makine otomatik olarak “**Pause**” duruma düşecek ve hizmet veremeyecek duruma gelecektir. Bu duruma düşmeden yada düştükten sonra aşağıdaki yöntemlerle **qcow2** disk dosyalarını boyutunu **hipervisor** katmanında aslında kullanılmadığı halde kullanıyor gibi gösterdiği alanları yeniden düzenleyerek küçültecektir.

İlk yöntem olarak “**virt-sparsify**” komutunu kullanarak etkili ve verimli bir şekilde **disk** dosyanızın boyutunu küçültebilirsiniz.

**NoT1** : “**virt-sparsify**” komutunu kullanabilmek için **libguestfs-tools** paketini öncesinde kurmalısınız.

**NoT2** : İşlemleri vm kapalıyken yapın.

İşlem yapılmadan önce aşağıdaki komutla disk dosyasının boyutunu görelim.

~~~
du -ha
~~~

![Crepe](assets/img/kvm-qcow2-shrink/kvm-qsh01.png)

Ardından “**virt-sparsify**” komutu ile istediğimiz küçültme işlemini gerçekleştirelim.

~~~
virt-sparsify --in-place diskdosyanız.qcow2
~~~

![Crepe](assets/img/kvm-qcow2-shrink/kvm-qsh02.png)

Daha sonra tekrar “**du -ha**” komutunu çalıştıralım ve boyuttaki azalmayı görelim.

![Crepe](assets/img/kvm-qcow2-shrink/kvm-qsh03.png)

**Ikinci yöntem olarak gene “virt-sparsify” komutunu convert yöntemi ile kullanalım.**

~~~
virt-sparsify diskdosyanız.qcow2 --convert qcow2 yeniisimdiskdosyanız.qcow2
~~~

![Crepe](assets/img/kvm-qcow2-shrink/kvm-qsh04.png)

Sonra “**du -ha**” komutu

![Crepe](assets/img/kvm-qcow2-shrink/kvm-qsh05.png)

**Son olarak üçüncü yöntem ise**,

Aşağıdaki yöntem ile **Guest**’in içinde “**dd**” komutu kullanarak kalan boş alana “**zero**” basıyoruz.

~~~
dd if=/dev/zero of=/MyDump
~~~

Daha sonra **MyDump** dosyasını siliyoruz.

~~~
rm -f /MyDump
~~~

Son olarak “**qemu-img**” komutu ile işlemi gerçekleştiriyoruz ama öncesinde fırsatınız varsa disk dosyasının yedeğini almanızda fayda var.

~~~
qemu-img convert -O qcow2 ilgilidosyaismi.qcow2 yeni_ilgilidosyaismi.qcow2
~~~

Ref:

[http://libguestfs.org/virt-sparsify.1.html](http://libguestfs.org/virt-sparsify.1.html) <br>
[https://pve.proxmox.com/wiki/Shrink_Qcow2_Disk_Files](https://pve.proxmox.com/wiki/Shrink_Qcow2_Disk_Files)