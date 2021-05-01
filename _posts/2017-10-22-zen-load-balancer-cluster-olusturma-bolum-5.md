---
layout: post
title: Zen Load Balancer Cluster Oluşturma - Bölüm 5
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [zen, proxy, loadbalancer, linux]
comments: true
---
Bu bölümde servis kesintisi yaşamamak için kullanılan yöntem olarak istekleri karşılayıp yönlendiren **Zen Load Balancer** için de **High Available** yapacağız. Bu işlemi çok basit olarak **Cluster** yöntemi ile gerçekleştireceğiz.

Bunun için **interface** yapılandırması önemli. Şöyle ki, iki **loadbalancer** için yapılandırma yapacağız. Yani iki yük dengeleyici arasında kümeleme yapacağımız için,

Loadbalancer01;
~~~
eth0 10.10.10.100
eth0:cl 10.10.5
~~~

Loadbalancer02;
~~~
eth0 10.10.10.200
eth0:cl 10.10.10.5
~~~

Yukarıdaki yapılandırmada **100** ve **200** **ip**’leri sunuculara yönetim için atanmış olan direk erişim ipleridir, **5 nolu ip** ise **cluster** için kullanılacak olan ip’dir. Bu durumda **cluster** yapılandırmamız **100**’lü sunucuda erişim kesildiği zaman **200** lü sunucuya geçecektir. Bu geçiş **5** li ip tarafından sağlanacaktır. Yani **5** ip’li aktif kullanımda olacağı için **5**’li ip **100** ip’li sunucu **up** ise onun üzerine geçecektir, **100** ip’li sunucu **down** olursa **5** ip si **up** olan **200** ip’li sunucuya geçecektir. Bu durumda servis **5** ip’si ile hizmet verdiğinden servis kesintisi yaşanmayacaktır.

Şimdi yapılandırmaya geçelim. **Interface**’leri yukardaki senaryoya göre yapılandırdıktan sonra,

**Settings** kısmından **Cluster** sekmesine geçin,

![Crepe](/assets/img/zen-cluster-bolum5/zen-clus-b501.png)

Aşağıdaki gibi **Cluster Virtual IP 10.10.10.5 Local** ve **Remote Hostname roo**t şifresini(**RSA** iletişimi için) ve **IP** bilgileri girip, “**cluster type**” olarak **loadbalancer master** and l**oadbalancer02 backup automatic failback** seçip **Save** deyin, ardından **Test Connection** ile işlemleri kontrol edebilirsiniz.

![Crepe](/assets/img/zen-cluster-bolum5/zen-clus-b502.png)

Detaylı : Zen -> [Zevenet](https://www.zevenet.com/knowledge-base/community-edition/community-edition-v3-05-administration-guide/community-edition-v3-05-settings-cluster/#prettyPhoto)