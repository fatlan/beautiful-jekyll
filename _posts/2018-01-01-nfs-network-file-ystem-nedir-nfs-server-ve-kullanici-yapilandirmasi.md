---
layout: post
title: NFS(Network File System) nedir/NFS Server ve Kullanıcı Yapılandırması
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [nfs, storage, linux, mount]
comments: true
---
**Sun Microsystems** tarafından geliştirilen **Unix/Linux** dosya paylaşım servisi olup, **Linux** sistemler arasında dosya paylaşamını sağlayan ve stabil çalışan bir teknolojidir. Tabi ki açık kaynak kodludur deyip bu kadar teorik bilgide noktalasak yeter herhalde.

Hemen yapılandırma ve kullanımına girmeliyiz. Ortamda Sunucu ve İstemci modeli ile çalışacağız. Yani Hem Server tarafında yapmamız gerekenler var hemde bunu client'ların kullanabilmesi için yapmamamız gerekenler var. Bunun için sanal ortamımı oluşturdum aşağıdaki gibi.

![Crepe](assets/img/lin-nfs-con/lin-nfs-c01.png)

**NFS Server**’ımın ip bilgisi aşağıdaki gibidir.

![Crepe](assets/img/lin-nfs-con/lin-nfs-c02.png)

**NFS Client** için de ip bilgisi aşağıdaki gibidir.

![Crepe](assets/img/lin-nfs-con/lin-nfs-c03.png)

Herşeyden evvel genelde sunucular minimal kurulum yapıldığından NFS paketlerini kurmak gerek.

**NOT** : Bu paketleri hem sunucuda hem istemcide kurmalısınız. Aşağıdaki komut ile bu işlemi yapabilirsiniz.

~~~
yum install nfs-utils nfs-utils-lib
~~~

Daha sonra servisin durumuna aşağıdaki komut ile bakın ve servisin hem çalışmadığını hemde **disabled** olduğunu görün.

~~~
systemctl status nfs-server.service
~~~

![Crepe](assets/img/lin-nfs-con/lin-nfs-c04.png)

![Crepe](assets/img/lin-nfs-con/lin-nfs-c05.png)

Şimdi servisi alttaki komutla çalıştırın.

~~~
systemctl start nfs-server.service
~~~

Daha sonra enabled durumuna çekin.

~~~
systemctl enable nfs-server.service
~~~

![Crepe](assets/img/lin-nfs-con/lin-nfs-c06.png)

![Crepe](assets/img/lin-nfs-con/lin-nfs-c07.png)

Şimdi son duruma bakalım ve bu kısımın sağlıklı bir şekilde tamamlandığını görelim.

![Crepe](assets/img/lin-nfs-con/lin-nfs-c08.png)

![Crepe](assets/img/lin-nfs-con/lin-nfs-c09.png)

***Server – Sunucu tarafı**

Şimdi sunucuda yapılandırmaya başlayalım. Tabi burada sizin ortamınıza göre planlama yapmanız gerekiyor. Yani **NFS** için ayıracağınız alanı belirleyip bunun için hususi ayrı bir **disk**, **storage** de kullanabilirsiniz ki bu yöntem her zaman daha sağlıklıdır. Ben sunucu tarafında belirlediğim disk alanına **MePaylasNFS** diye bir dizin oluşturuyorum.

**Mkdir** komut ile **MePaylasNFS** klasörünü oluşturuyorum. Ardından **cd** komutu ile içine girip test için dosya ve klasörler oluşturuyorum.

~~~
mkdir MePaylasNFS
~~~

![Crepe](assets/img/lin-nfs-con/lin-nfs-c10.png)

Paylaşım yapılandırmayı “**/etc/exports**” **config** dosyası yardımıyla gerçekleştireceğiz. Herhangi bir editör yardımıyla buraya girip oluşturduğumuz **MePaylasNFS** klasörünü herkes için(**“*”** işareti herkes bağlanıp kullanabilir manasına gelir, eğer belirli makinalar için yapacaksanız “*****” yerine makinanın ip’ni girmelisiniz). Ardından **nfs** servisini yeniden başlatın.

![Crepe](assets/img/lin-nfs-con/lin-nfs-c11.png)

~~~
systemctl restart nfs-server.service
~~~

“**exportfs**” komutu yardımıyla sunucudaki tüm paylaşımlarımızı listeleyebiliriz.

~~~
exportfs
~~~

![Crepe](assets/img/lin-nfs-con/lin-nfs-c12.png)

Birde “**showmount -e localhost**” komutunu çalıştırıp, burdan da paylaşımları gözlemleyelim.

![Crepe](assets/img/lin-nfs-con/lin-nfs-c13.png)

***Client – Kullanıcı tarafı**

Şimdi **client** tarafında yapılandırmaya girişelim. Paylaşımı kullanacak tarafta paylaşılan alanı **mount** edeceğiz. **Mount** için bir hedef yani klasör oluşturalım. Ben **NFSKullan** adında bir klasör oluşturuyorum.

~~~
mkdir NFSKullan
~~~

Şimdi aşağıdaki gibi sunucunun ip(192.168.2.250) bigisiyle **mount** edelim.

~~~
mount 192.168.2.250:/ MePaylasNFS /NFSKullan
~~~

Direk **mount** komutu ile durumu gözlemleyebilirsiniz. Ayrıca **NFS** servisinin de versiyon 4 olduğunu görün.

~~~
mount
~~~

![Crepe](assets/img/lin-nfs-con/lin-nfs-c14.png)

**df -h** komutu ile gözlemleyelim.

~~~
df -h
~~~

![Crepe](assets/img/lin-nfs-con/lin-nfs-c15.png)

Paylaşım noktasını **ls -l** ile listeleyelim, verileri kullanabildiğimizi gözlemleyelim. Bu bilgiler aslında sunucuda.

~~~
ls -l /NFSKullan
~~~

![Crepe](assets/img/lin-nfs-con/lin-nfs-c16.png)

Son kontrol amacı ile “**showmount -e 192.168.2.250(Server ip bilgisi)**” komutunu çalıştırıp değişimi gözlemleyelim.

~~~
showmount -e 192.168.2.250
~~~

![Crepe](assets/img/lin-nfs-con/lin-nfs-c17.png)

Herşey bittimi.? Tabi ki HAYIR.! Çünkü bu kullanıcı makinası **reboot** olduğunda **mount** bilgisi kaybolacak ve **NFS** paylaşımını kullanamayacak. Bunu sabit kılmak ve sürekli kullanıcı makinası **reboot** olsa da kullanabilir yapmak için “**/etc/fstab**” dosyasına **mount** noktasını yazacağız. Herhangi bir editör yardımıyla bu yapılandırma dosyasına girelim ve aşağıdaki şekilde yapılandıralım(boşluklar **tab** tuşuyla oluşturuldu), kaydedip çıkalım.

~~~
# 192.168.2.250:/MePaylasNFS    /NFSKullan  nfs4    rw,sync 0 0
~~~

![Crepe](assets/img/lin-nfs-con/lin-nfs-c18.png)
