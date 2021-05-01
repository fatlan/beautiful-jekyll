---
layout: post
title: Ovirt/RHEV 4.2 Kurulumu Hostlara NFS Iso/Export Ekleme - Bölüm 5
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [ovirt, rhev, nfs, linux, centos, redhat]
comments: true
---

**Ovirt/RHEV** yapısında **ISO** ve **EXPORT** olarak **NFS** alan istemektedir.

**ISO** alanı sanal makinaları kurmak için kullanılacak olan ***.iso** dosyalarının barındırılacağı alandır.

**EXPORT** alanı ise sanal makineleri yedekleme, taşıma, kopyalama ya da dışarı aktarma amaçlı kullanılan alandır.

Konuyu daha fazla uzatmadan nasıl yapacağımıza bakalım.

Herşeyden evvel şunu belirtmem gerekir ki **NFS** alanlarını eklemeden önce **Storage**’den açtığınız **NFS** alanlara yetkilendirmeyi doğru bir şekilde yapmalısınız, aksi takdirde **NFS** alanını eklerken “**permission denied**” uyarısını alırsınız. Örnek yetkilendirmeyi aşağıdaki ss’de gösterdim.

![Crepe](/assets/img/ovirt42-nfs-conf/ovirt42-nfs-con01.png)

Şimdi ilk olarak “**Storage/Domains**” sekmesinden “**New Domain**” butonuna tıklayın.

![Crepe](/assets/img/ovirt42-nfs-conf/ovirt42-nfs-con02.png)

Ardından “**Domain Function**”ı “**ISO**” olarak, “**Storage Type**”ı “**NFS**” olarak aşağıdaki gibi seçip yolu “**ip_adresi/dizin_yolu**” şeklinde girip “**OK**” butonuna basın. İşlem bu kadar.

![Crepe](/assets/img/ovirt42-nfs-conf/ovirt42-nfs-con03.png)

**EXPORT** alanını da aynı şekilde belirtip “**OK**” tıklayın. Bu kısımda da işlem tamam.

![Crepe](/assets/img/ovirt42-nfs-conf/ovirt42-nfs-con04.png)

**NoT** : Storage alanlarını sıfırdan ekler iseniz “**New Domain**” butonunu kullanmalısınız. Fakat daha önce eklemiş ve sistemden kaldırmış yada başka bir uygulama tarafından daha önce kullanılmış ise “**Import Domain**” sekmesini kullanmalısınız. Aksi takdirde hata mesajı alıp, alanı ekleyemeyeceksiniz.