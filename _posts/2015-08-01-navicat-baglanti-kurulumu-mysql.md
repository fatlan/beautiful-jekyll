---
layout: post
title: Navicat Bağlantı Kurulumu – Mysql
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [database, mysql]
comments: true
---

![Crepe](assets/img/navicat-mysql-connect/navmy01.png)

Navicat‘ı kısaca anlatacak olursak görsel arayüz aracılığı ile veritabanı yönetim aracıdır. Navicat ile local bilgisayarınızdan uzaktaki sunucuzda bulunan MySql veritabanınıza bağlanmayı anlatacağım.
İlk öncesinde local bilgisayardan Navicat’ın resmi sitesine bağlanıp MySql Tool’unun Trial versiyonunu indiriyorum. Siz lisanslı versiyonunu kullanıyor olabilirsiniz. Daha sonrasında kurulumu gerçekletirip, programı çalıştırın. Bu işlemler gayet basit hele ki windows kullanıyorsanız iki next bi finiş =)

![Crepe](assets/img/navicat-mysql-connect/navmy02.png)

Programı çalıştırdıktan sonra **Connection** sekmesinden **MySql** butonunu tıklıyoruz.

![Crepe](assets/img/navicat-mysql-connect/navmy03.png)

Çıkan ekranda aşağıdaki prosedürü uygulayın. **Host** kısmına MySql sunucunuzun **IP** numarası yazılacak, **Port** kısmına aksi belirtilmedikçe MySql Default port number **3306**’dır, **User Name** kısmıda MySql kullanıcısı olup aksi belirtilmedikçe **root** olur, **Password** kısmı MySql şifresi girilecektir.

![Crepe](assets/img/navicat-mysql-connect/navmy04.png)

Daha sonra herşeyin doğru olduğundan emin olmak için **Test Connection** butonuna basın

![Crepe](assets/img/navicat-mysql-connect/navmy05.png)

Aşağıdaki gibi hata alacaksınız.

![Crepe](assets/img/navicat-mysql-connect/navmy06.png)

Bunun nedeni tabiki MySql olan uzak sunucunuzda Navicat ile giriş yapmak istediğiniz bilgisayar için izin vermediniz. Bunun için sunucunuza gidip Aşağıdaki komutları çalıştırın.

~~~
#mysql -u root -p
password:
~~~

mysql komut satırında iken aşağıdaki komutları veriyoruz.

~~~javascript
mysql> use mysql
mysql> grant all on *.* to root@’Bağlanmak istediğiniz local bilgisayarınızın IP adresi’ identified by ‘root-şifreniz’;
mysql> FLUSH PRIVILEGES;
~~~

Daha sonrasında bu komutları başarılı bir şekilde çalıştırdıktan sonra tekrar **Test Connection** butonuna basın ve aşağıdaki gibi başarılı olduğunu görün.

![Crepe](assets/img/navicat-mysql-connect/navmy07.png)

**Ok** butonuna basın ve bağlantınız eklenmiş olacak, tekrar çift tıklayın. Ve karşınızda veritabanlarınız, tablolarınız vs.

![Crepe](assets/img/navicat-mysql-connect/navmy08.png)

