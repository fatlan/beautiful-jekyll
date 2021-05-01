---
layout: post
title: SSH Oturumu Zaman Aşımı - SSH timeout
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [ssh, linux, security]
comments: true
---
**SSH** kısa tabir ile istemci sunucu arasında bir bağlantı açmaktan ibarettir. Bu bağlantı sayesinde uzaktan sunucularımıza, ssh’in yapısı gereği güvenli haberleşme sağlanır. Fakat ssh’in güvenli bağlantısının dışında açılan oturumun zaman aşımı durumundan bahsetmek gerekir. Çünkü bu durum başlı başına bir güvenlik zafiyetini beraberinde getirebilir.

Şöyle ki kullanıcı makinenizden birden fazla sunucuya ssh bağlantısı sağlamış olduğunuz varsayıldığında ve herhangi bir acil durumda ya da insani bir zafiyet olan unutkanlık yüzünden makinenizi kilitlemeden başından ayrıldığınızı farz ederseniz bu durum kötü niyetli kişiler için bulunmaz bir fırsat olabilir ya da bu kişiler kötü niyetli olamayan çocuklarınızda olabilir. Bu durumda fiziksel olarak ele geçirilen makinenizle beraber sürekli açık olarak kalan ssh oturumlarınız, dolayısıyla sunucularınız da ele geçirilmiş olur. İşte bu zafiyetin bir nebze de olsa önüne geçebilmek için ssh bağlantılarına oturum zaman aşımı yapılandırarak sağlanabilir.

Bu durumda sizin belirlediğiniz süre boyunca kullanılmayan ve açık kalan ssh oturumları otomatik olarak kendiliğinden sonlanarak istemci sunucu iletişimini kesecektir. Böylelikle uzun süre kullanılmayan bağlantıların güvenliğini de sağlamış olacaktır.

Bu yapılandırma ilgili sunucularımızın “**sshd_config**” dosyasında, iki parametre eklenerek gerçekleştirilebilir. Yapılandırmaya geçmeden önce bu parametrelerden bahsetmek gerekir. İlk parametre “**ClientAliveInterval**”, bu parametre ile kaç saniyede sonra **null**(boş) paketi göndereceğine karar verir. İkinci parametre “**ClientAliveCountMax**”, bu parametre saniyesi ile de birinci parametrede belirtilen saniyenin, kaç saniye aralıkla kontrol edilmesini sağlar. Şöyle ki **ClientAliveInterval*ClientAliveCountMax**’dır. Yani **ClientAliveInterval** saniye bir **null**(boş) paketi göndermesini, **ClientAliveCountMax** kadar aralıkla oturumun test eder ve o süre zarfı boyunca kullanılmadığını tespitle beraber oturumu korur fakat çarpımdan elde edilen süre dolduğu an oturum kapanır. Şimdi aşağıdaki yapılandırma ile konu daha da anlaşılıp, basit olduğu kavranacaktır.

İlk önce ilgili sunucuda “**/etc/ssh/sshd_config**” dosyası herhangi bir editör aracılığı ile açılır. Ardından aşağıdaki parametreler eklenir.

~~~
- ClientAliveInterval 600
- ClientAliveCountMax 0
~~~

Ardından **sshd servisi restart** (**systemctl restart sshd.service**) edilmelidir.

Yapılandırmayı açıklayacak olursam eğer **600 saniye** belirleyerek ve aralık belirtmeden, **600saniye / 60saniye = 10 dakika** yani oturum hiçbir şey yapmadan **10 dakika** boyunca açık kalacak fakat 10 dakikayı geçtikten hemen sonra oturumu otomatik olarak sonlandıracaktır.