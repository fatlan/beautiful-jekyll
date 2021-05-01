---
layout: post
title: FreeIPA Komut Satırı Üzerinden DNS A kaydı Oluşturma
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [freeipa, ipa, centos, redhat, ldap, dns]
comments: true
---
![Crepe](/assets/img/fipa-cli-adns/freipa-cliadns01.png)

Şimdi **DNS**’e **A** kaydı gireceğiz. Bu işlemi **GUI**’de basit bir şekilde de yapabilirsiniz ki ilerde bir makale ile bu konuya da değinebiliriz, şimdi ise aşağıdaki komut yardımıyla bu işlemi komut satırıcı üzerinden gerçekleştirebiliriz.
İlk önce **kinit admin**e login olun(**GUI** ekranında login olduğunuz **admin** ve **şifresi**).

~~~
kinit admin
~~~

Ardından aşağıdaki komutu çalıştırın.

~~~
**ipa dnsrecord-add [domain name] [record name] [record type] [ip adress]**

ipa dnsrecord-add fatlan.local dc02 --a-rec 192.168.2.160
~~~