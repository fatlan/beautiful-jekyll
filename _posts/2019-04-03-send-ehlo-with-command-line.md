---
layout: post
title: Komut Satırı Üzerinden Mail Gönderme EHLO/HELO - Mail Server Test with Telnet on Linux
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [ehlo, mail, telnet, linux]
comments: true
---
![Crepe](assets/img/ehlo-with-telnet/ehlo-tl01.png)

ilk olarak mail server’a **telnet** ile **25** portu üzerinde bağlanalım.

~~~
telnet mailserver.com.tr 25
HELO deneme.com.tr
mail from: id@example.com
rcpt to: fatih.aslan@example.com
data #mail içeriğini bu kısımda hazırlayacağız
subject: test
 #mesaj bloğu
. #nokta işareti mailimiz tamam oldu, gönder manasına gelmektedir
~~~

![Crepe](assets/img/ehlo-with-telnet/ehlo-tl02.png)

