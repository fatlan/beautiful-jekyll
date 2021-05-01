---
layout: post
title: PDF Düzenleme ve Birleştirme İşlemleri(ücretsiz ve basit)
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [pdf, linux]
comments: true
---
Bu yazımızda çok ihtiyaç duyulan konulardan biri, pdf sayfalarında ücretsiz ve basit bir şekilde nasıl düzenleme ya da birleştirme yapabileceğimize bakacağız. Özellikle Linux işletim sistemlerinde bu işlem için çok efor sarf etmemize gerek kalmayacak.

GUI’li li sistemde pdf düzenleme ve birleştirme işlemini LibreOffice Draw ile yapabilirsiniz. Basit bir arayüzü olduğu için detaya girmiyorum.

Terminal üzerinden yüksek sayıda sayfası mevcut pdf’leri ise “**pdfunite**” ve “**convert**” komutlarıyla saniyeler içerisinde birleştirebilirsiniz.

**Pdfunite**;

~~~
pdfunite le1.pdf le2.pdf ... leN.pdf merged.pdf
~~~

**Convert**;

~~~
convert le1.pdf le2.pdf ... leN.pdf merged.pdf
~~~

![Crepe](assets/img/pdfconv/pdfconv01.png)

