---
layout: post
title: Linux Sunucular Üzerinde ISCSI Yapılandırması
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [linux, iscsi, lun, disk]
comments: true
---
![Crepe](assets/img/lin-iscsi-ad/icsi-liad01.png)

**iscsi**(**Internet Small Computer Systems Interface**), **ağ**(network) üzerinden **veri depolama**(storage) ünitelerine **tcp/ip** protokolünü kullanarak **blok** düzeyinde erişim sağlayan yöntemdir.

Storage tarafında ilgili **SVMs**, **LUNs**, **Volumes** ve **network** ayarlarını yapıptan sonra **iscsi**’nin sunuculara erişimi için “**Initiators**” ve “**Initiator Group**” erişimlerini vermelisiniz. **Storage** tarafına girmiyorum konu genişleyip, dağılmaya başlar.

**Not** : Sunucunun “**InitiatorName**” elde etmek için aşağıdaki komutu kullanabilirsiniz.

~~~
cat /etc/iscsi/initiatorname.iscsi
~~~

**İscsi’leri Keşfedelim;**

Sunucu üzerinde **İscsi** hedef tespiti için aşağıdaki komut ile **discover** işlemini yapalım. İlgili ip’lere bağlı erişimi olan tüm hedefleri listeleyecektir.

~~~
iscsiadm -m discovery -t st -p 10.10.30.5
~~~

Çıktı: 10.10.30.5:3260,1030 iqn.1992-08.com.netapp:sn.2626a89cce1111e66c5200a098d9e8ba:vs.18

**İscsi**’lere **login** olalım;

Şimdi var olan tüm **iscsi** hede erine **login** olmak için aşağıdaki komutu çalıştırın.

~~~
iscsiadm -m node -l
~~~

Ya da tek tek **login** olmak için aşağıdaki komutu da kullanabilirsiniz.

~~~
iscsiadm -m node -T -l iqn.1992-08.com.netapp:sn.2626a89cce1111e66c5200a098d9e8ba:vs.18 -p 10.10.30.5:3260
~~~

**İscsi’lere logout olmak için;**

**İscsi**'den tümüyle birden **logout** olmak için,

~~~
iscsiadm -m node -u
~~~

Ya da tek tek **logout** olmak için,

~~~
iscsiadm -m node -T -u iqn.1992-08.com.netapp:sn.2626a89cce1111e66c5200a098d9e8ba:vs.18 -p 10.10.30.5:3260
~~~

**İscsi** diskleri genişletme;

**İscsi** ile çalışan **stroge**’de alan genişlettikten sonra aşağıdaki komut ile de sunucuya yansımasını sağlayabilirsiniz.

~~~
iscsiadm -m session --rescan
~~~

**İscsi** genişletmesinden sonra eğer hedefleri **multipath** olarak kullanıyorsanız aşağıdaki komutu da çalıştırmanız gerekebilir.

~~~
multipathd resize map “multipath alias ya da wwid_numarası”
~~~

Ardından “**fdisk -l**” ve “**multipath -l**” komutları ile işlemin doğruluğunu kontrol edebilirsiniz.
