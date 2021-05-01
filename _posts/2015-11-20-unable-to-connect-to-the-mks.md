---
layout: post
title: Unable to connect to the MKS
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [virtualization, vmware]
comments: true
---
Bilindiği üzere **VMware vSphere Client** ile consol aracılığı ile sanal makinaları yönetip gerekli işlemleri yapabiliyoruz. Fakat **MKS(Mouse Keyboard Screen)** denen bir çeşit hata ile karşı karşıya kalma ihtimaliniz olabilir. Nedir bu hata derken şöyle başlayalım **MKS** ile devamında bir kaç çeşit hata ile dönebilir. Aşağıda screenshot ile paylaştığım gibi yada simyah ekranda bekleyip kalıyor ve sistem arayüzü gelmiyor. Sorunun nedeni bir çok neden olarak algılayabilirsiniz fakat genelde ana neden **DNS** sorunudur. Yani çalıştığınız makina **VMware ESX** ya da **ESXi** hostlarınızın ismini çözemediğinden kaynaklanıyor.

Genelde dönen hatalar aşağıdaki gibidir.

- Error connecting: Host address lookup for server <SERVER> failed: The requested name is valid and was found in the database, but it does not have the correct associated data being resolved for Do you want to try again?
- Error connecting: cannot connect to host <host>: A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host has failed to respond. Do you want to try again?
- Error connecting: You need execute access in order to connect with the VMware console. Access denied for config file.
- Unable to connect to MKS: failed to connect to server IP:903.

![Crepe](/assets/img/utocmks/utocmks01.png)

![Crepe](/assets/img/utocmks/utocmks02.png)

Sorunun en basit çözümü olarak hostlarınıza(**FQDN**) ve **VCenter** sunucularınıza karşılık gelen **IP** adreslerini **HOST** dosyası içine yazarak giderebilirsiniz. Ben bu yöntemle sorunu çözdüm tabi onaylı bir yöntem olmamakla beraber yapılan bu işlem sadece o makina için geçerli olacaktır. **Vmware** bununla alakalı yayınladığı **KB** leri aşağıda paylaşıyor olacağım.

![Crepe](/assets/img/utocmks/utocmks03.png)

192.168.2.20 esxhost1
192.168.2.21 esxhost1.yourdomain gibi...

Knowledge Base;

[http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=749640](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=749640) <br>
[http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2046356](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2046356) <br>
[http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2115126](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2115126) <br>
[http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2032016](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2032016) <br>
[http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=749640](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=749640) <br>