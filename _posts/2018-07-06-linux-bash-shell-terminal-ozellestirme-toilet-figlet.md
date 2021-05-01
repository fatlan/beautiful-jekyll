---
layout: post
title: Linux Bash Shell Terminal Özelleştirme - toilet and figlet
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [toilet, figlet, linux, bahs, terminal]
comments: true
---
![Crepe](assets/img/toilet-figlet/tandfi01.png)

**Linux terminal**’ini açılışta isim soyisim yada ünvan yada reklam gibi yazılar yazdırmak ve bunu her açılışta otomatik yaptırmak gibi bir fantazi isterseniz aşağıdaki adımları izleyebilirsiniz.

İlk önce aşağıdaki komutla **toilet** ve **figlet** paketlerini kurun.

~~~
sudo apt install toilet figlet
~~~

Ardından **home directory**’inizin olduğu yerdeki **.bashrc** dosyasını **edit**’leyip,

~~~
vi .bashrc
~~~

Son satıra aşağıdaki satırı ekleyip, kaydedip çıkın.

~~~
figlet Fatih ASLAN fatlan.com ya da

toilet -f standard -F metal ‘Fatih ASLAN fatlan . com’
~~~

İşlem tamam, terminali her açtığınızda yukarıdaki gibi görünecektir.