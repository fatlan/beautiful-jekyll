---
layout: post
title: SNMP Kurulumu ve Ayarları - Ubuntu Server 14.04
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [network, linux, ubuntu, snmp]
comments: true
---

Simple Network Management Protocol(Basit Ağ Yönetim Protokolü) Geniş yapıya sahip ağ yapılarında bulunan cihazları denetlemek için kullanılır. Cihaz üzerindeki sıcaklıktan, cihaza bağlı kullanıcılara, internet bağlantı hızından sistem çalışma süresine kadar çeşitli bilgiler SNMP’de tanımlanmış ağaç yapısı içinde tutulurlar.

Kurulumu başlatmak için aşağıdaki komutu çalıştırın.

~~~
apt-get install snmp snmpd -y
~~~

Daha sonra **vi** editörünü kullanarak aşağıdaki komutla **SNMP**’yi kon güre edebilirsiniz.

~~~
vi /etc/snmp/snmpd.conf
~~~

**SNMPv1** ve **SNMPv2** protokolleri bir topluluk anahtarı (community string) ile sorgulama yapar ve varsayılan olarak “**public**” ve “**private**” dir. UDP port 161 den çalışır.

Dosyanın içine Community(Topluluk) ekliyorum.

~~~
rocommunity fatlan(community_string)
syslocation “Istanbul/Turkiye” yada “fatlan Bilisim” gibi
syscontact email@domain_adresi
~~~

Daha sonra servisi yeniden başlatın ve işlem tamam.

~~~
service snmpd restart
~~~

