---
layout: post
title: IBM Power Sunucularda IPMITOOL Kullanımı
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [ibm, ipmitool, linux, ilo, remote]
comments: true
---
![Crepe](/assets/img/ibm-ipmitool-use/ibm-ipmi-to01.png)

**IBM** sunucular donanımsal olarak uzaktan kontrol edilebilir yapıdadır. Sunucunun fiziksel olarak yeniden başlatılması, kapatılması, açılması gereken durumlarda Web Konsol yerine **ipmitool** komutları kullanılabilinir. Hatta bağlantığınız konsoldan işletim sistemi seviyesine konsoluna geçip işlemler yapabilirsiniz. Bu işlemi öncesinde sunucuya **IPMITOOL IP SET** ile atadığımız ip’yi kullanarak yapacağız. Kullanıcı tarafında **ipmitool** paketini kurmalısınız.

IPMITOOL default kullanıcı adı ve şifre aşağıdaki gibidir,

- **Default User : ADMIN**

- **Default Pass : admin**

Power ile ilgili neler yapılabileceğini görmek için aşağıdaki komut,

~~~
ipmitool -I lanplus -H server_ip_address -U ADMIN -P admin power
~~~

![Crepe](/assets/img/ibm-ipmitool-use/ibm-ipmi-to02.png)

Power durumunu görmek için aşağıdaki komut,

~~~
ipmitool -I lanplus -H server_ip_address -U ADMIN -P admin power status
~~~

![Crepe](/assets/img/ibm-ipmitool-use/ibm-ipmi-to03.png)

**Power OFF** yapmak için aşağıdaki komut,

~~~
ipmitool -I lanplus -H server_ip_address -U ADMIN -P admin power off
~~~

![Crepe](/assets/img/ibm-ipmitool-use/ibm-ipmi-to04.png)

**IPMI** Konsoluna düşmek için aşağıdaki komutu kullanabilirsiniz.

~~~
ipmitool -I lanplus -H server_ip_address -U ADMIN -P admin sol activate
~~~

Herhangi bir sebepten dolayı konsolu devre dışı bırakmak için aşağıdaki komutu kullanabilirsiniz.

~~~
ipmitool -I lanplus -H server_ip_address -U ADMIN -P admin sol deactivate
~~~

Yardım için **ipmitool -H** parametresini kullanabilirsiniz.