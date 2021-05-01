---
layout: post
title: Linux Sunucular Üzerinde Dns Recursion Özelliğini Kapatmak
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [linux, dns, security]
comments: true
---

**DNS Recursion** özelliğinin açık olması, “**DNS Amplification Attack**” saldırılara maruz kalmanız demektir. Bu atak **DNS** kuvvetlendirmeli **DDOS** saldırısı olarakta bilinir. Bu nedenle **DNS Recursion** özelliğinin kapatılması gerekiyor ve biz bu işlemi nasıl yapacağımıza bakacağız. **DDOS** saldırısının sonuçlarına az/çok aşina olduğunuzu varsayarak, **DNS Amplification Attack** nedir.? çok fazla ayrıntıda boğmadan durumu anlatan güzel bir videoyu [buraya](https://www.youtube.com/watch?v=xTKjHWkDwP0 (https://www.youtube.com/watch?
v=xTKjHWkDwP0)) bırakıyorum.

Şimdi ilgili **DNS** sunucusunda **Recursion** özelliği açık/kapalı mı.? Herhengi bir kullanıcı makinasından sorgu göndererek test edelim. Ben **linux** bir **client** kullanıcam. Aslında **DNS** sunucusunun yapılandırma dosyasına bakıp bu durumu anlayabiliz ki zaten değişiklik **DNS** sunucusunun **conf** dosyasında yapılacak.

Testi “**nmap**” komutu ile yapacağım.

~~~
sudo nmap -sU -p 53 -sV -P0 --script=dns-recursion "ip_address"
~~~

![Crepe](/assets/img/bind-recursion-of/bind-rec-of01.png)

Yukarıdaki ss den de anlaşılacağı üzere **DNS Recursion** özelliği açık ve **DNS Amplification Attack** larına karşı savunmasız durumda. Şimdi **Recursion** özelliğini “**/etc/named.conf**” dosyasından **disable** edelim.,

**Recursion** özelliğini **disable** etmek için “**/etc/named.conf**” dosyasının “**options**” bölümüne “**recursion no;**” satırı eklenmelidir. Aşağıda ss ile de gösterilmiştir.

![Crepe](/assets/img/bind-recursion-of/bind-rec-of02.png)

Şimdi tekrar “**nmap**” komutumuzla test yapalım ve **recursion** anlaşılacağı üzere pasif durumda.

![Crepe](/assets/img/bind-recursion-of/bind-rec-of03.png)
