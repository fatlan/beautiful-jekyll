---
layout: post
title: Keycloak 21 Install on Ubuntu 22 LTS
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [linux, keycloak, ubuntu, redhat, sso]
comments: true
---

Keycloak Install and Configuration

~~~
sudo apt update && sudo apt upgrade -y

sudo apt install software-properties-common ca-certificates chrony -y
~~~
~~~
sudo vi /etc/chrony/chrony.conf
~~~
~~~
sudo systemctl restart chrony.service

sudo apt install openjdk-17-jre-headless openjdk-17-jdk-headless -y
~~~

~~~
wget https://github.com/keycloak/keycloak/releases/download/21.0.1/keycloak-21.0.1.tar.gz
~~~

~~~
sudo vi /etc/environment
~~~
~~~
JAVA_HOME="/usr/lib/jvm/java-17-openjdk-amd64"
~~~
~~~
source /etc/environment
~~~

~~~
tar zxvf keycloak-21.0.1.tar.gz

sudo mv keycloak-21.0.1 keycloak

sudo mv keycloak /opt/
~~~

~~~
sudo vi /opt/keycloak/conf/keycloak.conf
~~~
~~~
db=postgres
db-username=DB_USER_NAME
db-password=DB_PASSWORD
db-url=jdbc:postgresql://localhost/keycloak
health-enabled=true
metrics-enabled=true

# HTTP
#http-enabled=true
#hostname-strict=false
#hostname-strict-https=false
#log=console,file

proxy=edge
hostname=kullanici-panel.fatlan.com
#hostname=kullanici-panel.fatlan.com:8080
~~~

~~~
cd /opt/keycloak/bin/
~~~
~~~
export KEYCLOAK_ADMIN=admin
export KEYCLOAK_ADMIN_PASSWORD=KYC_PASS
~~~
~~~
./kc.sh --verbose build

./kc.sh --verbose start

ctrl+c
~~~

~~~
sudo vim /etc/systemd/system/keycloak.service
~~~
~~~
[Unit]
Description=Keycloak Identity Provider
Requires=network.target
After=syslog.target network.target

[Service]
Type=idle
User=ubuntu
Group=ubuntu
#RemainAfterExit=yes
LimitNOFILE=102642
ExecStart=/opt/keycloak/bin/kc.sh start --log=console,file
#WorkingDirectory=/opt/keycloak
StandardOutput=null

[Install]
WantedBy=multi-user.target
~~~
~~~
sudo systemctl daemon-reload
sudo systemctl enable keycloak.service
sudo systemctl start keycloak.service
sudo systemctl status keycloak.service
~~~



