---
layout: post
title: Installing Rancher on a Single Node Using Docker with SSL
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/25-26-20-uelinuxegitim/25-26-20le05.png
#share-img: /assets/img/path.jpg
tags: [rancher, docker, container, docker, kubernetes, ssl]
comments: true
---

**Kubernetes** ortamlarınmızı **UI** üzerinden yönetmek üzere **Rancher**'ı, **tek node**, **docker** ve **trusted** bir sertifika ile ayağa kaldıracağız.

Ortamda **docker container** kurulumunun yapıldığını var sayarak aşağıdaki komut satırını yürütmeniz yeterlidir.

**NoT**: **cert.pem** **full chain** olarak ele almayı unutmayın yani hem domain sertifikanın hemde **root** ve varsa **ara root** sertifikalarınız aynı dosyada **bundle** olması gerekmektedir.

Aslında ayrı **volume** `-v /<CERT_DIRECTORY>/<CA_CERTS.pem>:/etc/rancher/ssl/cacerts.pem` kullanarak da ayrı dosya olarak **up** edebilirsiniz.

Biz **bundle** olarak ele aldık.

~~~
docker run -d --name=rancher-server --restart=unless-stopped -p 80:80 -p 443:443 -v $PWD/ssl/cert.pem:/etc/rancher/ssl/cert.pem -v $PWD/ssl/key.pem:/etc/rancher/ssl/key.pem --privileged rancher/rancher:latest --no-cacerts
~~~

Bir diğer yöntem de aşağıdaki komutta yer almaktadır. Bir çok parametreyi aktif ederek ele aldık. En önemlisi **rancher data**'larını kendi **host**'unuzda saklayarak up etmek isteyebilirsiniz. Bu durum size birçok kolaylık getirecektir.
~~~
docker run -d --name=rancher-server --restart=unless-stopped -p 80:80 -p 443:443 -v $PWD/ssl/cert.pem:/etc/rancher/ssl/cert.pem -v $PWD/ssl/key.pem:/etc/rancher/ssl/key.pem -v $PWD/rancher_data:/var/lib/rancher -v $PWD/rancher_data/auditlog:/var/log/auditlog -e SSL_CERT_DIR="/container/certs" -e CATTLE_TLS_MIN_VERSION="1.0" -e AUDIT_LEVEL=1 --privileged rancher/rancher:latest --no-cacerts
~~~

Daha sonra **up** ettiğiniz **rancher**'a **web** üzerinden bağlanabilmek için default **admin** kullanıcısının şiresini aşağıdaki komut satırıyla elde etmeniz gerekmektedir.
~~~
docker logs  rancher-server  2>&1 | grep "Bootstrap Password:"
~~~

ref: [Rancher](https://docs.ranchermanager.rancher.io/pages-for-subheaders/rancher-on-a-single-node-with-docker#option-c-bring-your-own-certificate-signed-by-a-recognized-ca)