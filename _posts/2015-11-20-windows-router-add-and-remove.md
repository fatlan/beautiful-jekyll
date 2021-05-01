---
layout: post
title: Windows Komut Satırından Route Ekleme ve Silme (Netsh ve Route komutlarının kullanımı)
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [windows, network]
comments: true
---
Herhangi makinada birden fazla NIC dahi olsa network kon gurasyonları için birden fazla gateway(geçityolu) adresi giremiyoruz. Bu gibi durumlarda herhagi bir ayar yada oluşturulan tunellemelerde istenilen network’ü istenilen gateway’e yönlendirmek için route’leme işlemi kullanılır. Kısaca farklı networklerin birbirleriyle haberleşmek için hangi yolu kullanması gerektiğinin hesaplanması ya da seçilmesi işlemidir. Bunun için iki yöntemden yani iki komut setinden bahsedeceğim ilk olarak NETSH ve sonrasında ROUTE komutu.

**NETSH komutunu kullanarak route eklemek**

Komut satırını admin modda açtıktan sonra(admin modda açılmazsa hata verecektir) aşağıdaki komutları çalıştırın.

![Crepe](assets/img/wraar/winrouadd01.png)

İlk önce intefaceleri(NIC) listeleyelim. Ben tüm işlemler için “Ethernet0” interface’yi kullanacağım.

~~~
netsh interface ipv4 show interface
~~~

![Crepe](assets/img/wraar/winrouadd02.png)

Route ekleyelim, test için adresleri tamamen atıyorum.

~~~
netsh interface ipv4 add route ‘subnet’ “İnterfaceADI” ‘gateway’
~~~

![Crepe](assets/img/wraar/winrouadd03.png)

Ardından route tablosunu görmek için aşağıdaki komutu kullanabilirsiniz.

~~~
netsh interface ipv4 show route
~~~

![Crepe](assets/img/wraar/winrouadd04.png)

Ben aşağıdaki komutu kullanıyorum.

~~~
route print
~~~

![Crepe](assets/img/wraar/winrouadd05.png)

Yapılan işlemleri geri almak yani silmek için aşağıdaki komutu kullanabilirsiniz.

~~~
netsh interface ipv4 delete route ‘subnet’ “İnterfaceADI” ‘gateway’
~~~

![Crepe](assets/img/wraar/winrouadd06.png)

**ROUTE komutunu kullanarak route eklemek**

Komut satırını admin modda açtıktan sonra(admin modda açılmazsa hata verecektir) aşağıdaki komutları çalıştırın.

![Crepe](assets/img/wraar/winrouadd01.png)

Route ekleyelim, test için adresleri tamamen atıyorum. Yalnız burda bir püf nokta var komutun sonuna ‘-p’ parametresini eklemezseniz yaptığının routing işlemi makina yeniden başladıktan sonra kaybolacaktır. Kalıcı olması için ‘-p’ parametresini kullanmalısınız. Opsiyonel olarak kullanabileceğiniz parametleri route yazarak görebilirsiniz.

~~~
route add subnet MASK 255.255.255.0 gateway
~~~

![Crepe](assets/img/wraar/winrouadd07.png)

~~~
route print
~~~

![Crepe](assets/img/wraar/winrouadd08.png)

~~~
route delete subnet MASK 255.255.255.0 gateway
~~~

![Crepe](assets/img/wraar/winrouadd09.png)


