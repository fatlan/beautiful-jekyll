---
layout: post
title: CHROOT ile Linux ve Container’lardaki izolasyon meselesini anlayalım
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [chroot, linux, container, jail, virtualiaztion]
comments: true
---

**Chroot** adından da anlaşılacağı üzere **change root** olarak, **kök** dizini değiştirme ya da başka bir deyişle izole edilmiş farklı sanal **kök** dizinleri oluşturmak için kullanılır. Yani servis ve uygulamaların çalıştırdığı her bir **process**’in gördüğü ya da çalıştığı “**root**” “**kök**” “**/**” dizini değiştirmek için kullanılır.

İnternetten bulduğum aşağıdaki görsel ile durum daha kolay anlaşılabilir.

![Crepe](/assets/img/chroot-izo/chroot01.png)

Şimdi biz **chroot**’u kullanarak kendi **process**’lerimiz için oluşturacağımız **sanal root** dizini ile **Linux** ve **Container** için dosya sistemi izolasyon konusunu anlamaya çalışalım.

**NoT**: **Bash**, her kullanıcı için ilk başlangıç **progress**’idir(çünkü **bash** ile yönetimimizi gerçekleştiririz), her hizmet ve uygulama altında çalışan süreçler olarak yer alır. Bundan ötürü bizde **bash**’e **chroot** uygulayarak konuyu ele alacağız.

İlk önce kendi **sanal root directory**’imizi oluşturalım

~~~
mkdir jailroot
~~~

Sonra komutları atacağımız **bin** klasörünü oluşturalım

~~~
mkdir jailroot/bin
~~~

Akabinde **bash** ve istediğimiz **binary executable**(komutlar)leri bu dizin altına atacağız. Ben **bash**, **ps**, **pwd**, **ls**, **cat**, **mount**, **w**, **id**, **tree** komutlarını bu dizin altına atacağım ve onunla beraber bir takım klasörler oluşturacağım.

Fakat bu komutların çalışabilmesi için bağımlı olduğu kütüphaneleri de bu **sanal root**(**jailroot**) dizinin altına gönderilmesi gerekir.

Bash ve diğer komutlara ait bağımlı lib’leri görebilmek için “**ldd**” ve “**readelf**” komutlarını kullanabilirsiniz.

~~~
ldd /bin/bash
~~~
~~~
readelf -d /bin/bash | egrep -i "needed|rpath"
~~~

![Crepe](/assets/img/chroot-izo/chroot02.png)

Şimdi dosyaları **sanal root**(**jailroot**)un altına atalım. Tabi bağımlı kütüphane dosyalarını da gönderelim. Normalde yukarıdaki komutlar ile sadece kullanacağınız komutların kütüphane dosyalarını bulup atabilirsiniz ama ben direk uğraşmadan olduğu gibi atıyorum.

~~~
cp -a /lib jailroot/

cp -a /lib64 jailroot/

cp /bin/bash jailroot/bin/

cp /bin/ls jailroot/bin/

cp /bin/cat jailroot/bin/

cp /bin/mount jailroot/bin/

cp /bin/ps jailroot/bin/

cp /bin/pwd jailroot/bin/

cp /usr/bin/w jailroot/bin/

cp /usr/bin/id jailroot/bin/

cp /usr/bin/tree jailroot/bin/
~~~

Hiyerarşiye uygun bir takım klasörler oluşturalım.

~~~
mkdir	jailroot/opt

mkdir	jailroot/var

mkdir	jailroot/tmp
~~~

Son olarak **change root** yapalım.

~~~
chroot jailroot /bin/bash
~~~

![Crepe](/assets/img/chroot-izo/chroot03.png)

Tüm attığımız komutlar çalışıyor, özellikle **pwd** ve **tree** komutlarını çalıştırdığınızda **kök directory**’nin izole olarak göründüğünü fark edebilirsiniz. Fakat **w** ve **ps** koutlarında aşağıdaki gibi uyarı alacaksınız.

![Crepe](/assets/img/chroot-izo/chroot04.png)

Bu komutların çalışabilmesi için en tepedeki **/proc** süreçler dosyalarının saklandığı dizinin **proc** olarak **chroot** alanına **mount**(**mount proc /proc -t proc**) edilmesi gerekiyor uyarısı veriyor.

Bu yüzden aşağıdaki gibi **proc** klasörünü oluşturalım ve **mount** komutunu çalıştıralım.

~~~
mkdir	jailroot/proc
~~~
~~~
mount proc /proc -t proc
~~~

Ve sırasıyla **w**, **ps** ve **mount** komutlarını **run** edersek sorunsuz bir şekilde çalıştığını görebiliriz.

![Crepe](/assets/img/chroot-izo/chroot05.png)

Böylelikle **dosya sistemi izolasyon** konusunu, hatta **volume mount** konusuda aklımıza yakınlaştırmış olduk.

**Tabi konu aslında bunlarla sınırlı değil kaldı ki chroot tam yalıtım sağlamaz ve etkileri yalnızca kök bağlama noktasıyla sınırlıdır. Asıl önemli olan namespaces’ler ve cgroup’lardır ki container teknolojileri bunların üzerine bina edilmiştir.**
