---
layout: post
title: Mikrotik Nedir Kurulumu ve Temel Ayarlamaları
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [mikrotik, linux, network, router, firewall]
comments: true
---
![Crepe](assets/img/mikrotik-ku-kon/miko-kk01.png)

**Mikrotik** **Linux** çekirdeği üzerinde geliştirilen, **lisans** gerektiren bir **RouterOs** (Router İşletim Sistemi) dur. Bu işletim sistemi ile pc tabanlı bilgisayarınızı **modern** bir yönlendirici'ye (**router**), bir **vpn** sunucusuna veya yüksek özellikleri olan bir ateş duvarına (**firewall**) çevirebilirsiniz. Ülkemizde bu ürün genelde **web hosting** firmaları, orta ölçekli işletmeler ve trafiği az olan ağlarda tercih ediliyor.

**Mikrotik**’i yazılımsal ya da donamınsal olarak kurup kullanabilirsiniz. Ben işlemleri sistemi kurduğum vm makine üzerinden ve ayrıca hem kod hem görsel grafikler aracılığı ile göstereceğim. Görsel arayüze ulaşmak için **Mikrotik Winbox** uygulamasını indirmelisiniz. Onun aracılığı ile cihaza ulaşıp gerekli kon gürasyonu yapabilirsiniz. Donanımsal olarak elinizde buluna cihazın elektrik prizini takıp, lan kablosunu pc’nize bağladıktan sonra aşağıdaki screenshot’tanda anlaşılacağı üzere **Winbox** uygulamasını çalıştırıyoruz ve ardından sarı şerit olan yere cihazın bilgileri düşüyor, tıklayıp login olacağız. **Default**’ta gelen kullanıcı adı **Admin**‘dir ve ayrıca şifresi yoktur.

![Crepe](assets/img/mikrotik-ku-kon/miko-kk02.png)

Tabi uyarı gelecektir. Sistem ayarlarını resetlensin mi.? gibisinden resetlemenizi tavsiye ederim. Yazılımsal olarak’ta aşağıdaki kod yardımıyla **reset**’leme işlemini gerçekleştirebilirsiniz.

~~~
system reset-configuration
~~~

![Crepe](assets/img/mikrotik-ku-kon/miko-kk03.png)

Ardından onaylamanız için **y/n** uyarısı gelecektir, **y(yes)** bastıktan sonra sistem ayarlarını **reset**’leyip sistemi yeniden başlatacaktır. Açıldıktan ve **login** olduktan sonra aşağıdaki bir uyarı alacaksınız. **Default**ta gelen ayarları görmek için “**v**” silmek için ise “**r**” tuşuna basmanızı bekleyecektir. Biz yeni ve düzgün bir kon gürasyon yapmak için “**r**” tuşuna basıp siliyoruz.

![Crepe](assets/img/mikrotik-ku-kon/miko-kk04.png)

### NETWORK AYARLARI (IP, ROUTE VE GATEWAY)

Herşeyden evvel **interface**’leri görüntüleyelim, kaç interface var ve **public**(dış bacak), **private**(iç bacak) olduğunu anlayalım. Bunun için aşağıdaki komutu çalıştırabilirsiniz.

~~~
interface print
~~~

![Crepe](assets/img/mikrotik-ku-kon/miko-kk05.png)

Garafiksel olarak aşağıdaki gibidir.

![Crepe](assets/img/mikrotik-ku-kon/miko-kk06.png)

Bende, dış bacak yani dünyaya açılan interface “**ether2**” iç bacak yani local **ağ**’ım “**ether1**”dir. Tüm işlemleri buna göre yapacağım.

Hemen **Ip** yapılandırmasına dış bacak ile başlayalım, bunun için aşağıdaki komutu kullanabilirsiniz.

~~~
ip address add dışbacakipsi/subnet interface=ether2
~~~

![Crepe](assets/img/mikrotik-ku-kon/miko-kk07.png)

Görsel olarak aşağıdaki gibi.

![Crepe](assets/img/mikrotik-ku-kon/miko-kk08.png)

Şimdi iç network için iç bacak ip’sini yapılandıralım.

~~~
ip address add içbacakipsi/subnet interface=ether1
~~~

![Crepe](assets/img/mikrotik-ku-kon/miko-kk09.png)

