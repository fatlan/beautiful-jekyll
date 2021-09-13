---
layout: post
title: pg_auto_failover ile PostgreSQL Cluster Kurulumu
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/25-26-20-uelinuxegitim/25-26-20le05.png
#share-img: /assets/img/path.jpg
tags: [database, postgresql, linux, ubuntu, cluster, failover, pg_auto_failover]
comments: true
---

**Monitor**(10.10.10.147) + **3 PostgreSQL**(10.10.10.144, 10.10.10.145, 10.10.10.146) **Toplam 4 NODE**

Aşağıdaki komut **tüm node**'lerde, **sudo yetkili** kullanıcı ile çalıştırılmalıdır.
~~~
curl https://install.citusdata.com/community/deb.sh | sudo bash
sudo apt-get install postgresql-13-auto-failover-1.5
~~~

**NoT**: **PostgreSQL** **default**'ta **5432** **port**undan çalışır, yukarıdaki kurulum esnasında otomatik olarak **PostgreSQL** ayağa kalktığı için **5432 port** kullanımda olur. Biz de gelecekte herhangi bir karışıklığa sebebiyet vermemek için(yani dışarıdan bir kullanıcı veritabanına bağlanmak için **psql** dediğinde **varsayılan** olarak **5432** **port**u üzerinden çalışan **PostgreSQL** **instance** veritabanına bağlantı yapacaktır. Doğal olarak **pg_auto_failover** ile biz farklı **port**lar üzerinden **cluster** kurulumu yapabileceğimiz için **psql** ile **cluster**'a **ip** ya da **port**u belirterek **cluster** **PostgreSQL**'e bağlantı sağlayabiliriz. Biz hem bu durumun önüne geçip birden fazla **instance** **PostgreSQL** çalışmasını önlemek hem de farklı kullanıcılarda oluşabilecek kafa karışıklığının önüne geçebilmek için varsayılan **PostgreSQL** servisini **kapatıp**, **pasif** hale getiriyoruz. )

~~~
sudo systemctl stop postgresql.service

sudo systemctl disable postgresql.service
~~~

Akabinde **Monitor Node**'sinde(10.10.10.147) çalıştırılır.

**Posgres kullanıcısı**na geçilir.
~~~
sudo su - postgres

export PGDATA=./monitor

pg_autoctl create monitor --ssl-self-signed --hostname 10.10.10.147 --auth trust --run

ctrl+c   # servisin çalıştığı görülür ve kesilir
~~~

**postgres** kullanicisindan çıkılır ve **sudo yetkili** kullaniciya dönülür.
~~~
pg_autoctl -q show systemd --pgdata /var/lib/postgresql/monitor | sudo tee /etc/systemd/system/pgautofailover.service

sudo systemctl daemon-reload
sudo systemctl enable pgautofailover
sudo systemctl start pgautofailover
sudo systemctl status pgautofailover
~~~

**1. Node**(10.10.10.144)

**Posgres** kullanıcısına geçilir.
~~~
sudo su - postgres

export PGDATA=./data

pg_autoctl create postgres --hostname 10.10.10.144 --auth trust --ssl-self-signed --monitor 'postgres://autoctl_node@10.10.10.147:5432/pg_auto_failover?sslmode=require' --run

ctrl+c    # servisin çalıştığı görülür ve kesilir
~~~

**postgres** kullanicisindan çıkılır ve **sudo** yetkili kullaniciya dönülür.
~~~
pg_autoctl -q show systemd --pgdata /var/lib/postgresql/data | sudo tee /etc/systemd/system/pgautofailover.service

sudo systemctl daemon-reload
sudo systemctl enable pgautofailover
sudo systemctl start pgautofailover
sudo systemctl status pgautofailover
~~~

**2. Node**(10.10.10.145)

**Posgres** kullanıcısına geçilir.
~~~
sudo su - postgres

export PGDATA=./data

pg_autoctl create postgres --hostname 10.10.10.145 --auth trust --ssl-self-signed --monitor 'postgres://autoctl_node@10.10.10.147:5432/pg_auto_failover?sslmode=require' --run

