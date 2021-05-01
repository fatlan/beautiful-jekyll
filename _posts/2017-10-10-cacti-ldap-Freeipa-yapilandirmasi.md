---
layout: post
title: Cacti LDAP(FreeIPA) Yapılandırması
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [cacti, network, linux, ldap]
comments: true
---
![Crepe](assets/img/cacti-ldap/cacti-ldap01.png)

**Cacti**’yi opensource **LDAP** ortamına dahil etmek için **internal admin user**‘ı ile login olduktan sonra **Console/Configuration/Settings/Authentication** sekmesine gidip aşağıdaki görseldeki gibi yapılandırıyoruz. Yani **LDAP** server **IP** bilgisi yada **Domain name** ile **port** bilgileri giriyoruz.

![Crepe](assets/img/cacti-ldap/cacti-ldap02.png)

Ardından **Group Distingished Name (DN)**, **Search Base**, **Search Filter**,** Search Distingished Name (DN)**, **Search Password** bilgilerini doğru girdiğimizden emin olup **Save** butonuna basıyoruz.

![Crepe](assets/img/cacti-ldap/cacti-ldap03.png)

**Artık Cacti’ye LDAP user’ınızla login olabilirsiniz.**