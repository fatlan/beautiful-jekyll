---
layout: post
title: Linux Sistemlerde Block Aygıt Rescan(changes of devices)
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/25-26-20-uelinuxegitim/25-26-20le05.png
#share-img: /assets/img/path.jpg
tags: [block, rescan, device, disk]
comments: true
---
Block aygıtları listeleyelim,
~~~
ls -l /sys/class/scsi_device/*/device/block
~~~
örnek çıktı;
~~~
/sys/class/scsi_device/0:0:0:0/device/block:
total 0
drwxr-xr-x 8 root root 0 Aug 18 15:30 sda

/sys/class/scsi_device/1:0:0:0/device/block:
total 0
drwxr-xr-x 8 root root 0 Aug 18 15:30 sdb

/sys/class/scsi_device/4:0:0:0/device/block:
total 0
drwxr-xr-x 12 root root 0 Aug 18 15:30 sdc

/sys/class/scsi_device/5:0:0:0/device/block:
total 0
drwxr-xr-x 8 root root 0 Aug 18 15:30 sdd

/sys/class/scsi_device/6:0:0:0/device/block:
total 0
drwxr-xr-x 8 root root 0 Aug 18 15:30 sde

/sys/class/scsi_device/7:0:0:0/device/block:
total 0
drwxr-xr-x 8 root root 0 Aug 18 15:30 sdf
~~~

Örnek olarak **sde** disk **rescan** yapalım;
~~~
echo 1 > /sys/class/scsi_device/6:0:0:0/device/rescan
~~~

`**dmesg**` ile logları kontrol edebilirsiniz.
