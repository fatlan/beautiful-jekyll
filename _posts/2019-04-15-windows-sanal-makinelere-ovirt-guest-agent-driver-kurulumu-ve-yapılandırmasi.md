---
layout: post
title: Windows Sanal Makinelere oVirt Guest Agent Driver Kurulumu ve Yapılandırması
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [ovirt, windows, virtualization]
comments: true
---
**oVirt** üzerine Windows bir sunucu kurulurken **“Network Interfaces**”, “**e1000**” ve “**Disks**”, “**IDE**” olarak gelmektedir. Aksi bir yapılandırma yaparsanız sunucunuz açılmayacaktır. Bu durum ayrıca makinanın performansınıda etkilemektedir. Bu nedenle “**Network Interfaces**”, “**VirtIO**” ve “**Disks**”, “**VirtIO-SCSI**” yapmalısınız. Böylelikle makineniz düzgün bir performansta çalışacaktır.

Bu yapılandırmayı yapabilmeniz için ise Windows sunucuyu kurduktan sonra **oVirt Guest Agent Driver**(**oVirt-toolsSetup**)’ını kurmalısınız. İlk önce [oVirt-tools-setup.iso](https://resources.ovirt.org/pub/ovirt-4.2/iso/oVirt-toolsSetup/) dosyasının ilgili versiyonunu indirip, makineye **CD Drive** olarak bağladıktan sonra tipik ***.exe** kurulumu gerçekleştirmelisiniz.

Kurulum bittikten sonra,

Network ayarını **e1000**’den **VirtIO**’ya çekin.

![Crepe](/assets/img/win-ovirt-agentinst/winovirt-agen01.png)

**Disk** ayarlarını ise **IDE**’den **VirtIO-SCSI** yapın.

![Crepe](/assets/img/win-ovirt-agentinst/winovirt-agen02.png)
