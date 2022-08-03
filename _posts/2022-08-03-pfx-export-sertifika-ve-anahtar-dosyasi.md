---
layout: post
title: PFX(*.pfx) Dosyasından Sertifika ve Anahtarını Çıkarma İşlemi
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/25-26-20-uelinuxegitim/25-26-20le05.png
#share-img: /assets/img/path.jpg
tags: [pfx, sertifika, cert, key, openssl, certificate]
comments: true
---

*** Domain sertifikasını(certificate) çıkartma,
~~~
openssl pkcs12 -in [dcertificate.pfx] -clcerts -nokeys -out [dcertificate.crt]
~~~

*** Sertifika Anahtarını(certificate key) çıkartma,

Key dosyasını çıkartmak için *.pfx dosyasını oluşturduğunuz anahtar çiftini soracaktır, akabinde sizin koruma oluşturmanız için şifre soracaktır.
~~~
openssl pkcs12 -in [dcertificate.pfx] -nocerts -out [dcertificate-encrypted.key]
~~~

Certificate key dosyasının(yukarıda verdiğiniz anahtar çifti) şifresini çözmek(decrypt) için,
~~~
openssl rsa -in [dcertificate-encrypted.key] -out [dcertificate-decrypted.key]
~~~

