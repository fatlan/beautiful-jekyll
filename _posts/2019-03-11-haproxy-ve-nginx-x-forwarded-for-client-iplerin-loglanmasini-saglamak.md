---
layout: post
title: HAProxy ve Nginx x-forwarded-for (Client ip lerin loglanmasını sağlamak)
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [haproxy, nginx, linux, loadbalancer]
comments: true
---
![Crepe](assets/img/ha-nix-forwarder/x-forwarder-for-ha-nix01.png)

HAProxy(haproxy.cfg) için herbir backend yapılandırmanızda aşağıdaki satır bulunmalıdır.

~~~
– option forwardfor
~~~

Nginx için “/etc/nginx/nginx.conf” yapılandırma dosyasını aşağıdaki gibi yapılandırmalısınız.

~~~
log_format main ‘$http_x_forwarded_for ($remote_addr) - $remote_user [$time_local] ‘
                ‘”$request” $status $body_bytes_sent “$http_referer” ‘
                ‘”$http_user_agent”‘ ;

access_log /var/log/nginx/access.log main;
error_log /var/log/nginx/error.log;
~~~

Ardından servisi yeniden başlatın.

~~~
systemctl restart nginx.service

service nginx restart

/etc/init.d/nginx restart
~~~

Daha sonra aşağıdaki komut ile son durumu gözden geçirebilirsiniz.

~~~
tail -f /var/log/nginx/access.log
~~~