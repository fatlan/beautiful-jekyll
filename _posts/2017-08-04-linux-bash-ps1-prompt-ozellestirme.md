---
layout: post
title: Linux Bash (PS1) Prompt Özelleştirme
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [promt, shell, bash, linux]
comments: true
---
Bu işlem için atanan parametre PS1 değişkenidir.

Konsol yada ssh ile Linux sunucusuna login olduğunuzda komut satırı genelde standart aşağıdaki gibi açılır.

![Crepe](/assets/img/linux-shell-promt-gen/lin-cus-promt01.png)

**Ubuntu** da aşağıdaki gibi,

![Crepe](/assets/img/linux-shell-promt-gen/lin-cus-promt02.png)

Biz bunu özelleştirip kendimize göre daha ilgi çekici hale getireceğiz.

**echo #PS1** derseniz halihazırda kullanılan değişken parametrelerini görebilirsiniz.

![Crepe](/assets/img/linux-shell-promt-gen/lin-cus-promt03.png)

Değiştirmek için **PS1** değişkenini aşağıdaki gibi komut satırına girebilir. Bu işlemin kalıcı olması içinse kullanıcı profili **.bashrc** dosyasına yazmalısınız.

~~~
PS1="\[\033[02;32m\]\u@\H:\[\033[02;34m\]\w\$\[\033[00m\]"
~~~

![Crepe](/assets/img/linux-shell-promt-gen/lin-cus-promt04.png)

~~~
PS1="\[\e[31m\]\u\[\e[m\]\[\e[32m\]\H\[\e[m\]\[\e[33m\]\\$\[\e[m\]"
~~~

![Crepe](/assets/img/linux-shell-promt-gen/lin-cus-promt05.png)

~~~
PS1="\[\e[31;42m\]\h\[\e[m\]"
~~~

![Crepe](/assets/img/linux-shell-promt-gen/lin-cus-promt06.png)

~~~
PS1="\d\[\e[32m\][\[\e[m\]\[\e[31;40m\]\h\[\e[m\]\[\e[32m\]]\[\e[m\] "
~~~

![Crepe](/assets/img/linux-shell-promt-gen/lin-cus-promt07.png)

~~~
PS1="${debian_chroot:+($debian_chroot)}\[\033[01;31m\]\h\[\033[01;34m\] \W \$\[\033[00m\]"
~~~

![Crepe](/assets/img/linux-shell-promt-gen/lin-cus-promt08.png)

gibi gibi,

Bu işlemi kendiniz daha custom hale getirebilirsiniz. Bunu aşağıda paylaşacağım link ile çok kolay ve eğlenceli olarak yapabilirsiniz.

[https://scriptim.github.io/bash-prompt-generator/](https://scriptim.github.io/bash-prompt-generator/)