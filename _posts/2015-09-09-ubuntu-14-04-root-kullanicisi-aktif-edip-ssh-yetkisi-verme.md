---
layout: post
title: Ubuntu 14.04 root kullanıcısını aktif edip SSH yetkisi verme
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [ubuntu, linux, ssh]
comments: true
---
Ubuntu için oluşturduğunuz herhangi bir user ile login olduktan sonra aşağıdaki komutu çalıştırın.

~~~
sudo passwd root
~~~

Komutu çalıştırdıktan sonra login olduğunuz kullanıcının şifresini soracak, girdikten sonra **root** için yeni şifreyi girmenizi ardından tekrar girmenizi isteyecek. Sonrasında sisteme **root** kullanıcısı ile login olabilirsiniz. Çıktı aşağıdaki gibidir.

![Crepe](assets/img/ubunt-roo-ssh/urssh01.png)

Şimdi **root**’un **SSH** ile bağlanabilmesi için aşağıdaki yönergeleri uygulayın. Görülceği üzere aşağıdaki gibi işlemleri yapmadan önce SSH ile bağlantı denediğimde **Access denied** alıyorum.

![Crepe](assets/img/ubunt-roo-ssh/urssh02.png)

İlk olarak aşağıdaki komutu çalıştırarak **sshd_config** dosyasına girin.

~~~
sudo vi /etc/ssh/sshd_config
~~~

Dosyanın içindeki **PermitRootLogin without-password** satırını **PermitRootLogin yes** ile değiştirin.

![Crepe](assets/img/ubunt-roo-ssh/urssh03.png)

![Crepe](assets/img/ubunt-roo-ssh/urssh04.png)

Daha sonra SSH servisini restart edin.

~~~
service ssh restart
~~~

Şimdi **root** için **SSH** bağlantısını tekrar deneyelim ve başarılı olduğunu görelim.

![Crepe](assets/img/ubunt-roo-ssh/urssh05.png)
