---
layout: post
title: İfconfig Command Not Found - CentOS 7 Minimal Yüklemeler
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [centos, linux, network]
comments: true
---

![Crepe](assets/img/ifc-com-n-fou/if-comn01.png)

CentOS 7 minimal yükleme seçeneğinde sistemde **ifconfig** çalışır vaziyete gelmez. O yüzden bu hata alınır. Alternatif olarak **ip** komutu kullanılabilir. Aşağıda örnek belirtilmiştir.

~~~
ip addr
~~~

![Crepe](assets/img/ifc-com-n-fou/if-comn02.png)

**İfconfig** komutunu kullanabilmek için sisteme yüklemek gereklidir. Aşağıdaki komutu kullanarak bu işlemi gerçekleştirebilirsiniz.

~~~
yum install net-tools –y
~~~

![Crepe](assets/img/ifc-com-n-fou/if-comn03.png)

![Crepe](assets/img/ifc-com-n-fou/if-comn04.png)

Daha sonra **ifconfig** komutunu çalıştırabilirsiniz.

![Crepe](assets/img/ifc-com-n-fou/if-comn05.png)
