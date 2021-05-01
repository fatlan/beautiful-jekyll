---
layout: post
title: Merkezi Log Server Oluşturma - Rsyslog
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [log, rsyslog, linux, syslog]
comments: true
---
Sistem adminleri, Network yöneticileri, Güvenlik uzmanları, Yazılımcılar ve hatta Hacker için bile olmazsa olmaz bişey varsa oda sistemde var olan log lar yada oluşacak log lardır. Log'ların hayatımızdaki yeri ve önemi tartışılmazdır. Çünkü log lar aracılığı ile bir çok sorunun ve problemin üstesinden geliriz. Analiz gerçekleştirir, sistemlere olan saldırıları hatta öncesinde yapılmış saldırıları keşfederiz. Aslında daha fazla yazılacak teorik çok şey var fakat ben çok fazla uzatmak istemiyorum. Log, loglama deyince akan sular durur ve herkesin az çok bilgisi vardır. Ben hemen yapmak istediğim ortamı hayata geçireyim.

Ne yapmak istiyorum.? Ortamda merkezi bir log server oluşturmak istiyorum. Yani tüm sunucularımın, rewall yada benzeri cihazlarımın yada göndermek istediğim ne varsa log olarak, tek bir merkezi log sunucu oluşturup ona göndermek istiyorum. Bunun faydalarına gelince zaten aklınızda bir sürü ışık yanmıştır. Örneğin bence bu log server'ı daha sonra **5651 yasası**na göre değerlendirebilirsiniz yada sistemleriniz herhangi biri saldırıya uğradığında hacker bulunduğu o server daki log'ları temizleyebilir fakat log server'a erişimi olmadığından ordaki log'lara dokunamayacak ve siz bu log ları rahatlıkla değerlendirebileceksiniz.

Ben senaryoyu iki sanal Linux sunucuda **Syslog(rsyslog)** servisini kullanarak gerçekleştireceğim. Sunuculardan biri **client** olacak, log basacak diğeri de **log server** olacak ve log ları üstüne çekip barındıracak. Bunu kullanıcı tarafında ve Log server tarafında bir kaç ayarlama ve yapılandırmayla sağlayacağız.

![Crepe](assets/img/rsyslog-slog/m-rsyslog01.png)

Ama öncesinde Log yapılarından, seviyelerin bahsedelim. Log'lar **facility** ve **severity** - **level** olarak iki sütunda inceleyecek olursak eğer **facility.level** olarak belirlesek yanlış olmaz. Yani **facility** olarak (**none, kernel, mail, cron, authpriv, user**) desek, level olarak (**emerg, critical, alert, err, info, debug**) olarak ele aldığımızda; örneğin sistemdeki tüm herşeyin sadece hata log'larını almak için (***.err** kullanabiliriz.) yada bilgi mesajları ve yukarı doğru içerdiği log'ları almak için (***.info** kullanabilirsiniz.). Yukarı doğru derken hemen onu da açıklayalım. Mesela **level- seviye** olarak **info** seçilmiş ise içinde(**emerg, critical, alert, err** uda barındırmaktadır.), Seviyelerden en konuşkanı **debug**'tır. Tüm her hareketi log'lar. **user.emerg** yada **user.err** yada **user.info** gibi gibi kullanabilirsiniz.

![Crepe](assets/img/rsyslog-slog/m-rsyslog02.png)

Her Linux sistemde (“**/etc/rsyslog.conf**”) mevcut olan bir kaç satırdan bahsedeyim ve bahsedersek mevzu daha iyi anlaşılacaktır.

