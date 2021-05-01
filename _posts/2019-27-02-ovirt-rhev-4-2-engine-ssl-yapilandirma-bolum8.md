---
layout: post
title: Ovirt_RHEV 4.2 Engine SSL Yapılandırma – Bölüm 8
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [ovirt, rhev, redhat, centos, linux, virtualization]
comments: true
---
**Selfsign ssl** kullanan **oVirt**, aşağıdaki görselden de anlaşılacağı üzere **trusted** değil.

![Crepe](assets/img/ovirt42-ssl-con/ovirt42-ssl-co01.png)

Bunun yerine doğrulanmış, satın alınmış **ssl**’inizle değiştirebilirsiniz. Bunun için aşağıdaki adımları takip etmeniz yeterli. **Trusted ssl**’lerimizi “**/tmp/ssl/**” (ca, crt, key)dizini altına attık.

Satın alınmış **ssl ca**'yı “**/etc/pki/ca-trust/source/anchors/**” dizine kopyalayalım ve **update certificate** edelim.

~~~
cp /tmp/ssl/certi cate.ca certi cate.pem
update-ca-trust
~~~

Aşağıdaki **pem** dosyasını silebiliriz.

~~~
rm /etc/pki/ovirt-engine/apache-ca.pem
~~~

Satın alınmış **ssl ca**'yı “**/etc/pki/ovirt-engine/**” dizinine de kopyalayalım.

~~~
cp /tmp/ssl/certi cate.ca /etc/pki/ovirt-engine/apache-ca.pem
~~~

Eski **selfsign** sertifikalarının yedeğini alalım.

~~~
cp /etc/pki/ovirt-engine/keys/apache.key.nopass /etc/pki/ovirt-engine/keys/apache.key.nopass.bak
cp /etc/pki/ovirt-engine/certs/apache.cer /etc/pki/ovirt-engine/certs/apache.cer.bak
~~~

Satın alınmış **ssl key** sertifikasını “**/etc/pki/ovirt-engine/keys/**” dizinine atalım.

~~~
cp /tmp/ssl/certi cate.key /etc/pki/ovirt-engine/keys/apache.key.nopass
~~~

Aynı şekilde satın alınmış **ssl crt** dosyamızıda “**/etc/pki/ovirt-engine/certs/**” dizinine atalım.

~~~
cp /tmp/ssl/certi cate.crt /etc/pki/ovirt-engine/certs/apache.cer
~~~

Apache servisini yeniden başlatalım.

~~~
systemctl restart httpd.service
~~~

Şimdi aşağıdaki dosyaların içeriğine ilgili satırları ekleyin.

“**vi /etc/ovirt-engine/engine.conf.d/99-custom-truststore.conf**” **edit**‘leyin ve aşağıdaki satırları ekleyin.

~~~
- ENGINE_HTTPS_PKI_TRUST_STORE=”/etc/pki/java/cacerts”
- ENGINE_HTTPS_PKI_TRUST_STORE_PASSWORD=””
~~~

“vi /etc/ovirt-engine/ovirt-websocket-proxy.conf.d/10-setup.conf” dosyasını da edit‘leyin ve aşağıdaki satırları ekleyin.

~~~
- PROXY_PORT=6100
- SSL_CERTIFICATE=/etc/pki/ovirt-engine/certs/apache.cer
- SSL_KEY=/etc/pki/ovirt-engine/keys/apache.key.nopass
- #SSL_CERTIFICATE=/etc/pki/ovirt-engine/certs/websocket-proxy.cer
- #SSL_KEY=/etc/pki/ovirt-engine/keys/websocket-proxy.key.nopass
- CERT_FOR_DATA_VERIFICATION=/etc/pki/ovirt-engine/certs/engine.cer
- SSL_ONLY=True
~~~

Son olarak aşağıdaki servisleri yeniden başlatın.

~~~
systemctl restart ovirt-provider-ovn.service
systemctl restart ovirt-engine.service
~~~

Ve görünüm aşağıdaki gibidir.

![Crepe](assets/img/ovirt42-ssl-con/ovirt42-ssl-co02.png)
