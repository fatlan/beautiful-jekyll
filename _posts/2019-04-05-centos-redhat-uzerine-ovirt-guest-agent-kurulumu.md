---
layout: post
title: CentOS/Redhat Üzerine oVirt Guest Agent Kurulumu
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [centos, linux, ovirt]
comments: true
---
![Crepe](assets/img/c-r-o-a-ints/cos-ovirt-agent01.png)

İlk önce epel reposu kurulur.

~~~
yum install epel-release -y
~~~

Ardından guest agent kurulumu yapılır.

~~~
yum install ovirt-guest-agent -y yada yum install ovirt-guest-agent-common -y
~~~

Daha sonra servis aktif edilir.

~~~
systemctl enable ovirt-guest-agent.service
systemctl start ovirt-guest-agent.service
systemctl status ovirt-guest-agent.service
~~~

