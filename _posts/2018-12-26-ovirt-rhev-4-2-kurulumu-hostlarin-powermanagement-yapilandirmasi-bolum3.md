---
layout: post
title: Ovirt/RHEV 4.2 Kurulumu Hostların PowerManagement Yapılandırması - Bölüm 3
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [ovirt, rhev, linux, virtualization]
comments: true
---
Hostları ekleme sırasında son kısımlarda aldığımız power management uyarısını bu bölümde ele alacağız. **Power management** yapılandırması hostlar için **ilo** ekranına ya da **host**’un konsol ekranına düşmeden kapatıp, açma ve yeniden başlatma gibi özellikleri kazandıracak **agent**’ı(**fences agent**) aktif edeceğiz.

Aşağıda görüldüğü üzere hostları listelediğimizde üzerlerinde ünlem(!) işareti var ve **power management**’ın yapılandırılmadığı uyarısını veriyor.

![Crepe](/assets/img/ovirt42-powe-man/ovirt42-pm01.png)

Şimdi hostları tek tek seçip “**Edit**” tıklıyoruz. Ardından “**Enable Power Management**” **check** ettikten sonra, “**Add Fence Agent**” kısmındaki artı işaretine tıklıyoruz.

![Crepe](/assets/img/ovirt42-powe-man/ovirt42-pm02.png)

Daha sonra sunucuların (**ilo**, **idrac**, **BMC** vs.) ip adresi, kullanıcı ve şifre bilgisini yazıyoruz. Ben “**Type**” olarak “**ipmilan**” ile yapılandırıyorum, “**option**” kısmına da “**lanplus=1**” **key value**’sini verip, bağlantıyı “**Test**” ediyorum ve başarılı olduğunu görüyorum. **OK** ile işlemi tamamlıyorum.

![Crepe](/assets/img/ovirt42-powe-man/ovirt42-pm03.png)

Son durum aşağıdaki gibi, **OK** diyerek tamamlıyorum.

![Crepe](/assets/img/ovirt42-powe-man/ovirt42-pm04.png)

İşlem bu kadar. Artık hostları listelediğinizde ünlem işareti olmayacak ve **host** için **power** seçenekleri aktif olacak.