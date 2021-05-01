---
layout: post
title: oVirt Debian/Ubuntu Guest Agent Kurulumu ve Yapılandırması - Sorun Çözüldü
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [ovirt, ubuntu, virtualization, rhev, linux]
comments: true
---
![Crepe](assets/img/ovirt-ubuntu16-agent/u16o-ga01.png)

**oVirt** ortamına kurduğunuz **Debian/Ubuntu** makineler için aşağıdaki komutu çalıştırarak **ovirt-guest-agent** servisini kurabilirsiniz.

~~~
sudo apt install ovirt-guest-agent -y
~~~

Fakat servis durumunu kontrol ettiğinizde aşağıdaki gibi hata aldığınızı göreceksiniz.

![Crepe](assets/img/ovirt-ubuntu16-agent/u16o-ga02.png)

Aşağıdaki yönergeleri izleyerek hataları giderip, servisi sağlıklı bir şekilde çalıştırabilirsiniz. İlk önce “**ovirt-guest-agent.service**” servis dosyasını edit edip, “**User=root**” olarak değiştirelim(servisin root haklarında çalışmasının, güvenlik tarafındaki oluşturabileceği sıkıntılara şimdilik değinmiyorum)

~~~
sudo vi /lib/systemd/system/ovirt-guest-agent.service

    - User=root
~~~

Ardından aşağıdaki komutu çalıştırın.

~~~
systemctl daemon-reload
~~~

Akabinde aşağıdaki komutla device’leri listeleyebilirsiniz. Çünkü “**ovirt-guest.agent.0**” aygıtını kullanacağız.

~~~
ll /dev/virtio-ports/
~~~

![Crepe](assets/img/ovirt-ubuntu16-agent/u16o-ga03.png)

Şimdi “**ovirt-guest-agent.conf**” dosyasını aşağıdaki gibi yapılandıralım.

~~~
sudo vi /etc/ovirt-guest-agent.conf

    [virtio]
    device = /dev/virtio-ports/ovirt-guest-agent.0
~~~

Son olarak aşağıdaki komutları sırasıyla çalıştırın. İşlem bu kadar.

~~~
sudo systemctl enable ovirt-guest-agent.service
sudo systemctl start ovirt-guest-agent.service
sudo systemctl status ovirt-guest-agent.service
~~~

