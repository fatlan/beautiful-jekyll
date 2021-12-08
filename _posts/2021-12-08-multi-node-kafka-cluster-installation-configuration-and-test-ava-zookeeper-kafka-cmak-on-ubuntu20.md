---
layout: post
title: Multi Node Kafka Cluster Installation, Configuration and Test(Java, zookeeper, kafka, cmak) on Ubuntu20
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/25-26-20-uelinuxegitim/25-26-20le05.png
#share-img: /assets/img/path.jpg
tags: [kafka, zookeeper, cmak, linux, ubuntu, cluster, java]
comments: true
---

**Hosts** dosyasını **edit**'leyin.
~~~
10.10.10.181 kafka01
10.10.10.51  kafka02
10.10.10.128 kafka03
~~~

**1- Java** install
~~~
sudo apt install default-jre default-jdk
~~~

**Zookeeper** ve **Kafka** servisleri için kullanıcı tanımlayalım
~~~
sudo useradd -m kafka -s /usr/sbin/nologin
sudo useradd -m zookeeper -s /usr/sbin/nologin
~~~

**2- Zookeeper** kurulum ve yapılandırılması
**Tüm node**'ler aynı adımlar
~~~
cd /opt/

sudo wget https://dlcdn.apache.org/zookeeper/zookeeper-3.6.3/apache-zookeeper-3.6.3-bin.tar.gz

sudo tar xzf apache-zookeeper-3.6.3-bin.tar.gz

sudo rm apache-zookeeper-3.6.3-bin.tar.gz

sudo mv apache-zookeeper-3.6.3-bin/conf/zoo_sample.cfg apache-zookeeper-3.6.3-bin/conf/zoo.cfg
~~~
~~~
sudo vi apache-zookeeper-3.6.3-bin/conf/zoo.cfg

dataDir=/var/zookeeper
clientPort=2181
server.1=10.10.10.181:2888:3888
server.2=10.10.10.51:2888:3888
server.3=10.10.10.128:2888:3888
~~~
~~~
sudo mkdir /var/zookeeper/ -p
~~~

**Herbir node**'de farklı **echo** satırı yapılabilir.
~~~
#kafka01 node
sudo echo '1' >> /var/zookeeper/myid

#kafka02 node
sudo echo '2' >> /var/zookeeper/myid

#kafka03 node
sudo echo '3' >> /var/zookeeper/myid
~~~

Servisin düzgün çalışıp çalışmadığını manual olarak aşağıda ki komutlarla test edebilirsiniz, biz ayrıca **systemd servisi** haline getireceğiz
~~~
sudo /opt/apache-zookeeper-3.6.3-bin/bin/zkServer.sh start /opt/apache-zookeeper-3.6.3-bin/conf/zoo.cfg
sudo /opt/apache-zookeeper-3.6.3-bin/bin/zkServer.sh stop /opt/apache-zookeeper-3.6.3-bin/conf/zoo.cfg
~~~

Şimdi ilgili klasörlere **zookeeper kullanıcısı** için **sahiplik** yetkisi verelim.
~~~
sudo chown -R zookeeper:zookeeper /opt/apache-zookeeper-3.6.3-bin/
sudo chown -R zookeeper:zookeeper /var/zookeeper/
~~~

**Zookeeper systemd** servis dosyasını **create** edelim
~~~
sudo vi /lib/systemd/system/zookeeper.service


[Unit]
Description=Zookeeper Daemon
Documentation=http://zookeeper.apache.org
Requires=network.target
After=network.target

[Service]
Type=forking
WorkingDirectory=/opt/apache-zookeeper-3.6.3-bin
User=zookeeper
Group=zookeeper
ExecStart=/opt/apache-zookeeper-3.6.3-bin/bin/zkServer.sh start /opt/apache-zookeeper-3.6.3-bin/conf/zoo.cfg
ExecStop=/opt/apache-zookeeper-3.6.3-bin/bin/zkServer.sh stop /opt/apache-zookeeper-3.6.3-bin/conf/zoo.cfg
ExecReload=/opt/apache-zookeeper-3.6.3-bin/bin/zkServer.sh restart /opt/apache-zookeeper-3.6.3-bin/conf/zoo.cfg
TimeoutSec=30
Restart=on-failure

[Install]
WantedBy=default.target
~~~
~~~
sudo systemctl daemon-reload
sudo systemctl start zookeeper.service
sudo systemctl enable zookeeper.service
sudo systemctl status zookeeper.service
~~~

**3- Kafka** kurulum ve yapılandırılması
**Tüm node**'ler aynı adımlar
~~~
cd /opt/

sudo wget https://dlcdn.apache.org/kafka/3.0.0/kafka_2.13-3.0.0.tgz

sudo tar xzf kafka_2.13-3.0.0.tgz

sudo rm kafka_2.13-3.0.0.tgz
~~~

**Herbir node**'de farklı **kafka config** ayarı yapılır
~~~
sudo vi kafka-3.0.0-src/config/server.properties
~~~

