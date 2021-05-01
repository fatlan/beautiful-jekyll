---
layout: post
title: OVİRT(RHEV) 4.2 Kurulum ve Yapılandırması - Host Ekleme - Bölüm 2
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [ovirt, rhev, virtualization, host]
comments: true
---
Makalemizin ikinci bölümüyle beraber artık tüm işlemlerimizi **hosted engine** üzerinden yapacağımızı adım adım göreceğiz. Şimdi sırada ortama **hypervisor** olarak **host** ekleme var. **Host** ekleme işleminin sırasıyla nasıl yapılacağına bakacağız.

İlk olarak **engine login **olun.

![Crepe](/assets/img/ovrt42-host-add/ovirt42-hostad01.png)

Ardından “**Compute - Hosts**” kısmına gelin ve “**New**” butonunu tıklayın.

![Crepe](/assets/img/ovrt42-host-add/ovirt42-hostad02.png)

Daha sonra “**General**” sekmesinde;

- “**Host Cluster**”ı seçin, genelde **Default** olur. Fakat ismini siz daha sonra özelleştirebilirsiniz.
- “**Name**” ve “**Hostname**” kısmına **host** için verdiğiniz** domain name**’yi girin.
- “**SSH Port**” bölümüne **port** numarasını yazın.
- **Authentication** kısmındaki “**Password**” kısmına sunucuya **login** olduğunuz **user**’ın şifre bilgisini yazın.

![Crepe](/assets/img/ovrt42-host-add/ovirt42-hostad03.png)

Son olarak “**Hosted Engine**” sekmesine gelin ve “**Choose hosted engine deployment action**” seçeneğini “**Deploy**” olarak belirledikten sonra “**OK**” tıklayın.

![Crepe](/assets/img/ovrt42-host-add/ovirt42-hostad04.png)

Bu kısımda “**Power Management**” yapılandırılmadığına dair uyarı verecek, “**OK**” diyerek işlemi başlatın daha sonra nasıl yapılandırılacağına bakacağız. Bu arada “**Power Management**” yapılandırması **Engine** üzerinden sunucuyu kapatma, açma ve yeniden başlatma gibi yetenekler kazandırmasına olanak tanıyacak olan bir **agent**’tir(**fences agent**).

![Crepe](/assets/img/ovrt42-host-add/ovirt42-hostad05.png)

Kırmızı alanla işaretlediğimiz kısıma tıkladığınızda “**Deploy**” edildiği sürece tanık olabilirsiniz.

![Crepe](/assets/img/ovrt42-host-add/ovirt42-hostad06.png)

Ve ardından hostları listelediğimizde geldiğini görebiliyoruz. Siz ortamınızdaki tasarımınızda gerekli olan kaynak durumuna göre tüm hostlarınızı aynı yöntemle ortama dahil edebilirsiniz.

![Crepe](/assets/img/ovrt42-host-add/ovirt42-hostad07.png)
