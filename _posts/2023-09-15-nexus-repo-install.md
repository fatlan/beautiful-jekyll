---
layout: post
title: Nexus Artifactory Server Install and Configuration
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [linux, nexus, ubuntu, artifactory, redhat, centos, repo, repository, docker, docker-compose, container]
comments: true
---

Nexus Artifactory server'ı containerization olarak ayağa kaldıracağımız için ilk olarak docker kurulumu ile başlıyoruz. Bunun için docker kurulun resmi sitesinden takip edebilirsiniz. [https://docs.docker.com/engine/install/](https://docs.docker.com/engine/install/)

**Nexus** datalarımızı saklayacağımız ikinci bir disk(**/dev/sdb**) takmıştık ve onu yapılandırıp, **mount** ederek devam edelim.
~~~
sudo mkfs.ext4 /dev/sdb

sudo mkdir /mnt/nexus-data

sudo mount /dev/sdb /mnt/nexus-data

sudo chown -R $USER:$USER /mnt/nexus-data
~~~

Şimdi **mount point**'imizi **fstab**'a kaydedelim.
~~~
sudo vi /etc/fstab

#satır aşağıdaki gibidir.
/dev/sdb	/mnt/nexus-data	ext4	defaults	0 1
~~~

Şimdi bulunduğum **path**'de bir dizin oluşturuyorum.
~~~
mkdir nexus-repo

cd nexus-repo
~~~

Akabinde **compose yml** dosyamı oluşturuyorum.
~~~
vim docker-compose.yml
~~~
~~~
version: '3'

services:
  nexus:
    image: sonatype/nexus3
    container_name: nexus-repo
    networks:
      - nexus-net
    restart: always
    ports:
      - "8081:8081"
    volumes:
      - nexus-data:/nexus-data
    environment:
      - INSTALL4J_ADD_VM_PARAMS=-Xms2703m -Xmx2703m -XX:MaxDirectMemorySize=2703m -Djava.util.prefs.userRoot=/mnt/nexus-data

networks:
  nexus-net:
    driver: bridge

volumes:
  nexus-data:
    driver: local
    driver_opts:
      type: none
      o: bind
      device: /mnt/nexus-data
~~~
~~~
docker compose up -d

docker compose ps
~~~
~~~
NAME                IMAGE               COMMAND                  SERVICE             CREATED             STATUS              PORTS
nexus-repo          sonatype/nexus3     "/opt/sonatype/nexus…"   nexus               2 weeks ago         Up 2 weeks          0.0.0.0:8081->8081/tcp, :::8081->8081/tcp
~~~

