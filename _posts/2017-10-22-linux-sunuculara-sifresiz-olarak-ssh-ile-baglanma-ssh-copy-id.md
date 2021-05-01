---
layout: post
title: Linux Sunuculara Şifresiz Olarak SSH ile Bağlanma – SSH Copy ID
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [ssh, ssh-keygen, linux, keyfile]
comments: true
---
![Crepe](/assets/img/linux-ssh-copyid/lin-ssh-cid01.png)

Normalde client makinenizden linux sunuculara ssh bağlantısı için komut satırana **ssh kullanıcıadı@remoteip** tıkladıktan sonra şifre istenir ve şifreyi girdikten sonra sunucuya başarılı bir şekilde login olabilirsiniz.

**SSH Copy ID** ile sunuculara bağlanırken şifre sormadan login olabileceğiz. Bu işlemin gerekli olduğu durumlar, oluşturduğunuz script’lerde parola sorunu yaşamadan yada dosya çekip, atarken şifre problemi yaşamamak için lazım olabilecek bir yöntemdir.

Bunun için ilk önce Client makinemizde aşağıdaki gibi **ssh-keygen** komutunu çalıştıracağız. Bu komutu çalıştırdıktan sonra **Enter File** kısmında direk **Enter** tuşuna basabilir yada benim gibi bir **public key**’in dosya ismini(**LinuxSunucum.pub**) verebilirsiniz. Burada sadece şu fark var, siz isimlendirirseniz dosya oluşturulan kullanıcının **Home** dizininin altında oluşacak. Hiç birşey girmeden **Enter** derseniz orda oluşacak dosya ismi **id_rsa.pub** olacaktır ve bu dosyalar **.ssh/** dizini altında oluşacaktır. Daha sonra **Enter passphrase** ve **Enter same passphrese** kısmını iki **Enter** ile geçin. Yani toplamda üç kere **Enter** tuşuna basacaksınız.

**NOT**: **ssh-keygen** direk çalıştırdığınızda oluşturulan şifreleme yöntemi **RSA**’dir. **DSA** oluşturmak için komutu “**ssh-keygen -t dsa**” olarak kullanmalısınız. Arasındaki fark ise kabaca **RSA(v1)**, **DSA(v2)**’dir.

![Crepe](/assets/img/linux-ssh-copyid/lin-ssh-cid02.png)

Oluşan dosyayı **ls** kontol edelim ve kullanıcının **home** dizininde olduğunu keşfedelim.

![Crepe](/assets/img/linux-ssh-copyid/lin-ssh-cid03.png)

Birde isim vermeden oluşturdum, onunda **.ssh/** dizini altında oluştuğunu görelim.

![Crepe](/assets/img/linux-ssh-copyid/lin-ssh-cid04.png)

Tabi ben hiyerarşiyi bozmamak adına **LinuxSunucum** dosyalarını **mv** komutu ile **.ssh/** dinizi altına atıyorum ve son görünüm aşağıdaki gibidir.

~~~
mv LinuxSunucum* .ssh/
~~~

![Crepe](/assets/img/linux-ssh-copyid/lin-ssh-cid05.png)

Şimdi oluşturduğumuz public key’i aşağıdaki komut aracılığı ile sunucuya kopyalıyoruz, kopyalama sırasında bir kereye mahsus şifreyi girip başarılı bir şekilde key’i kopyaladığımızı görüyoruz.

~~~
ssh-copy-id -i ~/.ssh/LinuxSunucum.pub root@192.168.2.146
~~~

![Crepe](/assets/img/linux-ssh-copyid/lin-ssh-cid06.png)

Şimdi **SSH** ile sunucuya bağlanıp şifresiz olarak bağlandığımızı görelim.

~~~
ssh root@192.168.2.146
~~~

![Crepe](/assets/img/linux-ssh-copyid/lin-ssh-cid07.png)

Sunucu tarafına attığımız **key** dosyasını görmek ve sağlamasını yapmak için aşağıdaki adımları gerçekleştirebiliriz.

~~~
ls -lash .ssh/
~~~

![Crepe](/assets/img/linux-ssh-copyid/lin-ssh-cid08.png)

**authorized_keys** dosyasını okursak eğer **root@bt public key** bilgileri görmemiz gerekir ki aşağıdaki gibi görebiliyoruz.

~~~
cat .ssh/authorized_keys
~~~

![Crepe](/assets/img/linux-ssh-copyid/lin-ssh-cid09.png)

**MacBook** bir cihaz üzerinden **Linux** sunuculara erişim sağlayacaksanız, **brew** paket yöneticisi ile **ssh-copy-id** paketini kurup yukardaki işlemler ile yapabilirsiniz.

~~~
brew install ssh-copy-id
~~~
