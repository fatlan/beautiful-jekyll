---
layout: post
title: Redhat/CentOS Linux Sunucularda Disable Repolar İçin YUM Kullanımı – YUM ENABLE
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [redhat, centos, linuz]
comments: true
---
![Crepe](/assets/img/red-with-yum-enab/red-yum-en01.png)

Genelde özelliştirilmiş **CentOS** sunucularda ilgili servis/hizmet dışında olan repolar(**/etc/yum.repos.d**) buna **CentOS**’un Base reposu dahil varsayılan olarak “**enable=0**” olarak ayarlandığında yani bu repo **disabled** moda çekildiğinde “**yum**” ile Base de var olan paketleri kurmak istediğiniz de hata alacaksınız ve kuramayacaksınız. Bu durumda repodan “**enable=1**” yapmalısınız ya da bu ayarın böyle kalması gerekiyorsa “**yum**” paket yöneticisini tek seferlik **enable** modda kullanarak bu sorunu aşıp, paketi kurabilirsiniz.

Örnekler aşağıdaki gibidir.

**CentOS BASE** reposu için;

~~~
yum install --enablerepo=”base” CentOS-Base.repo “paket_adı” -y
~~~

**Epel** reposu için;

~~~
yum install --enablerepo=”epel” epel.repo “paket_adı” -y
~~~