---
layout: post
title: Linux Ubuntu Üzerinden OpenVPN Client-Server Bağlantısı
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [openvpn, ubuntu, linux]
comments: true
---
**Ubuntu** üzerinden **OpenVpn server**’inize bağlantının iki yolundan bahsedeceğim. Biri **CLI** üzerinden bağlantı şekli diğeri **GUI** üzerinden bağlantı. Ben **vpn** server olarak **PfSense** kullanacağım. **PfSense OpenSource firewall** olarak kullanmanın yanı sıra birçok amaç için de kullanabilirsiniz. Ben burada ki işlem için **vpn server** olarak kullanacağım ve yapılandırma dosyalarını ondan elde edeceğim.

İlk önce **vpn** sunucusundan yapılandırma dosyalarını çekiyorum. Aşağıdaki gibi ***.ovpn**, ***.p12**, ***.key**. Bu arada yapılandırma çeşitleri sunucu tarafında farklılık göstermektedir. Yani **ios** için, **android** için, **windows** için ve standart paket olarak ayrılır.

![Crepe](assets/img/u16-openvpn-conf/opnvpn-con01.png)

Daha sonra “**ifconfig**” yada “**ip address**” komutu ile ip tablonuzu listeleyin.

~~~
ifconfig
~~~

Bir de “**route**” yada “**netstat -nr**” yönlendirme tablunuzu listeleyin.

~~~
route
~~~

Ardından bize gerekli olan paketi aşağıdaki komut yardımıyla kurarak devam edelim.

~~~
sudo apt install network-manager-openvpn-gnome
~~~

Paket kurulumu bittikten sonra ilk olarak **CLI** üzerinden aşağıdaki komut aracılığı ile **vpn server**’e bağlantı gerçekleştirelim. Terminali başlatın ve **cd** komutu yardımıyla vpn dosyalarının bulunduğu dizine gidin.

~~~
cd ve ls komutu
~~~

![Crepe](assets/img/u16-openvpn-conf/opnvpn-con02.png)

Aşağıdaki komutu çalıştırın. Sudo şifresini girdikten sonra sonra;

~~~
sudo openvpn --config fw-TCP-1194.ovpn
~~~

Sizden **vpn user** ve **şifre** bilgisini isteyecektir.

![Crepe](assets/img/u16-openvpn-conf/opnvpn-con03.png)

Terminalin en son satırında aşağıdaki çıktıyı görürseniz başarılı bir şekilde bağlantı sağlamışsınızdır demektir.

![Crepe](assets/img/u16-openvpn-conf/opnvpn-con04.png)

Dada sonra ip’nizi ve yönlendirme tablonuzu yukarıdaki komutlar yardımıyla listelediğinizde sağlamasını yapmış olacaksınız.

NOT1 : Vpn bağlantısı gerçekleştikten sonra terminal açık kalması gerekecek, kapatırsanız bağlantı kopacaktır. Bu değişik atraksiyonlar ile çözülebilir fakat en doğru yöntem komuta “**daemon**” parametresini eklemektir.

~~~
sudo openvpn --config fw-TCP-1194.ovpn --daemon
~~~

**NOT2** : Hatta user ve şifre bilgisini girmeden dosyadan okutturarak ta bağlantı sağlayabilirsiniz. Bu işlerinizi kolaylaştırmak için gerekebilir. Bağlantı bilgilerinin olduğu dizinde yer almalı. Aşağıdaki komutla da bunu sağlayabilirisiniz.

~~~
sudo openvpn --config fw-TCP-1194.ovpn --daemon --auth-user-pass VpnBilgiler
~~~

![Crepe](assets/img/u16-openvpn-conf/opnvpn-con05.png)

**Şimgi GUI olarak yapılandırıp, bağlantı sağlayalım;**

Aslında ana yapılandırma dosyamız ***.ovpn**, diğer iki dosya ***.p12** ve ***.key** ise devamı gibi zaten ***.ovpn** içinde o dosyalar gösterilmiştir. Aşağıdaki screenshot’ta detaylıca açıkladım. **GUI** yapılandırmasını da gene ***.ovpn** dosyasının içeriğindeki bilgiler ışığında birebir aynı yaptıktan sonra bağlantı sağlayacağız.

![Crepe](assets/img/u16-openvpn-conf/opnvpn-con06.png)

Wi-Fi yayın işaretine tıklayıp, “**Edit Connections...**” tıklayın.

![Crepe](assets/img/u16-openvpn-conf/opnvpn-con07.png)

Ordan “**Add**” diyoruz.

![Crepe](assets/img/u16-openvpn-conf/opnvpn-con08.png)

Daha sonra “**OpenVPN**” seçeneğini **create** ediyoruz.

![Crepe](assets/img/u16-openvpn-conf/opnvpn-con09.png)

Ardından bilgileri ***.ovpn** dosyasının içeriğindeki bilgilerle aşağıdaki şekilde girdikten sonra “**Advanced**” tıklıyoruz.

![Crepe](assets/img/u16-openvpn-conf/opnvpn-con10.png)

Daha sonra alttaki seçenekleri **check** ediyoruz, aslında çok gerekli değil fakat biz gene de yapalım.

![Crepe](assets/img/u16-openvpn-conf/opnvpn-con11.png)

Sonra **Security** kısmından **Cipher** seçeneğini ***.ovpn** dosyasında yazan şeklinde belirliyoruz.

![Crepe](assets/img/u16-openvpn-conf/opnvpn-con12.png)

**TLS Authentication** kısmını gene ***.ovpn** dosyasındaki bilgiler ışığında belirleyip “**OK**” diyoruz.

![Crepe](assets/img/u16-openvpn-conf/opnvpn-con13.png)

**OK** dedikten sonra aşağıdaki gibi bilgiler kayıt altına alınıyor.

![Crepe](assets/img/u16-openvpn-conf/opnvpn-con14.png)

Şimdi **VPN** bağlantısını gerçekleştirelim. Gene **Wi-Fi** yayın işaretine tıklayıp, ordan **VPN Connections** kısmına ve **vpn** bağlantımıza tıklıyoruz.

![Crepe](assets/img/u16-openvpn-conf/opnvpn-con15.png)

**VPN user**’ın şifresini isteyecektir, alttaki gibi girip **OK** diyoruz ve bağlanmasını bekliyoruz.

![Crepe](assets/img/u16-openvpn-conf/opnvpn-con16.png)

Ve bağlantı başarılı bilgisini aldık.

![Crepe](assets/img/u16-openvpn-conf/opnvpn-con17.png)

Başarılı bağlantı sonrası **Wi-Fi** işaretinin üzerinde kilit şekli oluşur.

![Crepe](assets/img/u16-openvpn-conf/opnvpn-con18.png)

Daha sonra “**ifconfig**” yada “**ip address**” komutu ile ip bilgilerinizi ve bir de “**route**” yada “**netstat -nr**” komutu ile yönlendirme tablomuza bakarak bağlantı doğrulamasını yapabiliriz.
