---
layout: post
title: OpenSSL ile Online Olarak Çalışan Servisin Sertifikasını Elde Etme
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/25-26-20-uelinuxegitim/25-26-20le05.png
#share-img: /assets/img/path.jpg
tags: [openssl, certificate, key, ssl, tls]
comments: true
---

~~~
openssl s_client -showcerts -connect proxy.fatlan.com:465 </dev/null 2>/dev/null|openssl x509 -outform PEM > downloaded_cert.pem
~~~
