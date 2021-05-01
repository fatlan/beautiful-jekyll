---
layout: post
title: UFW - Uncomplicated Firewall
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [ufw, ubuntu, linux, security]
comments: true
---
**UFW(Uncomplicated Firewall)** Ubuntu Linux’lar için varsayılan olarak gelen güvenlik duvarı aracıdır.

**Ufw(Uncomplicated Firewall)**’nun iptables’a alternatif olmasında ki en büyük etken güvenlik duvarı olarak kullanıcıya kolay kullanım sağlamasıdır. Bu yüzden Linux topluluğu tarafından kısa zamanda desteklenip, popüler hale gelmiştir ve birçok dağıtımda gömülü bir şekilde gelmektedir.

**Ufw** sistemde **inactive** olarak gelmektedir. Çalışır vaziyete getirmek için active hale getirilmelidir. Bunun için aşağıdaki komutu kullanılabilir. **Ufw**’yu **active** hale getirmek için,

~~~
sudo ufw enable
~~~

![Crepe](assets/img/ub-ufw/ufw01.png)

Tekrar **inactive mode**’ye getirmek için,

~~~
sudo ufw disable
~~~

![Crepe](assets/img/ub-ufw/ufw02.png)

Aslında her güvenlik duvarı yönetiminde işleri kolaylaştıracak ve güvenliği sağlayacak ilk hareket içerden dışarı çıkışları serbest hale getirmek, dışardan içeri girişleri ise engellemektir.

**NoT** : Hatta içerden dışarı ağ akışını daha sıkı hale getirebilirsiniz, yani lokaldeki kullanıcıları içerden dışarı çıkışta da sınırlandırılabilir. Yalnızca ve gerekmedikçe varsayılan olarak 80 ve 443 portlarını açık bırakıbilir. Bunun için “**sudo ufw default deny outgoing**” komutunu çalıştırdıktan sonra **80** ve **443**’e izin vermek için kural girilmelidir (ilgili kuralları aşağıda gösterilecektir). Tabi bu durum beraberinde yönetim zorluğuyla beraber kural sıklığını da getirecektir.

Dışardan içeri gelen paketleri engellemek için,

~~~
sudo ufw default deny incoming
~~~

![Crepe](assets/img/ub-ufw/ufw03.png)

İçerden dışarı çıkan paketlere izin vermek için,

~~~
sudo ufw default allow outgoing
~~~

![Crepe](assets/img/ub-ufw/ufw04.png)

Servis bazında; **ssh** servisine izin vermek için,

~~~
sudo ufw allow ssh
~~~

Servis bazında; **ssh** servisine engellemek için,

~~~
sudo ufw deny ssh
~~~

Port bazında izin vermek için,

~~~
sudo ufw alllow 22/tcp
sudo ufw allow 53/udp
~~~

Port bazında engellemek için,

~~~
sudo ufw deny 22/tcp
~~~

Port aralığına izin vermek için,

~~~
sudo ufw allow 1500:2500/tcp
sudo ufw allow 1500:2500/udp
~~~

Port aralığını engellemek için,

~~~
sudo ufw deny 1500:2500/tcp
sudo ufw deny 1500:2500/udp
~~~

İp’ye izin vermek için,

~~~
sudo ufw allow from 192.168.0.25
~~~

İp’yi engellemek için,

~~~
sudo ufw deny from 192.168.0.25
~~~

Var olan, izin verilmiş kuralı silmek için,

~~~
sudo ufw delete allow ssh
sudo ufw delete allow 80/tcp
sudo ufw delete allow 1500:2500/tcp
~~~

Var olan, engellenmiş kuralı silmek için,

~~~
sudo ufw delete deny ssh
sudo ufw delete deny 80/tcp
sudo ufw delete deny 1500:2500/tcp
~~~

Şimdi “**sudo ufw status numbered**” komutu ile yapılan kurallar listelenecektir.

![Crepe](assets/img/ub-ufw/ufw05.png)

Yukarıda numbered parametresi ile görüntülenen kurallar aşağıdaki gibi sıra numarasına göre de silinebilir.

~~~
sudo ufw delete [number]
~~~

Tüm kuralları “sudo ufw status” ile beraber verbose parametresini ekleyerek de her ayrıntı görüntülenebilir.

~~~
sudo ufw status verbose
~~~

Tüm kuralları sıfırlamak yani **ufw**’yu varsayılana, **default**(fabrika çıkışına)’a çekmek için aşağıdaki komutu kullanılabilir.

~~~
sudo ufw reset
~~~

![Crepe](assets/img/ub-ufw/ufw06.png)

Ayrıca **ufw** güvenlik duvarı için **log**‘lamayı **aktif/pasif** etmek için aşağıdaki komutu kullanabilirsiniz. Log takibini ise “**/var/log/syslog**” dosyası üzerinden yapabilirsiniz.

~~~
sudo ufw logging ON/OFF
~~~