**kafka01 node**
~~~
broker.id=1
advertised.host.name=kafka01
advertised.listeners=PLAINTEXT://kafka01:9092
zookeeper.connect=10.10.10.181:2181,10.10.10.51:2181,10.10.10.128:2181
default.replication.factor=3
#delete.topic.enable = true
~~~

**kafka02 node**
~~~
broker.id=2
advertised.host.name=kafka02
advertised.listeners=PLAINTEXT://kafka02:9092
zookeeper.connect=10.10.10.181:2181,10.10.10.51:2181,10.10.10.128:2181
default.replication.factor=3
#delete.topic.enable = true
~~~

**kafka03 node**
~~~
broker.id=3
advertised.host.name=kafka03
advertised.listeners=PLAINTEXT://kafka03:9092
zookeeper.connect=10.10.10.181:2181,10.10.10.51:2181,10.10.10.128:2181
default.replication.factor=3
#delete.topic.enable = true
~~~

Servisin düzgün çalışıp çalışmadığını manual olarak aşağıda ki komutlarla test edebilirsiniz, biz ayrıca **systemd servisi** haline getireceğiz.
~~~
sudo /opt/kafka_2.13-3.0.0/bin/kafka-server-start.sh /opt/kafka_2.13-3.0.0/config/server.properties
sudo /opt/kafka_2.13-3.0.0/bin/kafka-server-stop.sh /opt/kafka_2.13-3.0.0/config/server.properties
~~~

Şimdi ilgili klasöre **kafka kullanıcısı** için **sahiplik** yetkisi verelim.
~~~
sudo chown -R kafka:kafka /opt/kafka_2.13-3.0.0/
~~~

**Kafka systemd** servis dosyasını **create** edelim
~~~
sudo vi /lib/systemd/system/kafka.service


[Unit]
Description=Kafka Daemon
Documentation=https://kafka.apache.org
Requires=zookeeper.service
After=zookeeper.service

[Service]
Type=simple
WorkingDirectory=/opt/kafka_2.13-3.0.0
User=kafka
Group=kafka
Environment="JAVA_HOME=/usr/lib/jvm/java-1.11.0-openjdk-amd64"
ExecStart=/opt/kafka_2.13-3.0.0/bin/kafka-server-start.sh /opt/kafka_2.13-3.0.0/config/server.properties
ExecStop=/opt/kafka_2.13-3.0.0/bin/kafka-server-stop.sh /opt/kafka_2.13-3.0.0/config/server.properties
TimeoutSec=30
Restart=on-failure

[Install]
WantedBy=default.target
~~~
~~~
sudo systemctl daemon-reload
sudo systemctl start kafka.service
sudo systemctl enable kafka.service
sudo systemctl status kafka.service
~~~

**TEST CLI and UI**

**Kafka Cluster**'ı komut satırı ile test etmek
~~~
#Testi başlatmak, topic açmak için;
/opt/kafka_2.13-3.0.0/bin/kafka-console-producer.sh --topic <fatihaslan> --bootstrap-server kafka01:9092,kafka02:9092,kafka03:9092


#Topic'leri dinlemek için(baştan sona);
/opt/kafka_2.13-3.0.0/bin/kafka-console-consumer.sh --topic <fatihaslan> --bootstrap-server kafka01:9092,kafka02:9092,kafka03:9092 --from-beginning


#Topic detaylarını görmek için lider, replica vs;
/opt/kafka_2.13-3.0.0/bin/kafka-topics.sh --describe --bootstrap-server kafka01:9092,kafka02:9092,kafka03:9092 --topic <fatihaslan>
Topic: fatihaslan       TopicId: qA4gvDiUS0eDH9_V-JngPw PartitionCount: 1       ReplicationFactor: 3    Configs: segment.bytes=1073741824
        Topic: fatihaslan       Partition: 0    Leader: 2       Replicas: 1,3,2 Isr: 2,3,1
~~~

**4- CMAK** kurulum ve yapılandırılması
**Tek host**'ta yapılması yeterlidir
~~~
sudo git clone https://github.com/yahoo/CMAK.git

cd CMAK/
~~~
~~~
vi /opt/CMAK/conf/application.conf


kafka-manager.zkhosts="10.10.10.181:2181,10.10.10.51:2181,10.10.10.128:2181"
cmak.zkhosts="10.10.10.181:2181,10.10.10.51:2181,10.10.10.128:2181"
~~~
~~~
./sbt clean dist

sudo cp /home/ubuntu/.sbt/1.0/staging/9fe122a9540185ff93da/cmak/target/universal/cmak-3.0.0.5.zip .

sudo unzip cmak-3.0.0.5.zip

cd cmak-3.0.0.5/

sudo bin/cmak -Dconfig.file=/opt/CMAK/conf/application.conf -Dhttp.port=9000
~~~
~~~
Open GUI http://10.10.10.181:9000/

add cluster

Cluster Name enter <3NodesKafkaBroker>

Cluster Zookeeper Hosts <10.10.10.181:2181,10.10.10.51:2181,10.10.10.128:2181>

SAVE
~~~

# ![](/assets/img/kfkacmak/cmak.png)
