---
layout: post
title: systemd-analyze blame Komutunu Keşfedelim - Redhat7, CentOS7, Ubuntu16
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [systemd, blame, linux, command]
comments: true
---
![Crepe](assets/img/syst-analyz-blame/sys-anl-blame01.png)

Bu makalemde çok hoş bir şekilde deneyimsellediğim bir durum ve komuttan bahsedeceğim. Bir komut nedir.? Diyebilirsiniz. Fakat sistem yöneticileri için özellikle de Linux Admin’leri için bir komut bile yeri geldi mi can kurtarıcı bir duruma dönüşebilir. Uzun süredir bakmadığınız ya da başkası tarafından kurulup çalışan, sonradan yönetimi devraldığınız bir sunucuda yaşayabileceğiniz bir durum olabilir. Bende böyle bir durum oluştu ve **RedHat Enterprise Linux 7** de açılışta bir problem yaşadım. Neydi bu problem?

Sunucu **RHEL 7** ama **GUI** de kurulu ve sunucu açılışta çok yavaş açılıyordu. **User**’lar, login ekranı dakikalar sonra geldi. Bir sorun olduğu belliydi ama onun dışında herhangi bir sorun yok sunucu canavar gibi çalışıyordu. Neyse tam bu noktada nedir problem? Nasıl çözerim.? derken, bir komut keşfettim. Bu komut “**system-analyze blame**”. Bu komut kısaca **analyze system boot-up performance**, sistemin **boot** olmasında açılan servis ve uygulamaların başlangıç performansı olarak **time** bazında size yansıtacaktır ve sizde oradan alacağınız ip ucuyla sorunu giderebilirsiniz ki ben bu şekilde, belki de uzun süre uğraştıracak bir sorunu saniyeler içinde hallettim.

Bu komutun üzerinde durmamım bir diğer sebebi ise **RedHat 7** de çalışmasıdır. Yani altı sürümlerde bu komut mevcut değil. Bu da tabi **RHEL 7**’nin kullanış seviyesini yansıtmaktadır. Tabi aynı şekilde **CentOS 7** ve **Ubuntu 16** ve üzerinde de bu komut mevcut.

Şimdi aşağıdaki screenshot’larla durumu daha detaylı inceleyelim.

Sistemi bir kere açmayı başarınca **system-analyze blame** komutunu çalıştırdım ve en geç açılan **nfs.mount** servisinin olduğunu gördüm.

~~~
system-analyze blame
~~~

![Crepe](assets/img/syst-analyz-blame/sys-anl-blame02.png)

Ardından “**/etc/fstab**” dosyasını incelemeye aldım. Zaten açılışta bu problemin olması ve sistem **boot** olduğundan **fstab** dosyasını okuması şüpheyi kendi üzerine çekmeye yetiyordu. Ve en son satırda bir makinadan **mount** işleminin olduğunu gördüm. Tabi ki sistem **boot** olduğunda **fstab** dosyasını okuyup **mount** ediyor ve o hedefteki ip’li makinayı bulamadığı için açılış problemi olduğunu netti.

![Crepe](assets/img/syst-analyz-blame/sys-anl-blame03.png)

**İnsert mod**da bu satırın başına **# **işareti koyup, kaydedip çıktım. Böylelikle o satırı yorum satırı olarak algılamasını sağladık. Ve sistemi **reboot** ettiğimde problemin çözüldüğünü gözlemledim.

![Crepe](assets/img/syst-analyz-blame/sys-anl-blame04.png)
