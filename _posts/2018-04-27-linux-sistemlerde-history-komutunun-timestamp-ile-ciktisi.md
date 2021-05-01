---
layout: post
title: Linux Sistemlerde History Komutunun TimeStamp ile Çıktısı
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [linux, bash]
comments: true
---
**Linux** sistemlerde çalıştırılan komutların history de, **tarih/saat/dakika** cinsinden tutturmak ve daha sonra bunları **timestamp**'lı olarak **history** komutu elde etmek için aşağıdaki adımları takip edin.

Tüm kullanıcılarda aynı formatta tutturmak için, **/etc/profile** yada **/etc/.bashrc**

Sadece belirli bir kullanıcıda yapmak için, **.bashrc** yada **bash_profile(.profile)**, dosyalarından birini **vi/vim/nano** aracı ile edit edip en alt satıra aşağıdaki parametreyi ekleyin.

~~~
export HISTTIMEFORMAT=”%F %T “
~~~

![Crepe](/assets/img/histr-tistamp/his-t-stam01.png)

Daha sonra sistemi yeniden başlatın yada değişiklik yaptığınız dosyayı **source** edin, aşağıdaki gibi.

~~~
source /etc/profile
~~~

![Crepe](/assets/img/histr-tistamp/his-t-stam02.png)
