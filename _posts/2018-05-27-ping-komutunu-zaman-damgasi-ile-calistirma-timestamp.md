---
layout: post
title: Ping Komutunu Zaman Damgası İle Çalıştırma (timestamp)
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [ping, timestamp, linux]
comments: true
---
![Crepe](assets/img/ping-wth-timestamp/ping-timespm01.png)

**Ping** komutunu **dakika/saniye** hatta tarih cinsinden akışını takip etmek isteyebilirsiniz. Bunun için aşağıdaki şekilde time stamp ekleyerek çalıştırmalısınız.

~~~
ping 8.8.8.8 | while read pong; do echo “$(date): $pong”; done
~~~