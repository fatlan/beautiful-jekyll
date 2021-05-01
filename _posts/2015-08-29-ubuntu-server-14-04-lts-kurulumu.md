---
layout: post
title: Ubuntu Server 14.04 LTS kurulumu
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [ubuntu, linux]
comments: true
---
![Crepe](assets/img/ubun14-inst/ubun14-ins01.png)

**LTS nedir**? : Bu sürüm için Nisan 2019’a kadar openstack ve Icehouse destek sürümü de dahil olmak üzere uzun dönem Ubuntu Server destek versiyonudur.
Herhangi bir kurulum aygıtı yada *.iso dosyasını boot ettikten sonra kurulum dilini seçin.

![Crepe](assets/img/ubun14-inst/ubun14-ins02.png)

Daha sonra **Install Ubuntu Server** sekmesini tıklayın.

![Crepe](assets/img/ubun14-inst/ubun14-ins03.png)

Ardından yükleme dilini seçin.

![Crepe](assets/img/ubun14-inst/ubun14-ins04.png)

Daha sonra lokasyon seçiminizi yapın. Türkiye bu sürümde Asia kıtasında yer alıyor.

![Crepe](assets/img/ubun14-inst/ubun14-ins05.png)

Lokasyon seçimini Türkiye yaparsanız dil ve lokasyon kombinasyon bilgileri eşleşmediği için kullanabilir olmadığına dair, seçtiğimiz dil için varsayılan yerel ayarların kullanılabilir olması için ülke seçimi menüsü tekrar açılacaktır.
Buradan seçimi yapıp devam edin.

![Crepe](assets/img/ubun14-inst/ubun14-ins06.png)

Daha sonra **klavye**‘yi belirlemek istediğimizi soran menü açılacak.

![Crepe](assets/img/ubun14-inst/ubun14-ins07.png)

Klavye seçimini yapıp **Enter**’ı tuşlayın. Ben **Turkish** seçeneğini seçiyorum.

![Crepe](assets/img/ubun14-inst/ubun14-ins08.png)

Ardından türkçe düzenini belirleyelim.

![Crepe](assets/img/ubun14-inst/ubun14-ins09.png)

Ardından eklenen bileşenler yükleme ekranı açılacak.

![Crepe](assets/img/ubun14-inst/ubun14-ins10.png)

**Network** ayarları için otomatik yükleme ekranı açılacak. Ortamımızda **DHCP** sunucusu olmadığı için hata verecek.

![Crepe](assets/img/ubun14-inst/ubun14-ins11.png)

**Continue** deyip devam edin.

![Crepe](assets/img/ubun14-inst/ubun14-ins12.png)

Daha sonra **network** ayarlarını yapabileceğimiz seçenekler açılacak, ben **manuel** ayar seçmesini seçiyorum.

![Crepe](assets/img/ubun14-inst/ubun14-ins13.png)

İp bilgisini gireceğimiz ekran açılacak, bilgileri girdikten sonra **Continue** deyip devam edin.

![Crepe](assets/img/ubun14-inst/ubun14-ins14.png)

Ardından **Netmask** bilgisi giriş ekranı gelecek, bilgileri girdikten sonra tekrar **Continue** deyip devam edin.

![Crepe](assets/img/ubun14-inst/ubun14-ins15.png)

Daha sonra **Gateway** bilgisi ekranı, **Continue** deyip devam edin.

![Crepe](assets/img/ubun14-inst/ubun14-ins16.png)

Daha sonra **DNS** adresi giriş ekranı açılacak, bilgileri girdikten sonra **Continue** deyip devam edin.

![Crepe](assets/img/ubun14-inst/ubun14-ins17.png)

**Hostaname** bilgisi giriş ekranı, bilgileri girdikten sonra **Continue** ile devam edin.

![Crepe](assets/img/ubun14-inst/ubun14-ins18.png)

Makina **Domaine join** olacaksa bu ekranda bilgileri yazabilirsiniz, **Continue** ile devam edin.

![Crepe](assets/img/ubun14-inst/ubun14-ins19.png)

Daha sonra yeni kullanıcı oluşturma ekranı açılacak, kullanıcı **Full name** girdikten sonra **Continue** ile devam edin.

![Crepe](assets/img/ubun14-inst/ubun14-ins20.png)

**Username** girişinden sonra devam edin.

![Crepe](assets/img/ubun14-inst/ubun14-ins21.png)

Şifre belirleyip devam edin.

![Crepe](assets/img/ubun14-inst/ubun14-ins22.png)

Şifreyi tekrar girip devam edin.

![Crepe](assets/img/ubun14-inst/ubun14-ins23.png)

**Home** klasörünü **Encrypt** edebileceğiniz menü açılacaktır. Ben **No** deyip devam ediyorum.

![Crepe](assets/img/ubun14-inst/ubun14-ins24.png)

Zaman ayar ekranı açılacaktır. **Yes** ile devam edin.

![Crepe](assets/img/ubun14-inst/ubun14-ins25.png)

**Disk bölümleme** ekranı açılacak, **manuel** yapabilirsiniz ben ikinci seçenek ile devam ediyorum.

![Crepe](assets/img/ubun14-inst/ubun14-ins26.png)

Sanal ortamda yaptığım için aşağıdaki gibi bir uyarı alıyorum, onaylayıp devam ediyorum.

![Crepe](assets/img/ubun14-inst/ubun14-ins27.png)

Değişiklikleri **Yes** diyerek diske yazdırıyoruz.

![Crepe](assets/img/ubun14-inst/ubun14-ins28.png)

Tüm alanı **Continue** ile **tek partition** yapıyorum.

![Crepe](assets/img/ubun14-inst/ubun14-ins29.png)

Tüm değişiklikleri **Yes** diyerek onaylıyorum.

![Crepe](assets/img/ubun14-inst/ubun14-ins30.png)

Yükleme ekranı,

![Crepe](assets/img/ubun14-inst/ubun14-ins31.png)

Ortamda Proxy server’ınız varsa burada girebilirsiniz.

![Crepe](assets/img/ubun14-inst/ubun14-ins32.png)

Sistem update’leri ayar menüsü, ben ilk seçenek ile devam ediyorum.

![Crepe](assets/img/ubun14-inst/ubun14-ins33.png)

Daha sonra boşluk tuşu ile **OpenSSH** aktif edip devam edin. Diğer seçenekleri kullanım amacına göre seçebilirsiniz.

![Crepe](assets/img/ubun14-inst/ubun14-ins34.png)

Yükleme ekranı,

![Crepe](assets/img/ubun14-inst/ubun14-ins35.png)

Disk **GRUB Boot loader** yükleme ekranı, **Yes** ile devam edin.

![Crepe](assets/img/ubun14-inst/ubun14-ins36.png)

Yüklemeyi bitirmek için **Continue** deyip çıkın ve sistem yeniden başlayacaktır.

![Crepe](assets/img/ubun14-inst/ubun14-ins37.png)

Daha sonra sistem açıldığında kullanıcı ve şifre bilgilerini girerek sisteme login olabilirsiniz.

![Crepe](assets/img/ubun14-inst/ubun14-ins38.png)
