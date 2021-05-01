---
layout: post
title: Ovirt_RHEV 4.2 Engine LDAP Yapılandırma - Bölüm 7
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [ovirt, rhev, ldap, linux]
comments: true
---
Engine login olduktan sonra, aşağıdaki komutla ilgili yapılandırma paketin varlığını kontrol edelim.

~~~
rpm -qa | egrep -i ovirt-engine-extension-aaa-ldap-setup
~~~

Eğer paket yoksa aşağıdaki komut ile kurulumu gerçekleştirelim.

~~~
yum install ovirt-engine-extension-aaa-ldap-setup
~~~

Şimdi aşağıdaki komut ile yapılandırmaya başlayaşlım.

~~~
ovirt-engine-extension-aaa-ldap-setup
~~~

Komutu girdikten sonra aşağıdaki gibi sizden belirtilen çeşitlerdeki **LDAP**‘lardan bağlantı sağlayacağınız **LDAP**‘ın ilgili numarasını girmenizi beklemektedir. Ben **FreeIPA** kullandığım için 6 numarayı seçip devam ediyorum.

![Crepe](/assets/img/ovirt7-ldap-conf/ov-ld-con01.png)

Ardından **DNS** sunucu kullanacakmısınız diye soruyor, **single server(1)** seçip devam ediyorum. Eğer sizin ortamınızda birden fazla varsa diğer **DNS** sunucularınızı da belirtebilirsiniz.

![Crepe](/assets/img/ovirt7-ldap-conf/ov-ld-con02.png)

**startTLS** ve **Insecure** seçip devam ediyorum.

![Crepe](/assets/img/ovirt7-ldap-conf/ov-ld-con03.png)

**LDAP** bağlantısı için belirlediğim kullanıcı ile beraber search user **DN** bilgilerini **EKSİKSİZ** ve **DOĞRU** bir şekilde girip, **base DN**(fatlan.com gibi)’ide girdikten sonra, **yes** diyerek tekrar devam ediyorum.

![Crepe](/assets/img/ovirt7-ldap-conf/ov-ld-con04.png)

Ardından **GUI** de görünecek **name**‘yi (fatlan.com gibi) girip devam ediyorum.

![Crepe](/assets/img/ovirt7-ldap-conf/ov-ld-con05.png)

İsterseniz burda test için belirlediğiniz user ile test edebilirsiniz.

![Crepe](/assets/img/ovirt7-ldap-conf/ov-ld-con06.png)

Daha sonra **Done** diyerek işlemleri sonlandırıyorum.

![Crepe](/assets/img/ovirt7-ldap-conf/ov-ld-con07.png)

En son olarak **Engine** servisini **restart** etmeniz gerekiyor.

~~~
systemctl restart ovirt-engine.service
~~~

Ardında Engine **GUI** de giriş yapabiliriz. Aşağıdaki gibi kullanıcı yetkili değil diye uyarı veriyor, aslında login olabildiniz fakat daha önceden kullanıcıya herhangi bir role atanmadığı için bu uyarıyı veriyor. **Local admin** ile ilglil kullanıcıya **role** tanımladığınızda **GUI** ona göre açılacaktır.

![Crepe](/assets/img/ovirt7-ldap-conf/ov-ld-con08.png)