***.info;mail.none;authpriv.none;cron.none /var/log/messages** → Burada sistemdeki her **facility**'i (*) **info** seviyesinde al (***.info;**) fakat** mail,authpriv** ve **cron**'u alma, neden çünkü onları **/var/log/** içerisinde başka bir dosyaya aldıracak, yapılandırmayı yapacak. Aşağıdaki screenshot'ta durum daha net anlaşılacak.

![Crepe](assets/img/rsyslog-slog/m-rsyslog03.png)

Ayrıca Linux ta log'lar için özelleştirilmiş **local soket**ler bulunmaktadır.

Bu kadar teorik bilgiden sonra hemen yapılandırmaya girişelim. Çünkü loglardan gene ve **/var** dizini **/var/log** dizininden ayrıntılı olarak yeri geldi mi bir başka makalede bahsedeğiz. Bir kullanıcı makinasından(192.168.2.71) log göndereceğimiz için orada yapılandırma yapacağız, ikinci olarak da log sunucuda(192.168.2.150) logları alabilmek için yapılandırma yapıp, işlemleri de teyit ettikten sonra makaleyi noktalayacağız.

***Server tarafında yapılandırma**

Yapılandırmayı “**/etc/rsyslog.conf**” dosyasından gerçekleştireceğiz. Vi, vim yada nano editörü ile dosyanın içine giriyoruz. Ben vim'i tercih ediyorum çünkü satırları renklendiriyor.

~~~
vim /etc/rsyslog.conf
~~~

Bu dosyanın içerisinde; son iki satırda bulunan **Modload** ve **Input** satırlarındaki “**#**” işaretini siliyoruz.

![Crepe](assets/img/rsyslog-slog/m-rsyslog04.png)

Durum aşağıdaki gibi olacak, dosyayı kaydedip çıkın.

![Crepe](assets/img/rsyslog-slog/m-rsyslog05.png)

Ardından aşağıdaki komutla servisi **restart** edin.

~~~
systemctl restart rsyslog.service
~~~

***Kullanıcı tarafından yapılandırma(Logları yönlendirelim);**

Tüm log ları info seviyesinde göndereceğim.

Gene bir editör aracılığı ile “**/etc/rsyslog.conf**” yapılandırma dosyasına girelim. Ve aşağıda resimdeki gibi yapılandırıp, kaydedip çıkalım. Bu arada **@(udp)**, **@@(tcp)** dir.

~~~
vim /etc/rsyslog.conf
~~~

![Crepe](assets/img/rsyslog-slog/m-rsyslog06.png)

**NOT**: bu log ları lokalde tutmak istemiyorsanız, dosyanın içindeki yapılandırma satırları silebilir ya da satırların başına “**#**” işareti koyarsanız, satılar yorum satırı olarak algılanacak ve log'lar sadece log'ları yönlendirdiğiniz sunucuda barınacaktır.

Şimdi burada da **servisi restart** edelim.

~~~
systemctl restart rsyslog.service
~~~

**NOT2**: Ayrıca input girdilerini(log yönlendirme) aşağıdaki gibi daha spesifik olarak **/etc/rsyslog.d/** (**/etc/rsyslog.d/YntmLog.conf** gibi) içine dosya olarak oluşturup, yönlendirebilirsiniz.

**“/etc/rsyslog.conf içine yazılacak.”**
~~~
$ModLoad imfile
~~~

**“/etc/rsyslog.d/YntmLog.conf içine yazılacak, aslında /etc/rsyslog.conf içine de yazılabilir.”**
~~~
##remote syslog settings
$InputFileName /var/log/httpd/access_log
$InputFileTag celery
$InputFileStateFile celery- le1
$InputFileSeverity info
$InputFileFacility local5
$InputRunFileMonitor
$InputFilePersistStateInterval 1000
local5.* @192.168.2.150
~~~

Şimdi sıra yaptığımız işlemi teyit etmekte. Bunu da “**logger**” komutu ile yapacağız. Yani o komut ile dışarıdan custom bir log üreteceğiz.

~~~
logger -p user.info “Bu mesajı info seviyesinde oluşturduk ve log sunucuda görmeliyiz.”
~~~

![Crepe](assets/img/rsyslog-slog/m-rsyslog07.png)

Şimdi Log sunucusunda görebildiğimizi teyit edelim.

![Crepe](assets/img/rsyslog-slog/m-rsyslog08.png)

Bu arada sunucular arasında network erişimi ve **514 port**’undan erişime açık olmalıdır.