---
layout: post
title: Ubuntu 14.04 GUI (Grafik Arayüzü) Kurulumu
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [linux, ubuntu]
comments: true
---
Sisteme **root** ile **login** olduktan sonra aşağıdaki komutları sırasıyla çalıştırın. **Root** hakkına sahip olmayan başka bir **user** ile **login** olduysanız komutun başına ‘**sudo**’ ekini ekleyin. Ubuntu için iki tür **GUI** kurulumundan bahsedeceğim. ilk olarak **ubuntu unity desktop** kurulumuna bakalım. Aşağıdaki komutu çalıştırarak **ubuntu unity desktop GUI** kurulumunu gerçekleştirebilirsiniz. Ardından Ubuntu klasik masaüstü **GUI** kurulumundan bahsedeceğim,

~~~
apt-get install ubuntu-desktop -y
~~~

Yükleme işlemi biraz sürecektir. Daha sonrasında sistemi reboot etmeden aşağıdaki komutu çalıştırarak GUI moda geçebilirsiniz. Sorun oluşması durumunda reboot la düzelecektir.

~~~
startx
~~~

Ve başarılı kurulum sonrası sisteme **GUI** arayüzü ile login olalım.

![Crepe](/assets/img/u14gui/ugui01.png)

Ve Ubuntu unity desktop

![Crepe](/assets/img/u14gui/ugui02.png)

**Gnome** Klasik masaüstü kurulumu için ise aşağıdaki komutu kullanabilirsiniz. **Root** hakkına sahip olamayan bir user ile login olmuşsanız komut başına ‘**sudo**’ ekini ekleyin. **Login** olduğunuz **user**’ın şifresini tekrar soracaktır, girdikten sonra yükleme başlayacaktır.

~~~
sudo apt-get install gnome-session-fallback -y
~~~

Kurulum bittikten sonra **logout** olun.
Oturum açma ekranında aşağıda görüldüğü gibi küçük ubuntu logosuna tıkladıktan sonra masaüstü menüsü açılacaktır. Seçimi yapıp **login** olduktan sonra sistem klasik **GUI** ile açılacaktır.

![Crepe](/assets/img/u14gui/ugui03.png)

Ve Ubuntu Gnome klasik desktop

![Crepe](/assets/img/u14gui/ugui04.png)

