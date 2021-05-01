---
layout: post
title: Wordpress wp-admin Güvenliği - WP Login Page Security on Apache
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [security, wordpress, linux, apache]
comments: true
---
![Crepe](/assets/img/wp-apache-losec/wp-a-losec01.png)

WordPress blog’larınızın admin sayfasına yapılan bruteforce ataklarından oldukça sıkılmış olabilirsiniz ki varsayılan olarak wp-admin eki ile bağlandığınız admin panel sayfasının herkes için açılmasını yasaklamanız güvenlik adına yapmanız gereken en mantıklı hareketlerden biri olacaktır.

Bunu aşağıdaki yapılandırmaları yaparak plugin kurmadan, basitçe sağlayabilirsiniz.

İlk olarak “**nano /etc/httpd/conf/httpd.conf**” dosyasını **nano** ile **edit** ediyoruz. Siz farklı bir editörde seçebilirsiniz.

Ardından aşağıdaki gibi sayfayı açabilme yetkisine sahip olacak(**allow**) **ip** adreslerini ekleyin. Yazacağınız **ip** adreslerinin dışındaki ip adresleri **wp-admin** sayfasını açamayacaktır. **Yetkin yok uyarısı** verecektir.

~~~
<Directory “/var/www/html/fatlan.com/wp-admin”>
    Order allow,deny
    Allow from 19.17.77.0/24
    Allow from 10.1.11.0/24
    Allow from 10.1.90.0
</Directory>
~~~

Son olarak ise apache(httpd) servisini **restart** edin

~~~
systemctl restart httpd.service
~~~

ya da

~~~
/etc/init.d/httpd restart
~~~