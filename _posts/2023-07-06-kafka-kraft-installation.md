---
layout: post
title: How To Install Kafka With Kraft Mode on Ubuntu
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [linux, kafka, kraft, ubuntu, zookeper]
comments: true
---

~~~
sudo apt update && sudo apt install default-jre default-jdk -y
sudo useradd -m kraft -s /usr/sbin/nologin
cd /opt/
sudo wget https://dlcdn.apache.org/kafka/3.5.0/kafka_2.13-3.5.0.tgz
sudo tar xzf kafka_2.13-3.5.0.tgz
sudo rm kafka_2.13-3.5.0.tgz
cd kafka_2.13-3.5.0/
KAFKA_CLUSTER_ID="$(bin/kafka-storage.sh random-uuid)"
echo $KAFKA_CLUSTER_ID
~~~

~~~
sudo vi config/kraft/server.properties
~~~
add below line
~~~
controller.mode=kraft
~~~

~~~
bin/kafka-storage.sh format -t $KAFKA_CLUSTER_ID -c config/kraft/server.properties
sudo bin/kafka-server-start.sh config/kraft/server.properties
ctrl+c
~~~

sudo vi /lib/systemd/system/kafka.service
~~~
[Unit]
Description=Kafka Kraft Daemon
Documentation=https://kafka.apache.org/quickstart
Requires=network.target
After=network.target

[Service]
Type=forking
WorkingDirectory=/opt/kafka_2.13-3.5.0
User=kraft
Group=kraft
Environment=KAFKA_HEAP_OPTS="-Xmx1G -Xms1G"
Environment=KAFKA_JVM_PERFORMANCE_OPTS="-XX:+UseG1GC -XX:MaxGCPauseMillis=20 -XX:InitiatingHeapOccupancyPercent=35 -XX:+ExplicitGCInvokesConcurrent"
Environment="JAVA_HOME=/usr/lib/jvm/java-1.11.0-openjdk-amd64"
#ExecStartPre=/bin/bash -c 'KAFKA_CLUSTER_ID="$(/opt/kafka_2.13-3.5.0/bin/kafka-storage.sh random-uuid)"; /opt/kafka_2.13-3.5.0/bin/kafka-storage.sh format -t $KAFKA_CLUSTER_ID -c /opt/kafka_2.13-3.5.0/config/kraft/server.properties'
#ExecStartPre=/opt/kafka_2.13-3.5.0/bin/kafka-setup.sh
ExecStart=/opt/kafka_2.13-3.5.0/bin/kafka-server-start.sh -daemon /opt/kafka_2.13-3.5.0/config/kraft/server.properties --override controller.mode=kraft
ExecStop=/opt/kafka_2.13-3.5.0/bin/kafka-server-stop.sh /opt/kafka_2.13-3.5.0/config/kraft/server.properties --override controller.mode=kraft
TimeoutSec=30
Restart=on-failure
LimitNOFILE=65536

[Install]
WantedBy=default.target
~~~

~~~
sudo chown -R kraft:kraft kafka_2.13-3.5.0
sudo systemctl daemon-reload
sudo systemctl enable kafka.service
sudo systemctl start kafka.service
sudo systemctl status kafka.service
sudo  tail -100f /opt/kafka_2.13-3.5.0/logs/server.log
sudo ss -plnt
~~~

Test
~~~
/opt/kafka_2.13-3.5.0/bin/kafka-topics.sh --create --topic kraft-test --partitions 1 --replication-factor 1 --bootstrap-server localhost:9092
/opt/kafka_2.13-3.5.0/bin/kafka-topics.sh --describe --bootstrap-server localhost:9092 --topic kraft-test
~~~


ref: https://kafka.apache.org/quickstart
