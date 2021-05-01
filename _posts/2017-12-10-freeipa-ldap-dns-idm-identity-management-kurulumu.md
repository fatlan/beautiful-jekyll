---
layout: post
title: FreeIPA(LDAP, DNS) - iDM(identity management) Kurulumu
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [freeipa, idm, dns, ldap, redhat, ipa, linux]
comments: true
---
**FreeIPA** **Linux/Unix** network ortamları için **iDM(identity management)** kimlik, **kimlik doğrulama**, **DNS** gibi servisleri barındıran bir merkez yönetim uygulamasıdır. Barındırdığı tüm servisleri management olarak **web gui** üzerinden basitçe yönetimini sağlar. **Redhat iDM Ipa Server**'ın **opensource** halidir diyebilirim. Barındırdığı servisler aşağıdaki gibidir.

- 389 Directory Server
- MIT Kerberos
- NTP
- DNS
- Dogtag (Certificate System)

Kuruluma geçmeden önce aşağıdaki komutu çalıştırın.

~~~
yum update
~~~

Daha sonra “**/etc/hosts**” dosyasının içine gerekli bilgileri bir editör aracılığı ile yada aşağıdaki gibi benim girdiğim şekilde girin.

![Crepe](assets/img/free-ipa4-iac/freeipa4-inc01.png)

Daha sonra “**/etc/hostname**” kaynak dosyası içinden **hostname**'inizi değiştirin. “**hostname -f**” komutu ile doğruluğunu kontol edebilirsiniz. Tekrar **logout/login** olduğunuzda TCentOS’u dc01 olarak göreceksiniz.

![Crepe](assets/img/free-ipa4-iac/freeipa4-inc02.png)

Şimdi ipa server paketlerini aşağıdaki komut ile yükleyelim. Komut bittikten sonra neler yaptığını rapor olarak ekrana basacaktır(Installed, Dependency Installed, Updated, Dependency Updated vs)

~~~
yum install ipa-server bind-dyndb-ldap ipa-server-dns -y
~~~

Ardından **DNS server** olarak locali yapılandıralım. Bunu “**/etc/resolv.conf** ve **/etc/sysconfig/network-scripts/ifcfg-720692**”

![Crepe](assets/img/free-ipa4-iac/freeipa4-inc03.png)

**Selinux**’u da yapılandırıp **permissive mod**a çekelim yani **pasif mod**da çalışsın yani log alsın fakat sisteme müdahelede bulunmasın. Fakat makina **reboot** olunca bilgiler kaybolur. Kaybolmaması için “**cat /etc/selinux/config**” dosyası içine **SELINUX=permissive** yazıp kaydedip çıkın.

![Crepe](assets/img/free-ipa4-iac/freeipa4-inc04.png)

Daha sonra aşağıdaki komutla İpa Server’ı yükleyelim ve aynı anda gerekli yapılandırmayı yapıp çalıştıralım.

~~~
ipa-server-install --setup-dns
~~~

Aşağıda gördüğünüz üzere aynı anda yapılandırmaya başlıyoruz ve ilk olarak **hostname** bilgisini istemektedir. Biz zaten başta sunucu **hostname**’ini yapılandırmıştık ve burda **ENTER** deyip geçiyoruz.

![Crepe](assets/img/free-ipa4-iac/freeipa4-inc05.png)

Daha sonra **domain name** bilgisi istemektedir. Biz senaryomuzda **fatlan.local**’u seçtiğimiz için zaten kendisi buldu ve burda da **ENTER** deyip devam ediyoruz.

![Crepe](assets/img/free-ipa4-iac/freeipa4-inc06.png)

Ardından **realm name** bilgisini istemektedir. Tekrar **ENTER** deyip devam ediyoruz.

![Crepe](assets/img/free-ipa4-iac/freeipa4-inc07.png)

Bu kısım önemli **administrative user** olan yani **Directory Manager** için şifre bilgileri girilmesi gerekmektedir. Bu kısımda **password** bilgisini girin.

![Crepe](assets/img/free-ipa4-iac/freeipa4-inc08.png)

Bu kısım da önemli çünkü bu seferde **administrator**’un **GUI** den yönetim için şifre bilgisini istemektedir. Burada **login user** “**admin**” dir. Buraya da belirlediğiniz şifreyi girin.

