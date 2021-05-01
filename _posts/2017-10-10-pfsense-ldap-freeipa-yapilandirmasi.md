---
layout: post
title: Pfsense LDAP(FreeIPA) Yapılandırması
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [pfsense, freeipa, ldap, firewall]
comments: true
---
**System/User Manager/Authentication Servers** menüsünden aşağıdaki gibi **Add** butonuna basıyoruz.

![Crepe](/assets/img/pfse-ldp-confi/pf-ldpconf01.png)

Ardından **Descriptive name**, **Type**, **Hostname** or **IP Address**, **Port value**, **Base DN**, bilgileri aşağıdaki screenshottaki gibi yapılandırın.

![Crepe](/assets/img/pfse-ldp-confi/pf-ldpconf02.png)

Daha sonra sayfanın alt kısmında kalan **uid**, **cn**, **dc**, **bind user** ve şifre kısmını yine aşağıdaki gibi yapılandırıp **Save** tıklayın.

![Crepe](/assets/img/pfse-ldp-confi/pf-ldpconf03.png)

Artık **Pfsense** ile başta **vpn** olmak üzere **Authentication** için kullanabileceğiniz bir **Authentication Server**’ınız oldu.
