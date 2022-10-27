---
layout: post
title: Install Python Pip Packages Without Internet
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/25-26-20-uelinuxegitim/25-26-20le05.png
#share-img: /assets/img/path.jpg
tags: [python, pip, intranet, local]
comments: true
---

**pip** ile **ceph-deploy.tar.gz** paket kurulumunu, internetsiz ortamda nasıl yapılacağını ele alacağız.

~~~
pip download requirements.txt -d "my-deps-packages"

pip install -r requirements.txt --find-links=my-deps-packages ceph-deploy.tar.gz [--no-index --no-deps]
~~~

ref: [stackoverflow](https://stackoverflow.com/questions/36725843/installing-python-packages-without-internet-and-using-source-code-as-tar-gz-and)