Ardından internete ulaşabilmek için Yani **0.0.0.0/0** (tanımsız tüm network) **route** yani **gateway** eklememiz gereklidir. Bunun için aşağıdaki komutu çalıştırabilirsiniz.

~~~
ip route add gateway=gatewayipsi
~~~

![Crepe](assets/img/mikrotik-ku-kon/miko-kk10.png)

Görsel olarak aşağıdaki gibi.

![Crepe](assets/img/mikrotik-ku-kon/miko-kk11.png)

![Crepe](assets/img/mikrotik-ku-kon/miko-kk12.png)

Ardından **DNS** adres tanımlamasını yapmalısınız ki **domain**lere **ping** atabilesiniz.

![Crepe](assets/img/mikrotik-ku-kon/miko-kk13.png)

Şimdi işlemlerimiz doğrulama niteliğinde bir kaç işlem yapalım. İlk önce ip adreslerimiz doğru **interface**’lere atandığını anlamak için aşağıdaki kodu çalıştıralım.

~~~
ip address print
~~~

![Crepe](assets/img/mikrotik-ku-kon/miko-kk14.png)

Eğer yanlış olduğunu düşündüğünüz bir ayar varsa aşıdaki komut ile silme işlemini gerçekleştirebilirsiniz.

~~~
ip address remove numbers=(# işaretinin altındaki sıralama numarası yazılacaktır)
~~~

![Crepe](assets/img/mikrotik-ku-kon/miko-kk15.png)

Ardından **Route**(Gateway) tablosuna erişmek için aşağıdaki kodu çalıştırın.

~~~
ip route print
~~~

![Crepe](assets/img/mikrotik-ku-kon/miko-kk16.png)

Şimdi buraya kadar önemli olan **ip**, **gateway**, **dns** atama gibi işlemleri yaptık. Son olarak önemli ve püf nokta olan bir işlem daha kaldı. O işlem ise local ağımın, yerel ip’lerin yani iç interface’nin, dış interface üzerinden internete ve dış dünyaya erimesi için(evinizdeki adsl modem gibi) **NAT** işlemidir. **Mikrotik**’te bu işleme **Masquerade** denir. Kısaca **Nat**’ı açıklamak gerekirse **Network Address Translation**(Ağ Adresi Dönüştürme) demektir. Yani iç bacak interface ve o ağdaki ip’lerin internete çıkarken dış bacak interface’deki ip adresine dönüşüp nete çıkması diyebiliriz. Daha geniş bilgiye [viki](https://tr.wikipedia.org/wiki/NAT) adresinden ulaşabilirsiniz.

Bu işlemi yapabilmek için aşağıdaki kodu çalıştırmanız yeterlidir.

~~~
ip firewall nat add chain=srcnat src-address=YOUR NETWORK ADDRESS/MASK action=masquerade comment=”” \disabled=no
~~~

Görsel olarak,

![Crepe](assets/img/mikrotik-ku-kon/miko-kk17.png)

![Crepe](assets/img/mikrotik-ku-kon/miko-kk18.png)

### ŞİFRE BELİRLEME, HOSTNAME ATAMA VE YARARLI KOMUTLAR

**Admin** kullanıcısına şifre belirlemek için,

~~~
/password
~~~

![Crepe](assets/img/mikrotik-ku-kon/miko-kk19.png)

**Hostname** atamak için,

~~~
system identity set name=Hostname
~~~

![Crepe](assets/img/mikrotik-ku-kon/miko-kk20.png)

Sistemi yeniden başlatmak için,

~~~
system reboot
~~~

![Crepe](assets/img/mikrotik-ku-kon/miko-kk21.png)

Sistemi kapatmak için,

~~~
system shutdown
~~~

![Crepe](assets/img/mikrotik-ku-kon/miko-kk22.png)

### BANDWİDTH SINIRI BELİRLEME

**Mikrotik** üzerinde kullanıcılar için **bandwidth** sınırlaması yapabilirsiniz. Aşağıdaki kullanıcının **2mb** olarak sınırlandırılmış örneğini görebilirsiniz.

![Crepe](assets/img/mikrotik-ku-kon/miko-kk23.png)

**Ayrıca Mikrotik’i İp atama işinden sonra web(80) üzerinden de yönetebilirisiniz.**

![Crepe](assets/img/mikrotik-ku-kon/miko-kk24.png)
