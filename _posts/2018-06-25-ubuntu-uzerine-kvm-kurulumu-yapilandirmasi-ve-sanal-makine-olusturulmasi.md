---
layout: post
title: Ubuntu üzerine KVM kurulumu, Yapılandırması ve Sanal Makine oluşturulması
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [ubuntu, kvm, linux, virtualization]
comments: true
---
**KVM** Çekirdek tabanlı sanal makine **Linux** çekirdeği için geliştirilen ve onu bir üstsisteme dönüştüren bir sanallaştırma altyapısıdır. Kurulumdan önce sunucumuza **login** olup, **cpu**'muzun destekleyip desteklemediğini anlamak için aşağıdaki komutlar yardımıyla **cpu** bilgilerini kontrol edelim.

Bu komutun çıktısı olarak “**CPU(s):**” değeri **0** ise dekteklemiyor, **1** veya **1** den fazla bir değer ise destekliyor manasına gelir.

~~~
egrep -c ‘(svm|vmx)’ /proc/cpuinfo
~~~

ya da

~~~
lscpu
~~~

Aşağıdaki komutla da çekidek modüllerinin yüklü olup, olmadığını kontrol edebilirsiniz.

~~~
lsmod | grep kvm
~~~

Şimdi kuruluma geçebiliriz. Aşağıdaki komut ile gerekli tüm paketleri kurup, **KVM** ortamımızı hazır hale getirelim.

~~~
sudo apt-get install qemu-kvm libvirt-bin bridge-utils virt-manager
~~~

Yukarıdaki kurulum komutundan sonra sorunsuz bir kurulum gerçekleştiğini, aşağıdaki komutla doğrulayın.

~~~
kvm-ok
~~~

![Crepe](assets/img/ubun16-kvm-ins-a-conf/u16-kvm-iac01.png)

Ardından sunucuyu **reboot** edin.

~~~
brctl show komutuyla da sonradan gelen sanal interface(NIC) i görüntüleyebilirsiniz.
~~~

![Crepe](assets/img/ubun16-kvm-ins-a-conf/u16-kvm-iac02.png)

**Default**’ta **Virtual Machine(qcow2)** lerin saklandığı dizindir.

**/var/lib/libvirt/images/**

**Not1** : Eğer **qcow2** formatında ki diskin yerini bilmiyorsanız aşağıdaki komut(komut ilgili vm'in disklerini bulunduğu dizin ile beraber listeler) size nerde olduğunu söyleyecektir.

~~~
virsh domblklist Ubuntu18
~~~

![Crepe](assets/img/ubun16-kvm-ins-a-conf/u16-kvm-iac03.png)

**Default**’ta **Virtual Machine images**'lar için, kurulum **iso** larının saklandığı dizindir.

**/var/lib/libvirt/boot/**

**Default**’ta **Virtual Machine** özelliklerinin(**ram,cpu,disk vs**.) **xml** formatında tutulduğu yerdir.

**/etc/libvirt/qemu**

**Not2** : Aşağıdaki komut ise ilgili makinanın **xml** formatında tutulduğu özellikleri(**ram,cpu,disk vs**.) ekrana bastırır.

~~~
virsh dumpxml Ubuntu18
~~~

Aşağıdaki iki komutla da mevcuttaki sanal makinaları görüntüleyebilirsiniz.

~~~
virsh -c qemu:///system list
~~~

ya da

~~~
virsh list --all
~~~

![Crepe](assets/img/ubun16-kvm-ins-a-conf/u16-kvm-iac04.png)

**Virtual Machine** lerinizi **GUI** olarak yönetmek, oluşturma ve silme gibi işlemler için **virt** arayüzünü kullanabilirsiniz. Komut aşağıdaki gibidir.

**NoT3** : Bunu yapabilmek için öncesinde sunucuya ssh bağlantısı yaparken, **-X** parametresini kullanarak bağlanmalısınız(**ssh -X root@192.168.1.100**). Ayrıca **Windows** (**Putty de x11 enable** edilmelidir) için **Xming** uygulaması, **Mac** için ise **Xquartz** uygulaması kurulu ve çalışır olması gerekiyor.

