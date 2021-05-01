---
layout: post
title: Fiziksel HP Gen9 Sunucuda OS Boot Ekleme - Kaybolan OS GRUB Ekleme on Linux
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [hp, linux, gen9, boot]
comments: true
---
HP sunucularda genelde fiziksel board değişiminden sonra işletim sistemi disk üzerinden açılmaz. Bunun nedeni sunucu başlangıç diski olarak **MBR**’ı görmemesidir. Bu işlemi göstereceğimi şekilde işletim sisteminin kurulu olduğu diski manuel olarak göstererek **grub**’ın **boot** olmasını sağlayacağız.

- Sunucu başladıktan sonra F9’a basılarak System Utilities gidilir.
- System Configuration seçilir.

![Crepe](assets/img/hp-gen9-ad-boot/hpgen9-lb01.png)

- **BIOS/Platform Configuration** seçilir.

![Crepe](assets/img/hp-gen9-ad-boot/hpgen9-lb02.png)

- **Boot Options** seçilir.

![Crepe](assets/img/hp-gen9-ad-boot/hpgen9-lb03.png)

- **Advanced UEFI Boot Maintenance** seçilir.

![Crepe](assets/img/hp-gen9-ad-boot/hpgen9-lb04.png)

- **Add Boot Option** seçilir.

![Crepe](assets/img/hp-gen9-ad-boot/hpgen9-lb05.png)

- Bu kısında işletim sisteminin yüklü olduğu disk otomatik olarak gelecektir, seçip devam edelim.

![Crepe](assets/img/hp-gen9-ad-boot/hpgen9-lb06.png)

- **EFI** seçilir.

![Crepe](assets/img/hp-gen9-ad-boot/hpgen9-lb07.png)

- Bizde Linux Ubuntu yüklüydü geldi, seçilip devam edilir.

![Crepe](assets/img/hp-gen9-ad-boot/hpgen9-lb08.png)

- **grub64.efi** seçip devam ediyoruz.

![Crepe](assets/img/hp-gen9-ad-boot/hpgen9-lb09.png)

- Burda yüklü işletim sistminin adını yazmak mantıklı,

![Crepe](assets/img/hp-gen9-ad-boot/hpgen9-lb10.png)

- **Enter** ile menüye girilir.

![Crepe](assets/img/hp-gen9-ad-boot/hpgen9-lb11.png)

- Bizde ubuntu yüklü olduğu için ubuntu yazıp devam ediyoruz.

![Crepe](assets/img/hp-gen9-ad-boot/hpgen9-lb12.png)

- **F10** ile işlem kaydedilir.

![Crepe](assets/img/hp-gen9-ad-boot/hpgen9-lb13.png)

- **Boot Options** seçeneğine tekrar gidilir ve en altta geldiği görülür.

![Crepe](assets/img/hp-gen9-ad-boot/hpgen9-lb14.png)

- Burda **+** ve **–** ile listenin en üstüne ubuntu sekmesi getirilir ki işletim sistemi direk açılsın.

![Crepe](assets/img/hp-gen9-ad-boot/hpgen9-lb15.png)

- **F10**’a basarak yapılandırmalar kaydedilir ve sunucu yeniden başlatılarak, sunucunun diskteki OS’den başlaması sağlanmış olur.
