---
layout: post
title: Zen Load Balancer Farm Oluşturma - Bölüm 4
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [zen, loadbalancer, proxy, linux, network]
comments: true
---
Zen yönetim paneline herhangi bir tarayıcıdan **login** olduktan sonra **Manage-Farms** seçeneğini tıklayın.

![Crepe](assets/img/zen-farm-bolum4/zen-far-bo401.png)

**Farm** ismini belirledikten sonra **Save & continue** tıklayın.

![Crepe](assets/img/zen-farm-bolum4/zen-far-bo402.png)

Ardından **request** alacak **interface**, **ip** ve **port** bilgilerini yazdıktan sonra **Save** tıklayın.

![Crepe](assets/img/zen-farm-bolum4/zen-far-bo403.png)

Ardından aşağıdaki gibi **Edit the http Farm** seçeneği ile **farm** yapılandırması başlayacağız.

![Crepe](assets/img/zen-farm-bolum4/zen-far-bo404.png)

**Farm**ın özet bilgisi aşağıdaki gibidir.

![Crepe](assets/img/zen-farm-bolum4/zen-far-bo405.png)

Hemen altında işte bu kısımda **Add Real Server** diyerek arka planda gidecek(çalışacak) sunucuların bilgilerini giriyoruz.

![Crepe](assets/img/zen-farm-bolum4/zen-far-bo406.png)

Birinci sunucu bilgilerini girip **Save Real Server** diyerek işlemi kaydediyoruz.

![Crepe](assets/img/zen-farm-bolum4/zen-far-bo407.png)

İkinci sunucu bilgilerini de girip **Save Real Server** diyerek işlemi kaydediyoruz. Ve görüntü aşağıdaki gibidir. Ben arkada çalışacak iki sunucu belirledim. Biri **IIS 1** diğeri **IIS 2**

![Crepe](assets/img/zen-farm-bolum4/zen-far-bo408.png)

**Zen** tarafında işlemleri tamamladık şimdi tarayıcıdan **farm** için belirlediğimiz ip yazarak işlemleri kontrol edelim. Arka tarafta istekleri **IIS** gönderiyor mu.? ve hangi **IIS** yönlendirdiğini görelim.

![Crepe](assets/img/zen-farm-bolum4/zen-far-bo409.png)
