---
layout: post
title: MySQL Change-Reset User Password
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [mysql, linux, database]
comments: true
---
![Crepe](/assets/img/mysql-pass-re/m-p-res01.png)

Yetkili bir kullanıcı ile **Mysql**‘e login olunur.

~~~
mysql -u root -p
~~~

Birden fazla veritabanı varsa aşağdaki komut ile listelenebilir.

~~~
show databeses;
~~~

Ardından kullanıcının bulunduğu veritabanı seçilir.

~~~
use “database_name”;
~~~

Aşağıdaki komut ile de veritabanındaki tablolar listelenebilir.

~~~
show tables;
~~~

Tablodaki alanları ise aşağıdaki komut ile listeleyebilirsiniz.

~~~
describe “tables_name”;
~~~

Tablodaki tüm verileri görmek için aşağıdaki komutu kullanabilirsiniz.

~~~
select * from “tables_name”\G;
~~~

Ya da aşağıdaki gibi özelleştirerek verileri çağırabilirsiniz.

~~~
select * from wp_users where ID=1;
~~~

Şimdi son olarak ilgili kullanıcının şifresini resetleyelim.

~~~
UPDATE wp_users SET user_pass=MD5(‘123456’) where ID = 1;
~~~

**NoT: Clear text giden şifre “vim ~/.mysql_history” dosyasından silinmelidir.**
