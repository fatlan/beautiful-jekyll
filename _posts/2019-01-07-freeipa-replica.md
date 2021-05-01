---
layout: post
title: FreeIPA(LDAP, DNS) – iDM(identity management) Replica Kurulumu
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [freeipa, ldap, dns, idm, redhat, linux]
comments: true
---
Daha önceki makalemizde **FreeIPA server** kurulumunu ele almıştık.

Tabi bu bizler her yapıda olduğu gibi **FreeIPA** sistemimizde de yedekli çalışıp **HA** yapısını sağlayacağız. Kısaca **FreeIPA Replica** işlemi ile kesintisiz ve yedekli bir yapı sağlamış olacağız.

- Master FreeIPA : dc01.fatlan.local
- Replica FreeIPA : dc02.fatlan.local

**Master** sunucumuz(**dc01.fatlan.local**) daha öncesinde kurulmuştu. Şimdi adım adım **Replica FreeIPA**(**dc02.fatlan.local**) kurulumunu ele alalım.

**Hostname, Hosts, Resolv, NIC** dosya’larını yapılandırdıktan sonra **firewalld** yapılandırmasını yapmalısınız. Aşağıdaki komut ile gerekli servislere izin verelim ve servisi **reload** edelim.

~~~
rewall-cmd --add-service={http,https,ssh,ntp,dns,freeipa-ldap,freeipa-ldaps,kerberos,kpasswd,ldap,ldaps,freeipa-replication} --permanent
rewall-cmd --reload
~~~

Hemen akabinde aşağıdaki paketleri **dc02** **replica** sunucumuza kuralım.

~~~
yum install ipa-server bind-dyndb-ldap ipa-server-dns -y
~~~

Ardından aşağıdaki komutu gene **dc02** **replica** sunucusunda çalıştırın ve aşağıdaki yönergeler ile **ipa client** kurulumunu bitirin.

~~~
ipa-client-install --domain=fatlan.local --realm=FATLAN.LOCAL --server=dc01.fatlan.local
-yes
-yes
-admin
-admin şifresi
~~~

![Crepe](assets/img/freipa-rep-ol/fre-ipa01.png)

![Crepe](assets/img/freipa-rep-ol/fre-ipa02.png)

![Crepe](assets/img/freipa-rep-ol/fre-ipa03.png)

![Crepe](assets/img/freipa-rep-ol/fre-ipa04.png)

Şimdi **Master** **dc01.fatlan.local** sunucusunda aşağıdaki komutu çalıştırın. Böylelikle **dc02** sunucusu “**ipaservers**” grubuna dahil etmiş olacağız.

**NoT** : Bu komutu master freeipa da çalıştırabilmek için, sunucuya login olduktan sonra “**kinit admin**” ile servise bağlantı kurmanız gerekmektedir. Aksi taktirde “**ipa: ERROR: did not receive Kerberos credentials**” hatasını alırsınız. Hatayı görmeniz için aşağıdaki ss’de paylaştım.

~~~
ipa hostgroup-add-member ipaservers --hosts dc02.fatlan.local
~~~

![Crepe](assets/img/freipa-rep-ol/fre-ipa05.png)

Şimdi son komutumuzu **Replica FreeIPA**(dc02.fatlan.local) da çalıştırarak kurulumu tamamlayalım.

~~~
ipa-replica-install
-yes
~~~

![Crepe](assets/img/freipa-rep-ol/fre-ipa06.png)

İşlem bu kadar. Artık her iki, yani dc01 ve dc02 **FreeIPA** sunucuları aktif-aktif çalışcaktır. Herhangi birinde yaptığınız bir işlem diğerine de yansıyacaktır. Böylelikle hem yedeklilik hemde **HA** sağlamış oldunuz.

Son durumu aşağıdaki komut ile de kontrol edebilirsiniz.

~~~
ipa-replica-manage list
~~~

![Crepe](assets/img/freipa-rep-ol/fre-ipa07.png)
