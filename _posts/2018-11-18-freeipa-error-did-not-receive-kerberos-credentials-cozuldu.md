---
layout: post
title: Freeipa ERROR did not receive Kerberos credentials - Çözüldü
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [freeipa, linux, idm, redhat]
comments: true
---
![Crepe](/assets/img/freeipa-kerb-creden/fi-k-er01.png)

**İpa** komutunu aşağıdaki gibi komut satırı aracılığı ile **DNS A** kaydı girerken yada user ları listelemek isterken aşağıdaki gibi “**ipa: ERROR: did not receive Kerberos credentials**” hatasını alabilirsiniz.

![Crepe](/assets/img/freeipa-kerb-creden/fi-k-er02.png)

Bu hatadan sıyrılabilmek için **kinit** ile **admin user**’a komut satırında login(**GUI** deki login olduğunuz **admin** kullanıcısı ve şifre) olmanız gerekiyor. Komut aşağıdaki gibidir.

~~~
kinit admin
~~~

![Crepe](/assets/img/freeipa-kerb-creden/fi-k-er03.png)

Şimdi komutlarınız sorunsuz çalışacaktır.