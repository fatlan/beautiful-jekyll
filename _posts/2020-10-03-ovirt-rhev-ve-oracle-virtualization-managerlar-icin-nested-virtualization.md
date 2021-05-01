---
layout: post
title: Ovirt RHEV ve Oracle Virtualization Manager’lar için Nested Virtualization(iç içe sanallaştırma) Yapılandırması
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [ovirt, centos, redhat, virtualization, rhev]
comments: true
---
![Crepe](/assets/img/ovirt-nested-conf/ov-nest-conf01.png)

Esasında **Open Virtualization Manager(oVirt)**, **Redhat** ya da **Oracle Virtualization Manager** gibi **hypervisor** ürünleri iç içe sanallaştırma yöntemini pek önermediğinden varsayılanda bu yapı kapalı gelmektedir.

Fakat bizim özellikle **KVM** ve benzeri hypervisorleri gerektiren **Docker Swarm**, **Kubernetes(MiniKube)**, **OKD(MiniShift)**, **Openstack**, **Proxmox** ve benzeri ürünleri hem yetersiz kaynak durumundan hem de gereksiz kaynak tüketmeden hızlıca bir takım testler yapabilmek için **Nested VM** özelliğini aktif ederek utilization’dan kar elde edebiliriz.

İlk olarak cluster ortamımıza dahil edilen tüm host’larda aşağıdaki gibi “**Hostdev Passthrough**” ve “**Nested Virtualization**” onay kutusunu işaretliyoruz.

**Compute** – **Host** menüsünden Edit Host yapıp, Kernel tabından işaretlermeleri yapabiliriz.

![Crepe](/assets/img/ovirt-nested-conf/ov-nest-conf02.png)

Son olarak **Compute** – **Virtual Machines** menüsünden ilgili VM için **Edit Virtual Machine** yapıp, **Host** tabından “**Specific Host(s)**” seçeneğini aktif edip, “**Pass-Through Host CPU**” onay kutusunu işaretleyip bitiriyoruz.

![Crepe](/assets/img/ovirt-nested-conf/ov-nest-conf03.png)


Referans: [https://community.oracle.com/docs/DOC-1038581](https://community.oracle.com/docs/DOC-1038581) <br>