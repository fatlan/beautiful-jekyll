---
layout: post
title: oVirt Unlock Virtual Disks, VM and Snapshot
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/25-26-20-uelinuxegitim/25-26-20le05.png
#share-img: /assets/img/path.jpg
tags: [oVirt, linux, virtualization, centos, redhat, rhev]
comments: true
---

oVirt Unlock Virtual Disks, VM and Snapshot
****
Beklenmedik durumlarda **oVirt** üzerinden **vm**'ler ya da **disk**'ler ya da **snap**'ler **lock** duruma düşebilir. Bu durumu **cockpit** üzerinden gözlemleyebilirsiniz fakat çözüm için **HostedEngine** makinesine **SSH** atıp **unlock_entity.sh** scriptini kullanabilirsiniz.

**Lock** durumdaki **vm**, **disk** yada **snapshot**'ları aşağıdaki yöntemle **consol** üzerinden de tespit edebilirsiniz.
~~~
/usr/share/ovirt-engine/setup/dbutils/unlock_entity.sh -q -t vm -c
/usr/share/ovirt-engine/setup/dbutils/unlock_entity.sh -q -t disk -c
/usr/share/ovirt-engine/setup/dbutils/unlock_entity.sh -q -t snapshot -c
~~~

Akabinde ilgili **vm** diskini **unlock** etmek için,
~~~
/usr/share/ovirt-engine/setup/dbutils/unlock_entity.sh -t vm UUID_OF_VM
~~~

Ya da tüm diskleri **unlock** etmek için,
~~~
/usr/share/ovirt-engine/setup/dbutils/unlock_entity.sh -t all
~~~

Genellikle tercih edilmemeli ama **db**(**postgresql**) üzerinden de işlem yapılabilir.
~~~
su - postgres
scl enable rh-postgresql95 bash
psql
\c engine
select snapshot_id,description,status from snapshots where status = 'LOCKED' and vm_id in (select vm_guid from vm_static where vm_name='');
select image_guid,imagestatus from images where vm_snapshot_id = '' and imagestatus != 1;
update snapshots set status = 'OK' where snapshot_id = '';
updates images set imagestatus = 1 where image_guid = '';
~~~

ref:<br>
Helper Utilities: https://www.ovirt.org/develop/developer-guide/db-issues/helperutilities.html<br>
oVirt: Unlock Virtual Disks: https://angrysysadmins.tech/index.php/2018/10/grassyloki/ovirt-unlock-virtual-disks/<br>
Unlock Virtual Disks list: https://bugzilla.redhat.com/show_bug.cgi?id=1678234<br>
How to solve snapshots state ‘LOCKED’ in on Postgres DB: https://noeschanga.wordpress.com/2018/02/09/how-to-solve-snapshots-state-locked-in-ovirt-4-1/<br>
PostgreSql in oVirt(psql enable Ovirt): https://lists.ovirt.org/pipermail/users/2018-April/088432.html
