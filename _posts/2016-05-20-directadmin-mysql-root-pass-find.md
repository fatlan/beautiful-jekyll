---
layout: post
title: MySQL root parolasını öğrenme - How can I find out Mysql root password
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [directadmin, mysql, database, pass, root, linux]
comments: true
---
![Crepe](assets/img/directadmin-mysql-root-pass/da-mysql-root-p01.png)

**DirectAdmin** sunucunuzda **phpmyadmine** yada **db** ye bağlanacaksınız ama **MySQL** **root** parolasını unutmuş yada bilmiyor olabilirsiniz. Fakat sunucuya **login** olabiliyorsanız **MySQL** şifresini elde edebilirsiniz.

Bu işlem için sunucuya konsol yada **ssh** yöntemi ile login olduktan sonra aşağıdaki komutu çalıştırarak gerçekleştirebilirsiniz.

~~~
cat /usr/local/directadmin/conf/mysql.conf
~~~

