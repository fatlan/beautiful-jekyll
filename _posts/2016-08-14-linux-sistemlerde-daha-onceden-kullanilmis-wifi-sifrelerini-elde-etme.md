---
layout: post
title: Linux Sistemlerde Daha Önceden Kullanılmış Wi-Fi Şifrelerini Elde Etme
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [wifi, linux, pass, şifre, network]
comments: true
---
**Linux** işletim sistemimizde daha önceden kullanılmış ve hali hazırda kullanılan **wi-fi ssid** ve şifrelerini iki yolla elde edebiliriz. Bunlardan birincisi **GUI(graphical user interface)** diğeri **terminal** aracılığı iledir. Biz her iki yöntemi kullanarak gösterilim.

ilki **GUI** ile; ben işlemleri Ubuntu üzerinde göstereceğim, diğer dağıtımlarda görünüm farklılık gösterebilir.

ilk olarak sistemin sağ üst köşesinde bulunan **wi-fi** yayın işaretini tıklayıp ardından **Edit Connections...** sekmesini tıklayın.

![Crepe](/assets/img/lin-wifi-pass-disc/lin-wifi-pass01.png)

Daha sonra kırmızı alanla listelenen **wi-fi** bağlantılarınızdan şifresine ulaşmak istediğiniz bağlantıyı tıklayarak **Edit** butonuna basın.

![Crepe](/assets/img/lin-wifi-pass-disc/lin-wifi-pass02.png)

Ardından **Wi-Fi Security** sekmesini tıklayarak **Show password** seçeneğini **aktif** edin ve **şifre** görünür olacaktır.

![Crepe](/assets/img/lin-wifi-pass-disc/lin-wifi-pass03.png)

İkinci yöntem ise **Terminal** kabuk aracılığı ile;

Aslında bağlandığınız **wi-fi**’ler şifrelenmemiş **text** dosyası olarak sistemde saklanmaktadır. Bizler o **text** dosyasına ulaşıp içini okuma yöntemi ile bilgilere erişiyoruz. Tabi bu işlemleri **root** olarak yapmalısınız değilseniz komutların başında **sudo** ekini kullanınız. İlk olarak **wi-fi** bağlantıları **clear text** olarak saklanan **directory**’e (**/etc/NetworkManager/system-connections/**) **cd** komutu ile giriyoruz.

~~~
cd /etc/NetworkManager/system-connections/
~~~

![Crepe](/assets/img/lin-wifi-pass-disc/lin-wifi-pass04.png)

Ardından **ls -lash** komutu ile **wi-fi** bağlantılarını barındırdığı şifrelenmemiş **text** dosyalarını detaylı olarak listeliyoruz.

~~~
ls -lash
~~~

![Crepe](/assets/img/lin-wifi-pass-disc/lin-wifi-pass05.png)

Daha sonra kırmızı alandaki **wi-fi** bağlantılarından şifresini elde etmek istediğimiz **clear text** dosyasını **cat** komutu ile okuyoruz. Ve **SSID**, Şifre ve ayrıca diğer bilgileri elde ettik.

~~~
cat ‘wifi baglantiadi’
~~~

![Crepe](/assets/img/lin-wifi-pass-disc/lin-wifi-pass06.png)