**NoT4** : Bu arada bir bilgi daha vermem gerekir. Eğer **Linux** kullanıcısı iseniz yani **client** makinanız **Linux** ise sunucuya **virt-manager** paketini kurmadan, **client** makinanızda **virt-manager** var ise ordan **File-Add Connection** diyerek sunuya bağlanıp yönetebilirsiniz.

~~~
sudo virt-manager
~~~

![Crepe](assets/img/ubun16-kvm-ins-a-conf/u16-kvm-iac05.png)

Yada sunucuya ait konsolu direk olarak aşağıdaki komutla alabilirsiniz.

**NoT5** : Bunu yapabilmek için öncesinde kullanıcı makinasına **virt-viewer** paketini kurmalısınız(**ubuntu/debian** için ’**apt install virt-viewer**’, **Redhat/CentOS** için ‘**yum install virt-viewer**’)

~~~
sudo virt-viewer -c qemu:///system Ubuntu18
~~~

![Crepe](assets/img/ubun16-kvm-ins-a-conf/u16-kvm-iac06.png)

Komutlar aracılığı ile sunucuyu yönetimi için aşağıda bir kaç komut paylaşacağım.

Sunucuyu başlatır.

~~~
virsh start Ubuntu18
~~~

Sunucuyu kapatır.

~~~
virsh shutdown Ubuntu18
~~~

Sunucuyu yeniden başlatır.

~~~
virsh reboot Ubuntu18
~~~

Sunucunun özelliklerini değiştirmek için kullanılır(xml dosyasını değiştirir)

~~~
virsh edit Ubuntu18
~~~

Sununun barındığı host açıldığında, bu vm de otomatik olarak başlatma seçeneğidir.

~~~
virsh autostart Ubuntu18
~~~

Sununun barındığı host açıldığında, bu vm de otomatik olarak başlatma seçeneğini devre dışı bırakır.

~~~
virsh autostart --disable Ubuntu18
~~~

Şimdi “**virt-install**” komtuna bakalım, bu komut aracılığı ile komut satırı üzerinden makine oluşturabilirsiniz. Alltaki komutta örnek komut ile bu işlemi sağlayabilirsiniz. Ben bir çok parametreyi kullandım ama siz tüm parametleri kullanmak zorunda değilsiniz. Zaten kullanmadığınız parametreler yerine default değerler atanacaktır. Tabi sonrasında siz bu değerleri değiştirebilirsiniz. Komutu yürüttükten sonra gui açılıp size kuruluma yönlendirecektir.

~~~
sudo virt-install --virt-type=kvm --name ubuntu-cli --ram 2048 --vcpus=2 --os-variant=Ubuntu16.04 --cdrom=/var/lib/libvirt/boot/ubuntu-18.04-live-server-amd64.iso --network=bridge=eth0,model=virtio --graphics spice --disk path=/var/lib/libvirt/images/ubuntu-cli.qcow2,size=40,bus=virtio,format=qcow2
~~~

![Crepe](assets/img/ubun16-kvm-ins-a-conf/u16-kvm-iac07.png)

Şimdi de “**virt-clone**” komutundan bahsedelim. Adında da anlaşılacağı üzere **clone** almak için kullanılan komuttur. Kullanımı aşağıdaki gibidir. Fakat **vm** kapalı durumda iken clone alabilirsiniz, yoksa uyarı verecektir. “**Clone ‘Ubuntu18.clone’ created successfully.**” uyarısını almalısınız.

~~~
virt-clone --original=Ubuntu18 --name=Ubuntu18.clone --file=/var/lib/libvirt/images/Ubuntu18.clone
~~~

Aşağıdaki komut ise **vmdk** formatındaki bir vm'i **qcow2** formatına çevirip **KVM host** unuzda çalıştırmanızı sağlar.

~~~
qemu-img convert -O qcow2 ubuntu.vmdk ubuntu.qcow2
~~~

Bir sonraki makalemizde **KVM** ortamında nasıl yedek alıp sonra aldığımız o yedeği nasıl geri yükleyip, çalışır hale getireceğiz ona bakacağız.