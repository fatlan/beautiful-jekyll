---
layout: post
title: Linux Sunucuya SSH Bağlantı Sonrası Uyarı Postası Gönderme
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [ssh, mail, linux, posta, e-mail]
comments: true
---
Sunucunuza yetkisiz bağlantı yada **ssh** bağlantılarını takip edebilme gibi bir çok neden için, özellikle **root** kullanıcıyla bağlantıları takip edebilmek ve gözlemleyebilmek için bağlantı sonrası uyarı isteyebilirsiniz ki genelde bu yöntem e-postayla uyarı olur. Bunun için aşağıdaki aşamaları gerçekleştirdiğinizde, ilgili user için **login** sonrası uyarı maili alacaksınız. Bu işlemi **mailx** uygulamasını kullanarak ve ardından **.bashrc** dosyasında bir kaç satır ekleme ile halledeceğiz.

İlk önce aşağıdaki komutla **mailx** uygulamasını kuralım.

**Debian** (aynı aileye mensup tüm sistemlerde geçerlidir) **OS**’lar için;

~~~
sudo apt install mailx -y
~~~

**CentOS**(**RedHat** ve aynı aileye mensup tüm sistemlerde geçerlidir ) **OS**’lar için;

~~~
yum install mailx -y
~~~

Ardından kurulum bittikten sonra herhangi bir **editör** yardımıyla istediğiniz kullanıcının **.bashrc** dosyasına aşağıdaki kodu ekleyin. Tabi **ServerName** (Sunucu **HostName** ile), **your@yourdomain.com** (Uyarı gönderilecek mail adresi ile) değiştirin. Aşağıda görsel olarak örneklendirilmiştir.

~~~
echo 'ALERT - Root Shell Access (ServerName) on:' `date` `who` | mail -s "Alert: Root Access from `who | cut -d'(' -f2 | cut -d')' -f1`" your@yourdomain.com
~~~

**.bashrc** dosyasını aşağıdaki komut ile görebilirsiniz.

~~~
ls -la
~~~

![Crepe](/assets/img/mail-send-after-ssh/ms-assh01.png)

~~~
vi .bashrc
~~~

En alt satıra ekledim.

![Crepe](/assets/img/mail-send-after-ssh/ms-assh02.png)

Test edelim ve root ile ssh bağlantısı gerçekleştirelim. Ardından uyarı mailinin geldiğini göreceksiniz.

![Crepe](/assets/img/mail-send-after-ssh/ms-assh03.png)

**NoT** : Ev kullanıcıları için **mail** gönderimi gerçekleşmiyecektir çünkü **mailing** yani **spam** önünü alabilmek için **25** portu dışarı kapalıdır. Ayrıca **PTR** kaydı fln gerekiyor derken son kısımda muhtemelen işlem gerçekleşmeyecektir. Yoksa buraya kadar yapılan tüm işlemler doğru.

Bu işlemi başka bir **smtp** sunucusu üzerinden de yapabilirsiniz. **From** kısmını da belirtebilirsiniz. Yukarıda **.bashrc** kısmını aşağıdaki gibi yapılandırarak bu işlemi gerçekleştirebilirsiniz.

~~~
echo 'ALERT - Root Shell Access (ServerName) on:' `date` `who` | mail -s "Alert: Root Access from `who | cut -d'(' -f2 | cut -d')' -f1`" -S smtp=smtp://smtpsunucuip -S from="gönderici mail adresi" $"gönderilecek mail adresi"
~~~

ref: [https://www.systutorials.com/5167/sending-email-using-mailx-in-linux-through-internal-smtp/](https://www.systutorials.com/5167/sending-email-using-mailx-in-linux-through-internal-smtp/)