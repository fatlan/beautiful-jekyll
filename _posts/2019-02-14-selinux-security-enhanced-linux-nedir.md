---
layout: post
title: Selinux (Security Enhanced Linux) nedir
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [selinux, security, linux]
comments: true
---

**Selinux** **C** dilinde, **1999** yılında **NSA** tarafından ordu için geliştirilmiş ve daha sonrasında **Linux** çekirdeğine eklenmiş ek bir güvenlik protokolü modülüdür. **Selinux** başta **RedHat** olmak üzere, özellikle türevi Linux dağıtımlarında entegre olarak gelmektedir fakat buna rağmen hepsi bu modülü entegre olarak içinde barındırmamaktadır. **Ubuntu** buna örnek olarak verilebilir ancak sonradan kurulum gerçekleştirilebilir.

**Selinux** kullanıcı programlarına, sistem servislerine, dosyalarına ve ağ kaynaklarına erişimi kısıtlayan zorunlu erişim kontrolü protokolüdür desek yanlış olmaz. Temel olarak iki modelde çalışır. **DAC(Discretionary Access Control-İsteğe Bağlı Erişim Kontrolü)** ve **MAC (Mandatory Access Control-Zorunlu Erişim Kontrolü)** mekanizmalarıdır. Temel çalışma prensibi kullanıcı ve sistem servislerinde yapılması gereken işlemleri en az yetki ile sınırlandırmayı amaçlar. **DAC**, kullanıcının ya da sürecin kaynaklara erişimini kullanıcı sahipliğine veya izinlerine bakarak belirler. **MAC**, kullanıcıların oluşturdukları nesnelerin üzerindeki denetim düzeyini, tüm dosya sistemi nesneleri için ek etiket ekleyerek kısıtlar. Dolayısıyla kullanıcı ya da süreçlerin bu dosyalara erişebilmesi için etiketlere uygun erişim haklarının olması gerekiyor. Bu arada **MAC DAC**’a müdahele etmemektedir yani onu ezmemektedir.

Konunun daha iyi anlaşılması için etiketler incelenerek detaylandırılacaktır. ls’in Z parametresini kullanarak kullanıcı, servislerin ya da portların etiketlerini görüntülenebilir.

/etc/passwd dosyası incelendiğinde,
~~~
ls -Z /etc/passwd
~~~

Çıktı aşağıdaki gibidir.
~~~
-rw-r–r–. root root system_u:object_r:passwd_file_t:s0 /etc/passwd
~~~

Selinux şartların takibini “**user:role:type:level**” göre yapar, yani yukarıdaki örneği baz alırsak “**system_u:object_r:passwd_file_t:s0**” olarak yetki politikası belirler.

Şimdi “**touch**” komutu ile kök dizine **(/)** (dosya istenilen dizinde de oluşturabilir) bir dosya oluşturup onun etiketlerini kontrol edildiğinde,

~~~
touch fatihaslan
~~~

Şimdi etiketleri “**ls -Z**” ile kontrol edildiğinde,

~~~
ls -Z /fatihaslan
~~~

Çıktı aşağıdaki gibidir.
~~~
-rw-r–r–. root root unconfined_u:object_r:etc_runtime_t:s0 /fatihaslan
~~~

Http servisi için etiketi görüntülendiğinde.

~~~
ls -Zd /etc/httpd/
~~~

![Crepe](assets/img/selinux-sel/selinux01.png)

Alt klasörlerle beraber listelemek,

~~~
ls -Z /etc/httpd/
~~~

![Crepe](assets/img/selinux-sel/selinux02.png)

**NoT** : Her dosyanın etiketi, üstündeki klasörün etiketi neyse onu çekmektedir.

