---
layout: post
title: Linux Sistemlerde ICMP Paketini Gizleme
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [icmp, network, linux]
comments: true
---
Hemen hemen tüm saldırılar ağda ip’lerin kullanımı keşfedildikten ve öncesinde çeşitli taramalardan geçirildikten sonra zafiyetler tespit edilip, saldırılar başlatılabilir. IP Flooding(ip seli) ya da DDOS gibi saldırılardan korunmalıdır ki aslında DDOS, saldırı türlerinden en berbat olanı ve hala dünya üzerinden önlemeyle alakalı tam bir çözümü olmayan bir yöntemdir. Bu durumda network üzerinde sunucular dış dünyaya gizlemelidir. ICMP paketlerini gizlemeniz bunun için bir başlangıç olacaktır ki zaten bu paketin mesajları kritik sunucular için gizlenmelidir.

Bu yapılandırma yukarıda güvenlik duvarları aracılığı ile yapılabilir fakat burada bahsedeceğimiz yöntemde bu işlemi Linux kernel modülünde de yapılabilir olduğunu göstermektir ki Linux’un kernel modüllerinde bunun gibi bir sürü parametre bulunmaktadır ve her işlem için kritik durum arz eden kısımlarda bu parametreler ayrı ayrı incelenip, tek tek yapılandırılmalıdır. Bu değişiklik için kullanacağımız çekirdek parametresi “**net.ipv4.icmp_echo_ignore_all**” modülüdür. Şimdi bu işlemleri nasıl yapılacağına göz atılmalıdır ama öncesinde bu değişkenin varsayılan parametresi neymiş ona bir göz atıp ve **0** olduğunu görelim.

~~~
sysctl -a | egrep -ie “net.ipv4.icmp_echo_ignore_all”
~~~

![Crepe](/assets/img/lin-icmp/lin-icmp01.png)

Ardından aşağıdaki “**sysctl**” komutu ile değeri **1**’e çekilmelidir fakat bu işlem anlık geçerli olacaktır yani sistem yeniden başladığı zaman varsayılan değerine geri dönecektir.

~~~
sysctl net.ipv4.icmp_echo_ignore_all=1
~~~

Ya da

~~~
echo “1” > /proc/sys/net/ipv4/icmp_echo_ignore_all
~~~

Bu işlemi kalıcı hale getirmek için “**/etc/sysctl.conf**” dosyasına bu değişken ve parametreyi yazmak gerekmektedir.

Herhangi bir editör yardımıyla “**/etc/sysctl.conf**” dosyasını edit edip “**net.ipv4.icmp_echo_ignore_all = 1**” ekleyebilir ya da aşağıdaki komut aracılığı ile bunu sağlayabiliriz.

~~~
echo “net.ipv4.icmp_echo_ignore_all = 1” >> /etc/sysctl.conf
~~~

Arından **sysctl.conf** dosyasını aşağıdaki komutla yeniden yükleyip, yapılandırmayı aktif hale getirmelisiniz.

~~~
sysctl -p
~~~