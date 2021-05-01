---
layout: post
title: Linux KVM Backup and Restore
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [linux, kvm, virtualization, backup, restore]
comments: true
---

Aşağıdaki yönergeleri izleyerek KVM sanallarınızı yedekleyebilir ve yedekten geri dönebilirsiniz.

**1.Backup**

İlk önce makinelerimizi listeleyelim ve çalışır vaziyette olduğunu görelim.

~~~
virsh list --all
~~~

![Crepe](/assets/img/kvm-bac-rest/kvm-br01.png)

Daha sonra yedekleyeceğimiz vm’i kapatalım.

~~~
virsh shutdown Ubuntu18
~~~

Ardından makineleri tekrar listeleyelim ve kapalı olduğunu görelim.

~~~
virsh list --all
~~~

![Crepe](/assets/img/kvm-bac-rest/kvm-br02.png)

Şimdi makineyi (XML dosyasını) aşağıdaki komut/yöntem ile yedekleyelim.

~~~
virsh dumpxml Ubuntu18 > /MyBackup/Ubuntu18.xml
~~~

ya da

default ta **XML** lerin tutulduğu “**/etc/libvirt/qemu**” dizinin altından ilgili **XLM**’i ilgili backup dizinine **cp** komutu ile kopyalayabilirsiniz.

![Crepe](/assets/img/kvm-bac-rest/kvm-br03.png)

Şimdi de disk dosyasını (**qcow2**) aşağıdaki komut/yöntem ile yedekleyelim.

**qcow2** formatındaki disk dosyasını da **/MyBackup** altına kopyalayalım. **Disk** dosyalarının **default**'ta tutulduğu yer “**/var/lib/libvirt/images/**” altındadır.

ya da

~~~
virsh domblklist Ubuntu18
~~~
komutu ile nerde olduğunu görebilirsiniz. İlgili yerden cp yada **scp** (remote) komutu ile backup klasörünüze kopyalayabilirsiniz.

![Crepe](/assets/img/kvm-bac-rest/kvm-br04.png)

~~~
cp /var/lib/libvirt/images/Ubuntu18.qcow2 /MyBackup/Ubuntu18.qcow2
~~~

Listeleyip tüm yedeklerimizi görelim.

~~~
ls -lash
~~~

![Crepe](/assets/img/kvm-bac-rest/kvm-br05.png)

**NoT**: Backup işlemini makineleri kapatmadan da yapabilirsiniz fakat oluşabilecek hatalara yada veri kaybına karşın, kapatıp yapmak sağlıklı olacaktır. Tabi illaki makinenin hizmet kesintisi olmaması gerekiyorsa, dediğim gibi **vm** açıkkende **backup** alabilirsiniz.

**2.Restore**

Şimdi yedekten geri dönme senryosunu uygulayalım. Bunun için **XML**’i silebilir yada unde ne edebilirsiniz.

~~~
virsh unde ne Ubuntu18
~~~

ya da

Sunucu özelliklerinin barındığı **XML** dosyası silinmiş olsun ve listelediğimizde makinenin gittiğini görebiliyoruz.

![Crepe](/assets/img/kvm-bac-rest/kvm-br06.png)

~~~
virsh list --all
~~~

![Crepe](/assets/img/kvm-bac-rest/kvm-br07.png)

Şimdi senaryoyu gerçekleştirmek için disk (**qcow2**) dosyasını silelim.

~~~
rm /var/lib/libvirt/images/Ubuntu18.qcow2
~~~

Ve senaryo uygulamasına başlayabiliriz. Hemen backup aldığımız yerden diskin (**qcow2**)’un olması gereken yere kopyalama işlemine başlayalım.

~~~
cp /MyBackup/Ubuntu18.qcow2 /var/lib/libvirt/images/
~~~

Diskin kopyalama işlemi bittikten sonra makine özelliklerinin olduğu **XML** dosyasını aşağıdaki komut/yöntem ile kullanabilir hale getirelim.

~~~
virsh define -file /MyBackup/Ubuntu18.xml
~~~

ya da

Sunucu özelliklerinin barındığı **XML** dosyası bulunduğu dizine tekrar kopyalanmış olsun.

![Crepe](/assets/img/kvm-bac-rest/kvm-br08.png)

Ardından vm’i başlatarak işlemi tamamlayalım.

~~~
virsh start Ubuntu18
~~~

![Crepe](/assets/img/kvm-bac-rest/kvm-br09.png)