ctrl+c    # servisin çalıştığı görülür ve kesilir
~~~

**postgres** kullanicisindan çıkılır ve **sudo** yetkili kullaniciya dönülür.
~~~
pg_autoctl -q show systemd --pgdata /var/lib/postgresql/data | sudo tee /etc/systemd/system/pgautofailover.service

sudo systemctl daemon-reload
sudo systemctl enable pgautofailover
sudo systemctl start pgautofailover
sudo systemctl status pgautofailover
~~~

**3. Node**(10.10.10.145)

**Posgres** kullanıcısına geçilir.
~~~
sudo su - postgres

export PGDATA=./data

pg_autoctl create postgres --hostname 10.10.10.146 --auth trust --ssl-self-signed --monitor 'postgres://autoctl_node@10.10.10.147:5432/pg_auto_failover?sslmode=require' --run

ctrl+c    # servisin çalıştığı görülür ve kesilir
~~~

**postgres** kullanicisindan çıkılır ve **sudo** yetkili kullaniciya dönülür.
~~~
pg_autoctl -q show systemd --pgdata /var/lib/postgresql/data | sudo tee /etc/systemd/system/pgautofailover.service

sudo systemctl daemon-reload
sudo systemctl enable pgautofailover
sudo systemctl start pgautofailover
sudo systemctl status pgautofailover
~~~

Cluster kurulumumuz tamamlandı.

Şimdi **Monitör node**'sinde **cluster status**unu gostermek için, **posgres** kullanıcısına bağlanıp,
~~~
export PGDATA=./monitor

pg_autoctl show state
~~~

~~~
  Name |  Node |        Host:Port |       LSN |   Connection |       Current State |      Assigned State
-------+-------+------------------+-----------+--------------+---------------------+--------------------
node_1 |     1 | 10.10.10.144:5432 | 0/8000B78 |   read-write |             primary |             primary
node_2 |     2 | 10.10.10.145:5432 | 0/8000B78 |    read-only |           secondary |           secondary
node_3 |     3 | 10.10.10.146:5432 | 0/8000B78 |    read-only |           secondary |           secondary

~~~

**Monitör node**'sinde **connection string**'i görmek için yine **posgres** kullanıcısında,

~~~
export PGDATA=./monitor

pg_autoctl show uri
~~~

~~~
        Type |    Name | Connection String
-------------+---------+-------------------------------
     monitor | monitor | postgres://autoctl_node@10.10.10.147:5432/pg_auto_failover?sslmode=require
   formation | default | postgres://10.10.10.145:5432,10.10.10.144:5432,10.10.10.146:5432/postgres?target_session_attrs=read-write&sslmode=require
~~~

**Monitör node**'sinde **cluster log**'larını görmek için yine **posgres** kullanıcısında,
~~~
export PGDATA=./monitor

pg_autoctl show events
~~~

Veritabanına dışarıdan(**remote** vs) erişim için **pg_hba.conf** dosyasında ilgili ayarlamalar yapılmalıdır, nasıl yapılacağına dair dosya içinde gerekli yönlendirme mevcuttur. Bu değişiklikler **her node** için ayrı ayrı yapılmalıdır.
~~~
#pg_hba.conf değişiklik yapıldığında

export PGDATA=./data

export PATH=/usr/lib/postgresql/13/bin:$PATH

pg_ctl reload
~~~

**PostgreSQL db connection example**;
~~~
sudo -u postgres psql postgres://10.10.10.144:5432,10.10.10.145:5432,10.10.10.146:5432/postgres?target_session_attrs=read-write
~~~

**PostgreSQL JDBC Spring read replica example**;
~~~
spring.datasource.url=jdbc:postgresql://10.10.10.144:5432,10.10.10.145:5432,10.10.10.146:5432/postgres?currentSchema=postgres&targetServerType=master
~~~


ref:

[pg_auto_failover github](https://github.com/citusdata/pg_auto_failover) | [readthedocs](https://pg-auto-failover.readthedocs.io/en/latest/architecture.html) | [ytube](https://www.youtube.com/watch?v=GtJfdsJ8_Yw)

