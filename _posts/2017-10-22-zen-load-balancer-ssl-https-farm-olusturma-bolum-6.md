---
layout: post
title: Zen Load Balancer SSL Sertifika ve HTTPS(443) Request Farm Olusturma - Bölüm 6
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [zen, proxy, loadbalancer, linux]
comments: true
---
**Loadbalancer** arkasında birden fazla **web** sunucusu çalıştığını var sayalım ve bunların **https(443)** protokolü ile çalışmasını sağlayalım. Bunun için yük dengeleyici sunucumuza **web server** domainimiz için satın aldığımız sertifikayı yükleyeceğiz ardından **https farm**’ı oluşturacağız. Zaten önceki yazılarımızda **farm** oluşturmayı görmüştük, şimdi bir kaç yapılandırma ile **https(443)** olarak **farm**’ı yapılandıracağız.

İlk olarak **Manage-Certificates** sekmesine geliyoruz.

![Crepe](/assets/img/zen-ssl-https-bolum6/ze-ssl-https-b601.png)

Ardından aşağıdaki gibi **upload** ***.pem**(pem sertifikanın **bundle** edilmiş hali, yani o dosya içeriğinde domain sertifika cer’i, orta kök cer’i, sertifikanın key’i nin hiyerarşik yapıda bulundurduğu hali) **certificate** kısmını tıklayıp, sektifikayı **load balancer** sunucusuna ekliyoruz.

![Crepe](/assets/img/zen-ssl-https-bolum6/ze-ssl-https-b602.png)

Sertifikayı ekledikten sonra **https farm** oluşturma kısmına geçiyoruz. **Manage-Farm** sekmesine geçtikten sonra **Add new Farm** butonuna tıklıyoruz. **Farm**’a isim verip **Save** ve **continue**’yi tıklayıp devam ediyoruz.

![Crepe](/assets/img/zen-ssl-https-bolum6/ze-ssl-https-b603.png)

Akabinde burada önemli kısımlar aşağıdaki gibi **Farm-Listener HTTPS** seçilecek, **HTTPS Certificat**e kısmında yüklediğiniz sertifikayı seçin, **Virtual IP** kısmında İsteği alacak **ip** ve dinleyeceği **port** bilgisini **https(443)** girin.

![Crepe](/assets/img/zen-ssl-https-bolum6/ze-ssl-https-b604.png)

Alt kısımda bir önemli bölüm daha olan **HTTPS Backends** seçeneğini aktif edip, **farm** oluşturmayı tamamlıyoruz.

![Crepe](/assets/img/zen-ssl-https-bolum6/ze-ssl-https-b605.png)
