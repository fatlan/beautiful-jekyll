---
layout: post
title: oVirt Internal Admin Şifresini değiştirme - Changing the Password for admin@internal(ovirt admin@internal password change)
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [ovirt, rhev, linux, virtualization]
comments: true
---
![Crepe](/assets/img/ovirt-ad-pc/ov-pass-adc01.png)

Aşağıdaki komut ile internal admin kullanıcısının detaylarını görebilirsiniz. Yukarıdaki ss’de de durumu gözlemleyebilirsiniz.

~~~
ovirt-aaa-jdbc-tool user show admin
~~~

Şifreyi **reset**’lemek için ise aşağıdaki komutu kullanabilirsiniz. Sorun olması durumunda (**--force**) parametresini kullanabilirsiniz.

~~~
ovirt-aaa-jdbc-tool user password-reset admin
~~~

Ardından Engine servisini yeniden başlatmalısınız.

~~~
systemctl restart ovirt-engine.service
~~~
