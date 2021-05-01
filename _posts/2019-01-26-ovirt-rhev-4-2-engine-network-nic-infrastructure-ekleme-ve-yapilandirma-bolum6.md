---
layout: post
title: Ovirt/RHEV 4.2 Engine Network(NIC) Infrastructure Ekleme ve Yapılandırma - Bölüm 6
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [ovirt, rhev, linux, network, virtualization]
comments: true
---
İlk bölümdeki görselde, ağ alt yapısını tasarlamıştık ve bu tasarımda network alt yapısını da belirtmiştik. O bölümü hatırlatarak işlemlere devam edelim.

Tasarımda 4 ayrı NIC kullandık;

1. Network interface “management”
2. Network interface “storage”
3. Network interface “hostnet”
4. Network interface “migration”

“**managemet**” **interface**’sini sunucuları kurarken, yönetmek için atamıştık zaten. Ardından sunucuların **storage** erişmesi, **iscsi** ve **nfs** kullanacağımız için nic’in birini de **host**’ta “**storage**” **interface**’sini tanımlayıp erişimlerini yaptık ve **storage** diskleri ekledik. Yapımızda **iscsi** ve **nfs** **vlan** ağında olduğundan ziksel **interface**’ye “**storage_iscsi**” ve “**storage_nfs**” adından iki mantıksal **vlan** **interface**’yi oluşturduk. Aslında **nfs**’i **vlan** olarak Engine tarafında da yapabilirdik fakat **iscsi** kısmı yapmak zorundayız ki **storage**’deki **lun**‘ları **host**’lara disk olarak ekleyebilelim ve **Engine**’yi kurabilelim. **Engine** **host** üzerinde yapılandırılan networkler üzerinden de erişim sağlayabilir. Bu arada **host** tarafında **nic**’leri **bonding**, **vlan** ve **bridge** olarak’ta tanımlayıp kullanabilirsiniz. Aynı şekilde **Hosted** **Engine**’de de **bonding**, **vlan** yapabiliriz.

“**hostnet**” ve “**migration**” **nic**‘lerini **Hosted Engine** tarafında şimdi beraber yapılandıracağız. “**hostnet**” **VM**’lerin internete çıkışı ve birbiriyle iletişimi için, “**migration**”da Vm makineleri hostlar arasında taşıma/göç ettirme için kullanacağız.

**Önemli NOT** : **Bonding** hakkında. **Bonding** yapılandırması host/sunucu tarafında ayrı, **Hosted Engine** tarafında ayrı fiziksel **NIC**’ler için yapılandırmalıdır. Çünkü host tarafında yapılandırılan **bonding**’i **Engine** görebiliyor ve gösterebiliyor fakat eklediğiniz mantıksal **interface**’leri atama yaptığınızda kullanmanıza izin vermeyip, eklerken hata veriyor. Kısaca ilgili **NIC**’ler için host tarafında yapılandırma olduğu için kullanmanıza izin vermiyor. Bu yüzden **Engine**’de **bonding** kullanacaksanız ki **Engine**’de sağ klik ile çok basit bir şekilde eklenebiliyor, ilgili ziksel **nic**’lere host tarafında herhangi bir yapılandırma yapılmamış olması gerekiyor. Ben başka bir yapılandırma için iki ziksel **nic**’i **Engine** tarafında **XOR** protokolü kullanarak çok basit bir şekilde yapılandırarak, esnek bir yapı elde ettim.

Şimdi “**hostnet**” ve “**migration**” **nic**‘lerini **Hosted Engine **tarafında tek tek yapılandırıp kullanıma hazırlayalım.

Önce mantıksal **nic**’lerimizi ekleyelim. Biz ilk önce “**hostnet**”i ekleyip, aktif edeceğiz. Sonra “**migration**” interfacesini ekleyip, aktif edeceğiz. Bunun için **Engine** login olduktan sonra “**Network/Networks/New**” diyerek ekleme işlemine başlıyoruz.

![Crepe](/assets/img/ovirt42-nics-conf/ovirt42-nic-co01.png)

Ardından ilgili **Data Center**’ı seçip “**Name**” kısmına interface ismini verin. **Network Parameters** kısmında yer alan bilgilerde size uygun olan bilgileri de girmeniz gerekiyor. Biz “**hostnet**”i **VM**’lerin çıkış **interface**’si olarak kullanacağımızdan “**VM network**” parametresini enable ettik. Belirlediğimiz subnet herhangi bir **Vlan**’a dahil olmadığı için **Vlan tag**’ini enable işaretlemedik. Sizin belirlediğiniz **subnet switch**’leriniz de vlan dahilinde ise bu kısımda **vlan** numarasını belirlemelisiniz. Son olarak **OK** tıklayıp, ekleme işlemini bitirin.

![Crepe](/assets/img/ovirt42-nics-conf/ovirt42-nic-co02.png)

Ve oluşturduğunu aşağıdaki gibi görelim. Henüz var olan tüm **host**‘larımıza tanıtmadığımız için aktif değil ve bu yüzden kırmızı görünüyor.

![Crepe](/assets/img/ovirt42-nics-conf/ovirt42-nic-co03.png)

Şimdi sırasıyla “hostnet”i var olan host’larımıza tanıtalım ve aktif etmiş olalım.

“**Compute/Hosts**” sekmesine geldikten sonra ilk **host**’umuzun üzerine tıklayıp ayrıntılar kısmına girelim. Oradan “**Network Interfaces**” sekmesine gelip “**Setup Host Networks**” butonuna tıklayın.

![Crepe](/assets/img/ovirt42-nics-conf/ovirt42-nic-co04.png)