Şimdi “**/var/www/html/**” dizinin içine **index.html** dosyası oluşturulup, içine de **Fatih ASLAN** yazılacaktır ve alacağı etiket “**httpd_sys_content_t**” olacaktır çünkü “**/var/www/html/**” dizininin etiketi “**httpd_sys_content_t**”dir.

![Crepe](assets/img/selinux-sel/selinux03.png)

~~~
ls -Zd /var/www/html/index.html
~~~

![Crepe](assets/img/selinux-sel/selinux04.png)

**NoT** : “**cp**” komutu ile kopyalanan dosyanın etiketi hedef dizininin etiketi olacaktır fakat “**mv**” komutu ile dosya taşınırsa kaynak dizinin etiketi kalmaya devam edecektir. Aşağıdaki görüntüden de anlaşılabilir.

Bu bilgiler ışığında sistem de anormal sorunlar çıktığında ve kullanıcı, dosya izinlerinde de sorun yoksa Selinux kısmını da bakılmalıdır ki etiket kısmında sorun olabileceğinden **Selinux** erişim sorunu çıkaracaktır.

![Crepe](assets/img/selinux-sel/selinux05.png)

Hazır yeri gelmişken bir konudan daha bahsetmek gerekmektedir. Yukarıdan da anlaşılacağı üzere herhangi sebepten ya da dosyanın taşınmasından ötürü etiketi bozulmuş olan dosyaları bulunduğu dizindeki etikete taşımamak için “restorecon” komutunu kullanılabilir. Aşağıda örnek gösterilmiştir.

~~~
restorecon -Rv /var/www/html/
~~~

Ayrıca “semanage” komutu ile kullanıcı tarafında ki etiketler de görüntülenebilir.

~~~
semanage user -l
~~~

Çıktı aşağıdaki gibidir.

![Crepe](assets/img/selinux-sel/selinux06.png)

Hatta tüm portlara atanan **Selinux** etiketlerini de “**semanage**” komutu ile görüntülenebilir. Fakat aşağıdaki örnekte sadece **httpd** servisi için atanan portlardaki etiketler görüntülenecektir.

~~~
semanage port -l | egrep ^http_port_t
~~~

![Crepe](assets/img/selinux-sel/selinux07.png)

Süreçlere(process) ait etiketleri aşağıdaki komut ile elde edilebilir.

~~~
ps -eZ
~~~

Özel olarak kullanıcıya ait etiketi görmek için aşağıdaki komutu kullanılabilir.

~~~
id -Z
~~~

![Crepe](assets/img/selinux-sel/selinux08.png)

Bu aşamadan sonra **Selinux**’un çalışma mod’larına bakıp, yukarıdaki teknik bilgiler ışığında hangi çalışma durumunda kalmasını da aktardıktan sonra **Selinux** kısmı noktalanacaktır. Öncesinde “**/etc/selinux/config**” **cat** ile okutalım, ardından **SELINUX** ve **SELINUXTYPE** parametlerinden rahatlıkla bahsedebilelim.

![Crepe](assets/img/selinux-sel/selinux09.png)

    **SELINUX**=enforcing, Selinux ilkelerine dayalı kurallarının geçerli olduğu yani aktif çalıştığı mod’dur.
    **SELINUX**=permissive, Selinux politikası uygulanmaz fakat log tuttuğu mod’dur.
    **SELINUX**=disabled, Selinux’un devredışı olduğu mod’dur. Sadece DAC kuralları geçerlidir.
    **SELINUXTYPE**=targeted, hedeflenen ilkelerde uygulanan varsayılan politikadır.
    **SELINUXTYPE**=mls, çok seviyeli güvenlik politikasıdır.

Şimdi hangi mod’da çalıştığı görüntülenecektir.

~~~
getenforce
~~~

![Crepe](assets/img/selinux-sel/selinux10.png)

Daha ayrıntılı görmek için aşağıdaki komut kullanılabilir.

~~~
sestatus
~~~

![Crepe](assets/img/selinux-sel/selinux11.png)

**Mod**’lar arsında geçişi direk “**/etc/selinux/config**” dosyasından düzenleyerek yapabilir ya da “**setenforce**” komutu kullanabilir. Aşağıdaki görselde durum daha net anlaşılacaktır.

![Crepe](assets/img/selinux-sel/selinux12.png)



