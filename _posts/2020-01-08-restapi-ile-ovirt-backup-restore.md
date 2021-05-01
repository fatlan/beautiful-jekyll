---
layout: post
title: RestAPI ile Ovirt(Open Virtualization Manager)’da Snapshot Tabanında Otomatik Backup/Restore Operasyonu
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [backup, vm, ovirt, rhev, virtualization]
comments: true
---
![Crepe](/assets/img/restapi-ovirt-bac-res/rest-bac-res01.png)

Konu ile ilgili daha önceden Python ile yazılmış online full backup script’ini kullanarak bu işlemi gerçekleştireceğiz.
[https://github.com/wefixit-AT/oVirtBackup](https://github.com/wefixit-AT/oVirtBackup) adresinden bu script’i indirebilirsiniz.

**BACKUP**

Tek yapmamız gereken bu tool’u kendi ortamımıza göre uyarlayıp, daha sonro **crontab** ile otomatik hale getirmek. Şimdi aşağıdaki adımlarla bu işlemin nasıl yapılacağına bir bakalım.

İlk olarak **oVirt** ortamına yedeklemi işlemini tetikleyeceğimiz(python script’inin çalıştıracağımız) bir makine kurup ayağa kaldırıyoruz. Ben CentOS bir makine kurdum. Ardından python çalıştırabileceğimiz paketleri kuruyoruz(ovirt-engine-sdk-python, gcc, python-devel, openssl-devel, libxml2, libxml2-devel, python36, python36-pip, ovirtsdk, curl-devel, pip3 vs. - Script’teki requirements.txt dosyasını okuyunuz.). Belirttiğim paketleri kurmadan çalıştırırsanız karşınıza çıkan uyarılarla gereken paketleri kurabilirsiniz ki bence öyle de yapabilirsiniz.

Ardından içerisindeki iki dosya bizim için önemli “**backup.py**” ve “**config_example.cfg**” dosyaları. “**backup.py**” dosyası bizim ortamımız için yapılandırılmış “**config_example.cfg**” dosyasına göre yedekleme işlemini çalıştıracaktır.

İş akışı sırasıyla aşağıdaki gibidir.

- Anlık görüntü oluştur
- Anlık görüntüden, yeni bir VM oluştur
- Anlık görüntüyü sil
- Önceki yedekleri sil (ayarlanmışsa)
- Anlık görüntüden oluşan VM’yi NFS paylaşımına Export et
- Anlık görüntüden oluşan VM’yi sil

**Crontab** çıktım aşağıdaki gibi olsun(yani her perşembe saat 23:00’de ilgili vm için yedeklemeyi başlat.)

~~~
crontab -l
~~~

~~~
00 23 * * 4 /usr/bin/python /scripts/oVirtBackup-master/backup.py -c /scripts/oVirtBackup-master/config_vms.cfg -d
~~~

Şimdi en önemli kısım olan “**config_example.cfg**” dosyasında ki alanları inceleyip, kendi ortamımıza göre nasıl yapılandıracağız ona bakalım. Sadece ilgili değişkenleri ele alacağız.

- vm_names: [“MysqlVM”]
- vm_middle=_BACKUP
- snapshot_description=Snapshot for backup script
- server=https://ovirt-server.mydomain/ovirt-engine/api
- username=admin@internal
- password=adminkullanıcısınınparolası
- export_domain=export_nfs #Bu alana Domain Function Export, Storage Type **NFS** yaptığınız **Storage Domain**’i yazmalısınız.
- timeout=5000
- cluster_name=Default
- backup_keep_count=8 #Yedeklemelerin bu günler içinde ne kadar süreyle tutulması gerektiği belirtir. Ben otomatik olarak crontab’ta her haftanın perşembe gününde(4) yedeklemeyi tetiklediğim için ve geriye dönük 2 adet yedek saklamak istediğim için 7+1=8 olarak atadım.
- dry_run=False
- vm_name_max_length=60
- use_short_su
- x=False
- storage_domain=data01 #Sunucunun, disk dosyasının barındığı Storage Domain’i yazmalısınız.
- storage_space_threshold=0.1
- logger_fmt=%(asctime)s: %(message)s
- logger_ le_path=/var/log/ovirt-vm-backup.log #Backup’lama esnasında oluşacak logların barındırılacağı yeri belirtmelisiniz.
- persist_memorystate=False

Artık VM’leriniz **Storage/Storage Domains/export_nfs** altına **crontab**’ın tetiklemesiyle **oVirtBackup script**’i sayesinde otomatik olarak yedeklenecektir.

**RESTORE**

**Storage/Storage Domains/export_nfs** mevcut olan herhangi bir **virtual machine** seçili iken “**Import**” tıklayın. Akabinden aşağıdaki görselden de anlaşılacağı üzere **OK** tıklayıp, aktif ortama dahil edip **restore** işlemini tamamlamış olursunuz.

![Crepe](/assets/img/restapi-ovirt-bac-res/rest-bac-res02.png)
