---
layout: post
title: Firewalld
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [security, firewalld, firewall, linux]
comments: true
---
**Firewalld**’nin **IPTables**’a göre fark olarak en belirgin özelliği bölgeler(**zone**) kullanmasıdır. **Zone** yapılan kurallar geçerlidir. Her bir **zone**(bölge)’de farklı yapılanmalar kullanabilir. Bu **zone**(bölge)’leri değiştirerek uyguladığınız tüm kuralları değiştirebilirsiniz.

**Firewall-cmd** bu iş kullanacağımız en temel komuttur.

Aşağıdaki komutla tüm yapılandırmalara ait detaylar görüntülünebilir. Aktif olup olmadığı, izin verilen **port** ya da **servisler** gibi birçok detay mevcuttur.

~~~
firewall-cmd --list-all
~~~

![Crepe](assets/img/firewalld19/lfd01.png)

Aşağıdaki komut ile de mevcuttaki tüm **zone**(bölge)’leri listeleyebilirsiniz.

~~~
firewall-cmd --get-zones
~~~

![Crepe](assets/img/firewalld19/lfd02.png)

Aşağıdaki komutlar ile de **default**(fabrika ayarı)’ta kullanılan **zone**(bölge)’yi görüntüleyebilirsiniz.

~~~
firewall-cmd --get-default-zone
~~~
ya da

~~~
firewall-cmd --get-active-zones
~~~

![Crepe](assets/img/firewalld19/lfd03.png)

Aşağıdaki komut ise default **zone**(fabrika çıkışı bölge)’yi değiştirebilirsiniz.
**NoT1** : **Firewalld**’de kurallar oluşturduktan sonra makineyi yeniden başlatmadan geçerli olması için “**firewall-cmd --reload**” komutu çalıştırmalısınız.

~~~
firewall-cmd --set-default-zone=home
firewall-cmd --reload
~~~

Alttaki komut ile servise dışarıdan erişim için izin verilebilir.
**NoT2** : **Permanent**(kalıcı) parametresi burada kuralın kalıcı olmasını sağlamaktadır.

~~~
firewall-cmd --zone=public --add-service=http --permanent
firewall-cmd --reload
~~~

Ya da var olan kurallar silinebilir.

~~~
firewall-cmd --zone=public --remove-service=http --permanent
firewall-cmd --reload
~~~

Aşağıdaki komut ile de **servis** değil de **port** bazlı erişim izini verilebilir.

~~~
firewall-cmd --zone=public --add-port=443/tcp --permanent
firewall-cmd --zone=public --add-port=53/udp --permanent
firewall-cmd --reload
~~~

Ya da var olan kurallar silinebilir.

~~~
firewall-cmd --zone=public --remove-port=443/tcp --permanent
firewall-cmd --zone=public --remove-port=53/udp --permanent
firewall-cmd --reload
~~~

Birde zengin(**rich**) kurallar vardır. Adında da anlaşılacağı gibi özel, ip bazlı oluşturulabilen kurallar vardır. Bu konuya da değinmek gerekmektedir. Sadece 192.168.1.25 ip’sinden gelen tüm istekleri kabul eden kural oluşturulur.

~~~
firewall-cmd --zone=public --add-rich-rule=’rule family=”ipv4′′ source address=192.168.1.25 accept’
firewall-cmd --reload
~~~

Aşağıda ise 192.168.1.25 ip’sinden gelen 22 isteklerini reddetmektedir.

~~~
firewall-cmd --zone=public --add-rich-rule=‘rule family=”ipv4” source address=”192.168.1.25” port port=22 protocol=tcp reject’
firewall-cmd --reload
~~~

Rich rule(zengin kurallar)’leri aşağıdaki komut ile de görüntülenebilir.

~~~
firewall-cmd --list-rich-rules
~~~

Şimdi şu ana kadar yapılan tüm kurallar “**firewall-cmd --list-all**” komutu ile görüntülenebilir.

![Crepe](assets/img/firewalld19/lfd04.png)

