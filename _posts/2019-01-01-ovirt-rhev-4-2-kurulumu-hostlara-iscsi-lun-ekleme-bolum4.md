---
layout: post
title: Ovirt/RHEV 4.2 Kurulumu Hostlara ISCSI LUN Ekleme - Bölüm 4
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [ovirt, rhev, iscsi, lun, virtualization]
comments: true
---

**Engine login** olduktan sonra “**Storage**” sekmesinden “**Domains**” bölümüne gidin ve “**New Domain**” butonuna tıklayın.

**NoT** : Bu kısımda bizim oluşturduğumuz **iscsi lun**’lar yeni oluşturulmuş olup, daha önce herhangi bir uygulama tarafından kullanılmamıştır. Biz bu lab ortamı için sıfır **lun** oluşturduk ve bu yüzden “**New Domain**” butonunu kullanarak lun ekleme işlemini yapacağız.

Fakat bu **lun** daha önce herhangi bir uygulama tarafından kullanılmış olsaydı ya da bizim bu **Ovirt** uygulamamız tarafından da kullanılmış olup herhangi bir nedenle **lun** silinmiş olsaydı “**Import Domain**” kısmını kullanarak ekleme işlemini yapacaktık. Çünkü **lun** daha önce kullanıldığı içinde veri barındırmasa dahi daha önce kullanıldığına dair bilgi saklıyorsa ya da sakladığından **Ovirt** daha önce kullanıldığına dair bilgi verecek yada **db** den sildim gibi uyarılar verip **lun**’u eklemeyecektir. Bu nedenle bu tarz durumlarda “**Import Domain**” kullanılmalıdır, eğer **lun** sıfır değil ise.

![Crepe](assets/img/ovirt42-iscsi-lun-add/ov42-iscsi-add01.png)

Ardından eklemek istediğiniz **lun**’lara, bizde bir tane kaldı, “**Name**” kısmını belirttikten sonra “**Add**” diyerek aktif edin. Daha sonra “**OK**” diyerek ekleme işlemini tamamlayın.

![Crepe](assets/img/ovirt42-iscsi-lun-add/ov42-iscsi-add02.png)

Ve “**Storage**” bölümünden **lun**’larımızı görüntülediğimizde, geldiğini görebiliyoruz.

![Crepe](assets/img/ovirt42-iscsi-lun-add/ov42-iscsi-add03.png)
