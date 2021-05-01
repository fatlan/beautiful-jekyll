---
layout: post
title: SNMP Kurulumu ve Ayarları - Centos 7
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [centos, linux, network, snmp]
comments: true
---
**Simple Network Management Protocol**(Basit Ağ Yönetim Protokolü) Geniş yapıya sahip ağ yapılarında bulunan cihazları denetlemek için kullanılır. Cihaz üzerindeki sıcaklıktan, cihaza bağlı kullanıcılara, internet bağlantı hızından sistem çalışma süresine kadar çeşitli bilgiler **SNMP**’de tanımlanmış ağaç yapısı içinde tutulurlar.

Kurulumu başlatmak için aşağıdaki komutu çalıştırın. Komutla **SNMP** ve araçlarını yükleyelim.

~~~
yum -y install net-snmp net-snmp-utils
~~~

![Crepe](assets/img/snmp-cent7/snmp-cent701.png)

Daha sonra vi editörünü kullanarak aşağıdaki komutla SNMP’yi kon güre edebilirsiniz.

~~~
vi /etc/snmp/snmpd.conf
~~~

**SNMPv1** ve **SNMPv2** protokolleri bir topluluk anahtarı (community string) ile sorgulama yapar ve varsayılan olarak “**public**” ve “**private**” dir. **UDP** port **161** den çalışır. Dosyanın içine Community(Topluluk) ekliyorum.

~~~
rocommunity Community_ismi
syslocation “Istanbul/Turkiye” yada “fatlan Bilisim” gibi
syscontact email@domain_adresi

veya

com2sec readonly Snmp_ile_erişim_sağlayacak_Server_ipsi(ör:Cacti) Community_ismi
syslocation “Istanbul/Turkiye” yada “fatlan Bilisim” gibi
syscontact email@domain_adresi
~~~

Ardından servisi yeniden başlatın.

~~~
service snmpd restart
~~~

![Crepe](assets/img/snmp-cent7/snmp-cent702.png)

Yani **yanlış metot** kullanıyorsun aşağıdaki yöntemle o komutun yapacağı işi yapabilirsin diyor. Yani o komut CentOS 7’nin altındaki sürümlerde o şekilde çalışır.

~~~
systemctl restart snmpd.service
~~~

Daha sonra servisin sürekli çalışır durumda olması için aşağıdaki komutu çalıştırırsanız gene yukardakine benzer bir uyarı alacaksınız. Aynen bu komutta 7’nin altındaki sürümlerde kullanılır.

~~~
chkconfig snmpd on
~~~

![Crepe](assets/img/snmp-cent7/snmp-cent703.png)

Aşağıdaki şekilde tamamlanacaktır.

~~~
systemctl enable snmpd.service
~~~



