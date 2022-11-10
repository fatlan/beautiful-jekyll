---
layout: post
title: İlgili Log Dosyasını Rsyslog ile Merkezi Log Server'a Yönlendirme on Ubuntu
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/25-26-20-uelinuxegitim/25-26-20le05.png
#share-img: /assets/img/path.jpg
tags: [ubuntu, log, rsyslog, syslog, linux]
comments: true
---

Ubuntu server üzerinde kurulu herhangi bir servis ya da uygulamamızın belirli bir path'de mevcut olan log dosyasını siem(10.10.10.99) server'a yönlendirdiğimizi varsayalım.

~~~
sudo vi/etc/rsyslog.d/my-service.conf
~~~
~~~
$ModLoad imfile
$InputFilePollInterval 10
$PrivDropToGroup safirdepo
$InputFileName /opt/my-service-path/my_service_log.log
$InputFileTag MY SERVICE
$InputFileStateFile MY_SERVICE_LOG
$InputFileSeverity info
$InputFileFacility local5
$InputRunFileMonitor
$InputFilePersistStateInterval 1000
local5.* @10.10.10.99:514
~~~

Ardından servisi yeniden başlatın.
~~~
sudo systemctl restart rsyslog.service
~~~

Durumu netleştirmek adına yönlenen log akışını aşağıdaki komutla izleyebilirsiniz.
~~~
sudo tcpdump -i ens3 -annXX host 10.10.10.99
~~~

Bu arada default sistem log'larını(/var/log/syslog) yönlendirmek için, aşağıdaki gibi yapmanız yeterli.
~~~
sudo vi /etc/rsyslog.conf
~~~
~~~
*.*    @10.10.10.99:514 #tcp
*.*    @@10.10.10.99:514 #udp
~~~
~~~
sudo systemctl restart rsyslog.service
~~~

Konumuz dışı ama ek olarak çalıştığınız sunucunun log alabilmesi için, aşağıdaki gibi ayarlayabilir ardından bu server'a da log yönlendirebilirsiniz.
~~~
sudo vi /etc/rsyslog.conf
~~~
~~~
# provides UDP syslog reception
module(load="imudp")
input(type="imudp" port="514")

# provides TCP syslog reception
module(load="imtcp")
input(type="imtcp" port="514")
~~~
~~~
sudo systemctl restart rsyslog.service
~~~
Teyit,
~~~
sudo ss -plnt | grep 514
~~~
~~~
LISTEN     0      25           *:514                      *:*                   users:(("rsyslogd",pid=7788,fd=7))
LISTEN     0      25          :::514                     :::*                   users:(("rsyslogd",pid=7788,fd=8))
~~~

ref: [serverfault](https://serverfault.com/questions/396136/how-to-forward-specific-log-file-outside-of-var-log-with-rsyslog-to-remote-serv) | [casesup](https://www.casesup.com/category/knowledgebase/howtos/how-to-forward-specific-log-file-to-a-remote-syslog-server)