![Crepe](assets/img/free-ipa4-iac/freeipa4-inc09.png)

Daha **DNS forwarders** bilgisini istemektedir. Yani kendinde barındırmadığı kayıtlar için hangi **DNS**’e sorup çözdürmesi gerektiğini belirleyen kısımdır. Ben “**no**” diyorum, çünkü içerde **GUI** den de yapabilirim bu ayarları ama siz “**yes**” derseniz belirlediğiniz **DNS server**’ların ip bilgisini ardından girmeniz gerekmektedir.(örneğin, turktelekom dns yada google dns vs.)

![Crepe](assets/img/free-ipa4-iac/freeipa4-inc10.png)

Daha sonra **reverse zone** olup olmaması için sizden talimat beklemektedir. Ortam da **mail server**‘ınız varsa mutlaka yes ve **ENTER** demelisiniz. **Reverse zone** nun daha ayrıntılı araştırmalısınız. Konu uzamaması için burda “**yes**” deyip geçiyorum.

![Crepe](assets/img/free-ipa4-iac/freeipa4-inc11.png)

Daha sonra tüm girilen isteklerle yapılandırılsın mı diye soruyor, “**yes**” deyip kuruluma devam ediyoruz.

![Crepe](assets/img/free-ipa4-iac/freeipa4-inc12.png)

Ve kurulumumuz başarılı bir şekilde gerçekleşti ve burada çok önemli bir detay daha aşağıda görüldüğü üzere belirtilen **port**ları açmanız şart diye bir uyarı veriyor. Bu işlemi de akabinde gerçekleştireceğiz.

![Crepe](assets/img/free-ipa4-iac/freeipa4-inc13.png)

Şimdi **firewalld**’yi yapılandırıp aşağıdaki komutla, aşağıdaki **port**ları açalım.

![Crepe](assets/img/free-ipa4-iac/freeipa4-inc14.png)

İlk önce şimdiki halini aşağıdaki komutla yapılandırılmamış halini görelim.

~~~
firewall-cmd --list-all
~~~

![Crepe](assets/img/free-ipa4-iac/freeipa4-inc15.png)

Şimdi **port**ları aşağıdaki şekilde açalım.

~~~
firewall-cmd --add-service={http,https,ssh,ntp,dns,freeipa-ldap,freeipa-ldaps,kerberos,kpasswd,ldap,ldaps,freeipa-replication} --permanent
~~~

Daha sonra işlemlerin geçerli olması için aşağıdaki komutu çalıştırın.

~~~
firewall-cmd --reload
~~~

Daha sonra tekrar listelediğimizde, servisler açıldığını otomatikmen kullandığı tüm **port**ların açıldığını anlıyoruz.

![Crepe](assets/img/free-ipa4-iac/freeipa4-inc16.png)

Daha sonra **reboot** edin.

Şimdi **GUI** bağlanalım, ama öncesinde kullandığınız makinanın **DNS** bilgisini kurduğunuz sunucu yapın ya da “**/etc/hosts**” dosyanıza “**ip domain_name**” bilgisini yazın. Ve sonuç başarılı.

![Crepe](assets/img/free-ipa4-iac/freeipa4-inc17.png)

![Crepe](assets/img/free-ipa4-iac/freeipa4-inc18.png)

![Crepe](assets/img/free-ipa4-iac/freeipa4-inc19.png)

Hemen aşağıdaki komutla versiyon bilgisine bakalım, hem repo’daki güncel versiyonu çözmüş oluruz. Versiyon bilgisi önemli çünkü makalenin devamı gelecek ve versiyonuna göre işleyiş gerçekleştireceğiz. 4.5 olduğunu gördük.

~~~
ipa --version
~~~

![Crepe](assets/img/free-ipa4-iac/freeipa4-inc20.png)

Aşağıdaki komutlada çalışan tüm servislerin durumuna bakalım.

~~~
ipactl status
~~~

![Crepe](assets/img/free-ipa4-iac/freeipa4-inc21.png)

Aşağıdaki komut servisleri durdurur.

~~~
ipactl stop
~~~

Aşağıdaki komuttaservisleri başlatır. Bu şekilde de yönetim gerçekleştirebilirsiniz.

~~~
ipactl start
~~~

Daha fazla ayrıntı : [https://access.redhat.com/](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html-single/identity_management_guide/index)
