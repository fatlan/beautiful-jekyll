---
layout: post
title: Openstack Client Install and openstack.rc File Configuration
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/25-26-20-uelinuxegitim/25-26-20le05.png
#share-img: /assets/img/path.jpg
tags: [openstack, cloud, bulut, pip, python]
comments: true
---

İlgili kurulumları yapalım
~~~
sudo apt install python3-dev python3-pip

sudo pip3 install --upgrade pip

pip3 --version

sudo pip3 install python-openstackclient

pip3 show python-openstackclient

which openstack
~~~

Akabinde **rc** dosyasını oluşturup, yapılandıralım.
~~~
vi openstack.rc
~~~
~~~
bash
# Ansible managed
export LC_ALL=en_US.UTF-8

# COMMON CINDER ENVS
export CINDER_ENDPOINT_TYPE=publicURL

# COMMON NOVA ENVS
export NOVA_ENDPOINT_TYPE=publicURL

# COMMON MANILA ENVS
export OS_MANILA_ENDPOINT_TYPE=publicURL

# COMMON OPENSTACK ENVS
export OS_ENDPOINT_TYPE=publicURL
export OS_INTERFACE=publicURL
export OS_USERNAME=fatlan@fatlan.com
echo "Please enter your OpenStack Password for user $OS_USERNAME: "
read -sr OS_PASSWORD_INPUT
export OS_PASSWORD=$OS_PASSWORD_INPUT
export OS_PROJECT_NAME=fatlan@fatlan.com
export OS_TENANT_NAME=fatlan@fatlan.com
export OS_AUTH_TYPE=password
export OS_AUTH_URL=https://bulut.fatlan.com:5000/v3
export OS_NO_CACHE=1
export OS_USER_DOMAIN_NAME=Default
export OS_PROJECT_DOMAIN_NAME=Default
export OS_REGION_NAME=RegionOne
export OS_DOMAIN_NAME=Default

# For openstackclient
export OS_IDENTITY_API_VERSION=3
export OS_AUTH_VERSION=3
export PS1='${debian_chroot:+(debian_chroot)}\[\033[01;31m\]\u@\h\[\033[01;34m\](openstack_admin)\[\033[01;41m\]\$\[\033[00m\] '
~~~

Komutları yürütmekte rahat etmek için **bash completion**'u aktif edelim.
~~~
openstack complete | sudo tee /etc/bash_completion.d/osc.bash_completion > /dev/null

source openstack.rc
~~~

Bazı komutlar
~~~
openstack network list

openstack project list

openstack server list

openstack keypair list

openstack volume type list

openstack image list
~~~


