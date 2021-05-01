---
layout: post
title: Zen Load Balancer Kurulumu - Bölüm 1
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [zen, network, loadbalancer, proxy, linux]
comments: true
---
**Load Balancer** nedir = Kısaca işi, iki ya da daha fazla bilgisayar, işlemci, sabit disk ya da diğer kaynaklar arasında paylaştırma teknolojisidir. Daha fazla bilgi için [linki](https://tr.wikipedia.org/wiki/Y%C3%BCk_dengeleme) ziyaret edebilirsiniz.

**Zen Load Balancer Debian** tabanlı bir uygulamadır. Ayrıntılar için resmi sitesini ziyaret edebilirsiniz. [http://www.zenloadbalancer.com/](http://www.zenloadbalancer.com/)

Ben aşağıdaki screenshot’tan da anlaşılacağı üzere sanal platformda çalıştıracağım için aşağıdaki gibi bir konfigürasyonda makine oluşturdum, iç ve dış interface(**E1000** daha sonra **VMXNET**’e çekeceğim) olmak üzere ve **iso**’yu **mount** ettikten sonra kuruluma başlayabiliriz.

![Crepe](assets/img/zen-kurulum-bolum1/zen-kur-bo101.png)

Kuruluma **Install** ile başlıyorum.

![Crepe](assets/img/zen-kurulum-bolum1/zen-kur-bo102.png)

**Dil** seçiyorum.

![Crepe](assets/img/zen-kurulum-bolum1/zen-kur-bo103.png)

**Lokasyon** bilgisini seçiyorum, dil **EN** lokasyon **TR** seçerseniz aşağıdaki gibi ikinci bir menü daha karşınıza getirecektir. Dile göre lokasyon bilgisi yok gibisinden, **default** değerlerden tekrar seçtirecektir.

![Crepe](assets/img/zen-kurulum-bolum1/zen-kur-bo104.png)

![Crepe](assets/img/zen-kurulum-bolum1/zen-kur-bo105.png)

**Klavye** seçim menüsü ile devam ediyoruz,

![Crepe](assets/img/zen-kurulum-bolum1/zen-kur-bo106.png)

**Network** değerlerinin girildiği bölüm ile **IP** bilgisini yazdıktan sonra **Continue** ile devam ediyoruz.

![Crepe](assets/img/zen-kurulum-bolum1/zen-kur-bo107.png)

**Netmask**,

![Crepe](assets/img/zen-kurulum-bolum1/zen-kur-bo108.png)

**Gateway**,

![Crepe](assets/img/zen-kurulum-bolum1/zen-kur-bo109.png)

**DNS** kısmı,

![Crepe](assets/img/zen-kurulum-bolum1/zen-kur-bo1010.png)

**Hostname** makineye verilecek isim kısmı ile **Continue** ile devam ediyoruz.

![Crepe](assets/img/zen-kurulum-bolum1/zen-kur-bo1011.png)

Varsa **Domain name** kısmı,

![Crepe](assets/img/zen-kurulum-bolum1/zen-kur-bo1012.png)

**Root** için **password** bilgisinin girildiği önemli bir menü, belirlediğiniz şifreyi girdikten sonra **Continue** ile devam ediyoruz.

![Crepe](assets/img/zen-kurulum-bolum1/zen-kur-bo1013.png)

**Şifreyi tekrar girerek** teyit işlemini yapıyoruz,

![Crepe](assets/img/zen-kurulum-bolum1/zen-kur-bo1014.png)

**Disk** yapılandırma menüleri gelecek, ben olduğu gibi **default** değerlerle ile **LVM** olarak yapılandırıyorum. Siz istediğiniz gibi **partition**‘lara bölerek **EXT** olarakta kurabilirsiniz.

![Crepe](assets/img/zen-kurulum-bolum1/zen-kur-bo1015.png)

![Crepe](assets/img/zen-kurulum-bolum1/zen-kur-bo1016.png)

![Crepe](assets/img/zen-kurulum-bolum1/zen-kur-bo1017.png)

**LVM** işlemleri diske yazdırmak için **Yes** ile devam ediyoruz.

![Crepe](assets/img/zen-kurulum-bolum1/zen-kur-bo1018.png)

**Finish** ile **disk** işlemleri sonlandırıp,

![Crepe](assets/img/zen-kurulum-bolum1/zen-kur-bo1019.png)

Yapılan tüm değişiklikleri diske **Yes** ile yazdırıyoruz.

![Crepe](assets/img/zen-kurulum-bolum1/zen-kur-bo1020.png)

Ve kurulum başladı.

![Crepe](assets/img/zen-kurulum-bolum1/zen-kur-bo1021.png)

Ardından **Continue** ile kurulumu tamamlayıp sunucuyu **restart** ediyoruz.

![Crepe](assets/img/zen-kurulum-bolum1/zen-kur-bo1022.png)

Ve **konsol** login ekranı...

![Crepe](assets/img/zen-kurulum-bolum1/zen-kur-bo1023.png)

Daha sonra **Zen load Balancer panal**e bağlanmak için;

**https://YönetimIPAdresi:444/**

**Default**ta gelen KullanıcıAdı=**admin** Şifre=**admin**

![Crepe](assets/img/zen-kurulum-bolum1/zen-kur-bo1024.png)

Daha sonra Yönetim **IP**’sinin dışında, **Farm** için **request**’leri alacak dış ip ve iç **interface**’yi yapılandırıp, **farm** ve **cluster** mevzularına bakacağız. **Vmtool** ve **İnterface Adapter(VMXNET)** olaylarına inceleyeceğiz.

[http://www.zenloadbalancer.com/community/documentation/](http://www.zenloadbalancer.com/community/documentation/)

[http://www.zenloadbalancer.com/zlb-administration-guide-v304/#prettyPhoto](http://www.zenloadbalancer.com/zlb-administration-guide-v304/#prettyPhoto)
