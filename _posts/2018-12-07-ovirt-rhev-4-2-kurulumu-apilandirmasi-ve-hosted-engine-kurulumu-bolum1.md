---
layout: post
title: OVİRT(RHEV) 4.2 Kurulumu, Yapılandırması ve HOSTED ENGINE Kurulumu - Bölüm 1
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [ovirt, rhev, linux, virtualization]
comments: true
---
![Crepe](/assets/img/ovirt42-hosted-engine/ovir-he-ins01.png)

RHEV(Red Hat Enterprise Virtualization) yada Ovirt Açık Kaynak Kodlu KVM temelinde hypervisor olarak kullanılan, ölçeklenebilen sanallaştırma platformudur. İki ürünün temeli aynı fakat Ovirt [https://ovirt.org/](https://ovirt.org/) RHEV’in forklanmış hali olup tamamen ücretsizdir.

Konuyu tamamen adım adım ele alan ve bölümlerden oluşan makale serisi ile baştan sona böyle bir sanal bulut ortamını elde edebilmek için **Ovirt** yapısını inceleyeceğiz. İşlemlere başlamadan önce istediğimiz yapının bir çizgisini yukarıda paylaşıyorum. Kafanızda durum şekillenmesi için çizgiyi paylaştım, siz daha basit yapıda da kurabilirsiniz. Örneğin tek **nic** yada **internal disk** gibi.

    Node1: 192.168.22.10
    Node2: 192.168.22.20
    HostedEngine: 192.168.22.15

gibi, **engine** için ayrı management ip vermek gerekiyor. Çünkü yönetimi engine aracılığı ile yapacağız, bir nevi **vcenter** gibi düşünebilirsiniz.

Ovirt’i iki şekilde kurabiliriz.

1.Manuel olarak terminal üzerinden ilk önce CentOS kurup, ardından gerekli paketleri indirip daha sonra da barematel üzerine engine setup ederek yapabilirsiniz.

Manual kurulum linkleri :

[https://ovirt.org/download/](https://ovirt.org/download/) <br>
[https://www.ovirt.org/documentation/install-guide/Installation_Guide/](https://www.ovirt.org/documentation/install-guide/Installation_Guide/) <br>
[https://www.ovirt.org/documentation/install-guide/chap-Installing_oVirt/](https://www.ovirt.org/documentation/install-guide/chap-Installing_oVirt/) <br>

2.**Ovirt** için hazırlanmış özelleştirilmiş **iso** paketi ile kurulum gerçekleştirebilirsiniz ki **CentOS** sunucunun özelleştirilmiş halidir ve **repoları** ona göre otomatik yapılandırılmış gelir, ayrıca **hosted engine vm** olarak kurulup yönetim sağlanmaktadır. Biz bu yöntemi kullanarak kurulum gerçekleştireceğiz.

İso paket linki : [https://resources.ovirt.org/pub/ovirt-4.2/iso/ovirt-node-ng-installer-ovirt/4.2-2018100407/](https://resources.ovirt.org/pub/ovirt-4.2/iso/ovirt-node-ng-installer-ovirt/4.2-2018100407/)

İso dosyasını **host**’lara mount ettikten sonra aşağıdaki karşılama ekranından “**Install oVirt Node**” diyerek işleme devam edelim.

![Crepe](/assets/img/ovirt42-hosted-engine/ovir-he-ins02.png)

Aşağıda görüldüğü üzere normal **CentOS** kurulum gibi Anaconda ekranı, o yüzden buranın üzerinde fazla durmayacağım. Bu bölümün kurulumunu yapıp sunucuyu **reboot** edin.

![Crepe](/assets/img/ovirt42-hosted-engine/ovir-he-ins03.png)

**NoT1** : Kurulum bittikten sonra gerekli erişimler için hostların **network** yapılandırmasını yapmalısınız. **Update**’ni geçebilirsiniz. Bu arada “**yum**” ile paket kurmak istediğinizde hata alabilirsiniz. Çünkü **repolar** **Ovirt** için özelliştirilmiş olacaktır. Onun dışındaki **repolar disable** gelecektir. Bu yüzden paketlerde kurulum yapamazsanız “**yum**” komutunu aşağıdaki gibi kullanabilirsiniz. Tek seferlik paket kurulumu için **enable modu**.

Ayrıca “**/etc/hosts**” ve “**/etc/resolv.conf**” dosyalarını yapılandırıp, **search domain.com** ibaresinide ekleyin. **DNS**, **SSH Keypair** ve **NTP**(ntp olarak chrony servisi kullanılmaktadır, yapılandırma dosyasına ilgili **ntp server**’ı girdikten sonra servisi yeniden başlatmak yeterlidir) yapılandırmalarının yapılmış olması gerekmektedir.

~~~
yum install --enablerepo=”base” CentOS-Base.repo “paketadı” -y
yum install --enablerepo=”epel” epel.repo “paketadı” -y
~~~

**NoT2** : **Hosted Engine** kurulumunu terminal ekranından da yapabilirsiniz. Fakat biz **GUI** kullanarak yapacağız. Bu arada **HostedEngine** **storage** üzerine kurmak zorundasınız. Komut satırından kurulum linki : [https://ovirt.org/documentation/how-to/hosted-engine/](https://ovirt.org/documentation/how-to/hosted-engine/)

Devam edelim. Sunucu **reboot** olduktan sonra aşağıdaki paketi terminal ekranından kurun. Daha sonra **GUI** ekranında fazlaca beklememiş olursunuz.

~~~
yum install ovirt-engine-appliance -y
~~~

**Hosted Engine** kurulumuna geçmeden önce **storage** ayarlarını yapmamız gerekiyor. **Host**’ta saklayacağımız **vm** ler için **iscsi** protokolünü kullarak **data01** ve **data02** adında iki adet **lun** bağlayacağız, daha sonra **HostedEngine** bağlanacak. **Ovirt**’in istediği, **nfs protokolü** ile de **iso** ve **export** adında iki adet **volume** **HostedEngine** bağlayacağız.** Nfs**’leri **host**’a bağlamak zorunda değilsiniz. Hatta bağlamayın çünkü **Nfs**’ler HostedEngine bağlanacak. Dikkat etmeniz gereken **Nfs**’ler için yazma ve okuma haklarının **unix** için tam olması gerekiyor yoksa **HostedEngine** bağlanma sırasında “**permission**” hatası alırsınız.

Konu ile alakalı cause linki : [https://support.hpe.com/hpsc/doc/public/display?docId=mmr_kc-0108935](https://support.hpe.com/hpsc/doc/public/display?docId=mmr_kc-0108935)

**ISCSI**’leri terminal üzerinden bağlayacağız çünkü **UI** tarafında bu işlem yapılamıyor ama daha sonra **node**’lerin gui’sinde görüntülüyebileceksiniz. Aslında bu protokollerin dışında **FC(fiber channel)** yada **Gluster storage** protokolleri de kullanılabilir ama şuan için bizim konumuz dışında.

**İscsi yapılandırması:**

**Storage** tarafında ilgili **SVMs**, **LUNs**, **Volumes** ve **network** ayarlarını yapıp **data01**, **data02**, **iso** ve **export** bölümlerini açtıktan sonra **iscsi**’nin hostlara erişimi için “**Initiators**” ve “**Initiator Group**” erişimlerini vermelisiniz. **Storage** tarafına detaylıca girmiyorum konu genişleyip, dağılmaya başlar.

**Not3** : Sunucunun “**InitiatorName**” elde etmek için aşağıdaki komutu kullanabilirsiniz.

~~~
cat /etc/iscsi/initiatorname.iscsi
~~~

**İscsi** hedef tespiti için aşağıdaki komut ile **discover** işlemini yapalım. İlgili ip’lere bağlı erişimi olan tüm hedefleri listeleyecektir.

~~~
iscsiadm -m discovery -t st -p 10.10.30.5
~~~

Çıktı: 10.10.30.5:3260,1030 iqn.1992-08.com.netapp:sn.2626a89cce1111e66c5200a098d9e8ba:vs.18

Şimdi var olan tüm **iscsi** hedeflerine **login** olmak için aşağıdaki komutu çalıştırın.

~~~
iscsiadm -m node -l
~~~

Ardından “**fdisk -l**” ile bağlanan diskleri görüntüleyebilirsiniz.

Daha sonra da “**multipath -l**” ile de **iscsi** ile takılan disklerin **multipath** bağlantısını görüntüleyebilirsiniz.

**Not4** : **Host** tarafında **ISCSI** bağlantıları için yaptığınız **bond**, **vlan** ve diğer **network** yapılandırmalarından sonra, **NetworkManager**’ı kapatmanız gerekmektedir. Aksi halde **Engine** tarafında hatalar alıp, **nic** kopmalarıyla beraber, ip’nizin kaybolması gibi durumlarla karşılaşabilirsiniz.

**Host**’ta **Datastorage**’de işlemleri tamamladıktan sonra **HostedEngine** kuruluma geçelim, bunun için tabi ilk önce **Ovirt host**’a **login** olalım.

Kurulum ekranında **network** kısmında management için verdiğiniz ip’yi tarayıca da **9090** ile çağırıp işlemlere devam edelim. Örnek olarak 192.168.22.10:9090 yada domain name gibi...

Bu ekran hostun yönetimi için açılan web panelidir. Sunucuyu konsol yerine burdan **UI** üzerinden rahatça yönetebilirsiniz. Biz **HostedEngine** kurulumunu bu ekrandan yapacağız. Bu ekrana sunucuya verilen kullanıcı adı ve şifresi ile login olacaksınız.

![Crepe](/assets/img/ovirt42-hosted-engine/ovir-he-ins04.png)

Aslında bu **GUI** ekranında terminalde yaptığınız birçok işlemi gerek kalmadan görsel ekranda da yapabilirsiniz. **Iscsi** bağlantınızı **host**’larda gerçekleştirdikten sonra bu ekranda da görünecektir, etkileşimli olarak yapılan işlemler yansıyacaktır.

**Ovirt node**’ye **login** olduktan sonra **Storage** ile çalışacağımız için **Virtualization - Hosted Engine** sekmesine gidip **start** veriyoruz. **Hyperconverged** sekmesi ile şuan ilgilenmiyoruz çünkü bu kısım **internal diskler** ile **glusterfs** le sistemi yedekli **node**’ler ile kullanmaktadır. **Gluster** için; **OS**'in disklerini **Raid 1**, **Gluster**‘ın kullanacağı disklerini **Raid 5** olarak iki **Raid** grup yapabilirsiniz.

![Crepe](/assets/img/ovirt42-hosted-engine/ovir-he-ins05.png)

Bu kısmda hosted engine için yukarıda belirdediğimiz ip’ye karşılık gelen domaini dns sunucumuzda daha önceden tanımlamıştık. Örnek bulut.fatlan.com gibi.

**Engine VM FQDN** kısmına o domaini yazıyoruz.

Belirlenen ip’yi **dhcp**’den ya da **static** olarak verip, **gateway** ve **dns** bilgilerini de giriyoruz.

**Manage** olacak **bridge interface**’mizide seçtikten sonra **host**’un **root pass** bilgisini giriyoruz.

En son olarak **virtual CPU** ve **Memory** kısımların da **default**’ta bırakıp **Next** ile devam edelim.

![Crepe](/assets/img/ovirt42-hosted-engine/ovir-he-ins06.png)

Bu kısımda ki **Engine Credentials** kısmı önemli. Çünkü Bu kısımdan **HostedEngine login** olabileceğiniz şifreyi belirliyorsunuz.

**Notification** kısmı şuan için çok zaruri değil, **defaultta** bırakabilirsiniz daha sonra da değiştirebilirsiniz.

![Crepe](/assets/img/ovirt42-hosted-engine/ovir-he-ins07.png)

Ardından **config**’ler hazır halde **Prepare VM** demenizi beklemektedir. Tıklayıp devam edin.

![Crepe](/assets/img/ovirt42-hosted-engine/ovir-he-ins08.png)

**NoT4** : **HostedEngine** atadığınız ip **gateway**’ine **ping** atabilmelidir ve **network**’sel erişimde kısıtlı olmamalıdır aksi halde aşağıdaki ss’lerden belirttiğim hataları alırsınız ve çok sizi çok uğraştırır.

**Terminalde;**

![Crepe](/assets/img/ovirt42-hosted-engine/ovir-he-ins09.png)

**GUI’de;**

![Crepe](/assets/img/ovirt42-hosted-engine/ovir-he-ins10.png)

**Prepare VM** dedikten sonra aşağıdaki gibi başarılı bir kurulum uyarısı alacaksınız, **Next** ile devam edin.

![Crepe](/assets/img/ovirt42-hosted-engine/ovir-he-ins11.png)

Şimdi **Engine**’nin barınacağı **datastore** için **iscsi** ayarlarını yapmamız gerekiyor. Diğer **store**’leri **engine** kurulduktan sonra yapacağız.

**Storage type iscsi** seçip,

Portal ip address kısmına **iscsi** hedefimizin ip adresini yazıyoruz, eğer hedefinizde user ve şifre varsa onu da girin bende olmadığı için girmiyorum.

**Ardından Retrieve Target List** butonuna tıklayın.

![Crepe](/assets/img/ovirt42-hosted-engine/ovir-he-ins12.png)

**Retrieve Target List** butonuna bastıktan sonra aşağıdaki gibi hede eriniz gelecektir. Seçiminizi yapıp **Next** ile devam edin.

![Crepe](/assets/img/ovirt42-hosted-engine/ovir-he-ins13.png)

Ardından aşağıdaki gibi yeşil **Finish** ekranı sizi karşılıyorsa **Hosted Engine** kurulumu başarılı bir şekilde gerçekleştirildi demektir.

![Crepe](/assets/img/ovirt42-hosted-engine/ovir-he-ins14.png)

Tekrar **Hosted Engine deploy** ettiğimiz **Ovirt Node** ekranına dönecek olursak aşağıdaki gibi **deploy** işleminin başarılı ve sunucunun çalıştığını anlayabiliriz.

![Crepe](/assets/img/ovirt42-hosted-engine/ovir-he-ins15.png)

Şimdi kurduğumuz **hosted engine login** olalım. **Default** kullanıcı adı “**admin**”, **password**'de **deploy** sırasında verdiğiniz şifre olacak.

![Crepe](/assets/img/ovirt42-hosted-engine/ovir-he-ins16.png)

![Crepe](/assets/img/ovirt42-hosted-engine/ovir-he-ins17.png)


Artık bundan sonra ki işlemlerimize **hosted engine**’de devam edeceğiz. **Datacenter** ekleme, **Cluster** ekleme, **Host** ekleme, **Storage** ekleme, **Vm** oluşturma gibi vesaire işlemleri bu ekranda yapacağız.