Ardından aşağıdaki ss’den de anlaşılacağı üzere sağdaki **Logical Networks**’te görünen “**hostnet**” **nic**’ini sürükleyip, belirlediğiniz ilgili ziksel **nic**’e bağlayın.

![Crepe](/assets/img/ovirt42-nics-conf/ovirt42-nic-co05.png)

Yukarıdaki siyah kutucukta görünen kalem işaretini tıkayın ve aşağıdaki gibi ip yapılandırma penceresine gelin, ip yapılandırdıktan sonra,

![Crepe](/assets/img/ovirt42-nics-conf/ovirt42-nic-co06.png)

**DNS Configuration** sekmesine gelin ve dns ip’sini verdikten sonra **OK** tıklayın.

![Crepe](/assets/img/ovirt42-nics-conf/ovirt42-nic-co07.png)

Şimdi aynı işlemi ikinci **host**’umuza yapalım, bizde toplamda iki host vardı siz bütün hostlarınızda yapmalısınız. Şimdi ikinci **host**’umuzda da “**Setup Host Networks**” butonuna tıklayın.

![Crepe](/assets/img/ovirt42-nics-conf/ovirt42-nic-co08.png)

Aynı şekilde **fiziksel nic**’e, **logical nic**’i bağlayın.

![Crepe](/assets/img/ovirt42-nics-conf/ovirt42-nic-co09.png)

**Kalem işaretini** tıklayıp **ikinci** **host** için **ip** yapılandırmasını yapalım.

![Crepe](/assets/img/ovirt42-nics-conf/ovirt42-nic-co10.png)

**DNS** yapılandırmasını yapıp, **OK** tıklayalım.

![Crepe](/assets/img/ovirt42-nics-conf/ovirt42-nic-co11.png)

Ve “**Compute/Cluster/Default/Logical Network**” bölümünden **mantıksal nic**’lerimizi listelediğimizde “**hostnet**”in yeşil yandığını yani aktif olduğunu görebiliyoruz.

![Crepe](/assets/img/ovirt42-nics-conf/ovirt42-nic-co12.png)

Şimdi sıra geldi ikinci **mantıksal interface**’miz olan “**migration**” **nic**’ine. Aynı şekilde “**Network/Networks/New**” diyerek aşağıdaki gibi **nic**’i oluşturalım. Bu kısımda “**VM network**” **enable** sekmesini tıklamıyorum çünkü bu **interface**’yi **VM**’ler çıkış için kullanmayacak. Tabanda **Engine VM**’leri migration yapmak için bu **nic**’i kullancağı için “**Name**” belirleyip **OK** tıklıyorum.

![Crepe](/assets/img/ovirt42-nics-conf/ovirt42-nic-co13.png)

Tekrar “**Compute/Cluster/Default/Logical Network**” listelediğimizde “**migration**” **nic**’inin geldiğini fakat kırmızı yani aktif olmadığını görelim.

![Crepe](/assets/img/ovirt42-nics-conf/ovirt42-nic-co14.png)

Yukarıdaki ss’lerde gösterdiğimiz gibi yapınızdaki tüm **host**’ların tek tek “**Setup Host Networks**” bölümünden, “**migration**” interface’sini ziksel **interface**’ye bağladıktan sonra aşağıdaki gibi aktif olacaktır.

**NoT2** : “**migration**” **interface**’si için ip yapılandırması **YAPMIYORUZ** yani ip vermiyoruz.

![Crepe](/assets/img/ovirt42-nics-conf/ovirt42-nic-co15.png)

Aslında şu anda **nic** ekleme işleminiz tamamlandı fakat bu **nic**’ler için **roller** atayacağız. Bu **roller** ilgili **logical nic**’lere belirli özel iş yükleri atayacak. Şöyle ki “**Compute/Cluster/Default**” bölümünden “**Logical Networks**” sekmesine gelin ve aşağıdaki görselden de anlaşılacağı üzere ”**Manage Networks**” tıklayın.

![Crepe](/assets/img/ovirt42-nics-conf/ovirt42-nic-co16.png)

Ardından aşağıdaki ss’den de anlaşılacağı üzere **logical nic**’lerimize spesifik roller atayalım. **Migration Network rolü**nü “**migration**” **interface**’sine, **Default Route** olarak ben “**hostnet**” kullanmak istediğimden ona atıyorum, diğer iki **rol management interface**‘sinde kalabilir. **OK** deyip işlemleri bitiriyorum.

![Crepe](/assets/img/ovirt42-nics-conf/ovirt42-nic-co17.png)

Son olarak **nic**’leri “**Compute/Cluster/Default/Logical Network**” listelediğimde **nic**’lere atanmış rolleri de görebiliyorum. İşlem bu kadar.

![Crepe](/assets/img/ovirt42-nics-conf/ovirt42-nic-co18.png)

**NoT3** : Evet işlemlerimiz bitti fakat **nic**’lerimizin tüm **host**’larda sağlıklı bir şekilde çalıştığından emin olmak yani **sync** olduğunu görmek için “**Network/Networks/hostnet**” bölümü **Hosts** sekmesi ve “**Network/Networks/migration**” bölümü **Hosts** sekmesinden aşağıdaki ss’lerde işaretlediğim kırmızı alanda “**Out-of-sync**” kısmının aşağıdaki gibi tertemiz olması lazım, aksi halde zaten orada kırmızı bir işaretçi çıkıp size bir sorun olduğunu söylecektir. Böyle bir durumda network yapılandırmalarınızı gözden geçirin, eğer bir sorun yoksa “**Sync All Networks**” butonunu kullanın.

![Crepe](/assets/img/ovirt42-nics-conf/ovirt42-nic-co19.png)

![Crepe](/assets/img/ovirt42-nics-conf/ovirt42-nic-co20.png)


