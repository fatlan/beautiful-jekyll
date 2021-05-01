---
layout: post
title: Data Link Connection Test - SQL DB Connection Test
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [sql, database, test, connection, windows]
comments: true
---
Uzaktaki **SQL server** sunucusunda oluşturulan **db** ile bağlantı var mı yok mu yada böyle bir db mevcut mu gibisinden; anlayabilmek için basit bir yöntem ile test edelim.

İlk önce sunucuda global olarak **SQL port**u açık olması gerekiyor yani sorgulara dışarıdan cevap veriyor olması gerek. Bunu **local** de özel komutlarla test edebileceğiniz gibi online olarak ta test edebilirsiniz.

Bunun için benim de kullandığım [http://www.t1shopper.com/tools/port-scan/](http://www.t1shopper.com/tools/port-scan/) sitesini kullanabilirsiniz.

![Crepe](assets/img/sql-db-conn-test/db-con-t01.png)

Sorgu cevabı olarak aşağıdaki gibi pozitif dönmeli,

![Crepe](assets/img/sql-db-conn-test/db-con-t02.png)

Daha sonra masaüstünde yeni bir **text** dosyası oluşturun fakat bu dosyanın uzantısı ***.txt** olarak görünür olmalı,

![Crepe](assets/img/sql-db-conn-test/db-con-t03.png)

Değil ise; **folder options**(klasör seçenekleri) kısmından **hide extensions for known file types uncheck** edip, onaylayın.

![Crepe](assets/img/sql-db-conn-test/db-con-t04.png)

Daha sonra oluşturduğunuz ***.txt** dosyasının uzantısını ***.udl** olarak değiştirin ve son görünüm aşağıdaki gibi olmalıdır.

![Crepe](assets/img/sql-db-conn-test/db-con-t05.png)

Daha sonra **çift** tıklayarak çalıştırın. Ardından 1. bölüme **SQL** sunucunun ip adresi **DNS** kaydı varsa **domain** ismide olabilir, 2. bölüme **db** ismi ve şifresini yazdıktan sonra T**est Connection** butonuna basarak testi gerçekleştirebilirsiniz.

![Crepe](assets/img/sql-db-conn-test/db-con-t06.png)

![Crepe](assets/img/sql-db-conn-test/db-con-t07.png)


Bu işlemleri uygulama kurarakta yapabilirsiniz tabi uygulama daha bir çok özellik desteklemektedir. Free mevcut [http://www.sqlmanager.net/en/downloads#mssql](http://www.sqlmanager.net/en/downloads#mssql)