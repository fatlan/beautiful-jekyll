---
layout: post
title: Linux Sistemlerde TAB Tuşu ile Otomatik Komut yada Dizin Tamamlama - Bash Completion
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [linux, redhat, ubuntu, bash]
comments: true
---
![Crepe](/assets/img/bash-completion/bash-ac01.png)

Bash (Bourne Again Shell) şüphesiz en popüler Linux Shell olma özelliği ile beraber şuan için varsayılan shell olarak Linux yönetiminde top level seviyesindedir.

Her Linux sistem yöneticinin ihtiyaç duyabileceği Shell ile sistem yönetirken tab tuşu ile komut tamamlama Redhat/CentOS gibi Linux sistemlerin minimal kurulumlarında aktif bir şekilde gelmemektedir. Bu yüzden aşağıdaki komut ile **auto-completion**’u destekleyen bash için **bash-completion** paketini kurmalısınız.

Redhat/CentOS

~~~
yum install bash-completion
~~~

Ardından konsol ekranında **logout/login** olarak paketi aktif edebilirsiniz.