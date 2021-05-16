---
layout: post
title: Nginx HTTP Basic Authentication(Kimlik Doğrulama) Kurulumu
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [nginx, linux, http, authentication]
comments: true
---

![Crepe](/assets/img/http-auth-basic/http-basic-auth.png)

Aşağıdaki paketlerin kurulumunu gerçekleştirin.

Ubuntu;
~~~
sudo apt install nginx apache2-utils
~~~

Redhat/CentOS;
~~~
yum install epel-release nginx httpd-tools
~~~

Nginx “**/etc/nginx/sites-available/sites.conf**” altındaki kaynak konfig dosyası “**/etc/nginx/sites-enabled/sites.conf**” altında link’li olduğunu düşünerek, Yapılandırma şekli aşağıdaki gibi yapılabilir.

İlk önce aşağıdaki komutları yürütün,
```
- mkdir /www/sites/indexpage
- mkdir /www/sites/files
- mkdir /www/sites/files/exclude
- echo “Welcome to nginx” > /www/sites/indexpage/index.html
```

Ardından **sites.conf** aşağıdaki gibi yapılandırın.

~~~
server {			     ###Şifresiz olarak index sayfası gelmesi için yapılan konfigürasyon
        listen 80 default_server;
        server_name "";

	location / {
            root /www/sites/indexpage;
            index index.html index.htm;
        }

	location /files {		###/www/sites/files içerisine kullanıcı ve şifre ile erişim sağlamak için yapılan konfigürasyon
	    satisfy all;

	    root /www/sites/;

	    auth_basic "HTTPS Basic Auth Server";
	    auth_basic_user_file .htpasswd;
	}

	location /files/exclude { 	###exclude klasörü için şifresiz erişim sağlamak için yapılan konfigürasyon
	    alias /www/sites/files/exclude;
	}

}
~~~

**Basic auth** için kullanıcı ve şifre oluşturma(kullanıcı adı **fatlan**)
~~~
sudo htpasswd /etc/nginx/.htpasswd fatlan
~~~

 Kullanıcıları görmek için(şifreler hash’li görüntülenecektir),
~~~
cat /etc/nginx/.htpasswd
~~~

Son olarak yapılan konfig'lerin geçerli olması için;
~~~
sudo systemctl reload nginx
~~~
