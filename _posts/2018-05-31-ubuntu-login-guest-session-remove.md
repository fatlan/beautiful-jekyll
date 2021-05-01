---
layout: post
title: Ubuntu Login Ekranı Guest Session Kaldırma - Removing Guest Session at Login in Ubuntu
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [ubuntu, session, guest, login]
comments: true
---
![Crepe](/assets/img/ubuntu-log-guest-session/ulgsession-r01.png)

**Ubuntu** açılış ekranında oluşturduğunuz kullanıcı dışında **default** olarak gelen **Guest Session** oturumuda bulunmaktadır ve bu kullanıcı ile şifresiz olarak **login** olabilirsiniz. Bu kullanıcı oldukça kısıtlı haklara sahip olmasına rağmen güvenlik endileşelerinden dolayı(hak yükseltme saldırılarına karşın) yada bir çok farklı olabilecek sebepten dolayı kaldırmak isteyebilirsiniz. Şimdi aşağıdaki yöntemle bu işlemi nasıl yapacağımıza bir göz atalım.

**Default** gelen ubuntu açılış ekranı yukarıdaki gibidir.

Şimdi aşağıdaki dosyayı(**50-ubuntu.conf**) **edit** edin. Ben **nano** kullandım.

~~~
sudo nano /usr/share/lightdm/lightdm.conf.d/50-ubuntu.conf
~~~

Daha sonra aşağıdaki satırı ekleyip kaydedip çıkın.

~~~
allow-guest=false
~~~

![Crepe](/assets/img/ubuntu-log-guest-session/ulgsession-r02.png)

Ardından makinayı **reboot** edin.

Ve **Login** ekranının son hali.

![Crepe](/assets/img/ubuntu-log-guest-session/ulgsession-r03.png)
