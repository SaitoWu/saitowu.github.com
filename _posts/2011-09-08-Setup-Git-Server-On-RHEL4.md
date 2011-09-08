---
layout: post
title: Set Up Git Server On RHEL4
category: note
excerpt: what a f*cking env
---

### 前情提要:

公司的机器是RHEL4.

还好有yum可以用.但是Source里神马都木有.

### 编译安装Git

`sudo yum install zlib-devel gettext-devel openssl-devel curl-devel expat-devel`

在我当前环境下只有gettext需要安装.

还好公司内部的Source里面有. 一切顺利.

`wget http://kernel.org/pub/software/scm/git/git-1.7.6.tar.gz`

`tar -xzvf git-1.7.6.tar.gz`

`cd git-1.7.6 `

`make prefix=/usr/local all` 

`sudo make prefix=/usr/local install` 

编译挺快的,一切顺利.谢天谢地git依赖的东西这么质朴.

### 配置Git Server

git有四种server的方式: local ssh git http

这里用ssh的方式就可以了.

`mkdir -p repo/project.git`

`cd repo/project.git`

`git --bare init`

### 收集id_rsa.pub

收集团队成员的id_rsa.pub文件.

`cat all_keys >> ~/.ssh/authorized_keys`

大功告成.

### 团队成员自身方便配置

`touch ~/.ssh/config`

填入下面的内容:

    Host gitserver
    HostName x.x.x.x
    User user

如果没有上面的这一行我们需要这样clone:

`git clone user@x.x.x.x:~/repo/project.git`

有了之后就可以这样了:

`git clone gitserver:~/repo/project.git`

对于其他的ssh访问,类似的配置也有效.

### First Blood

这时候你要做的是以极快的手速输入下列命令:

    git clone gitserver:~/repo/project.git
    touch README.md
    cat "first blood" >> README.md
    git commit -am "first blood"
    git push -u origin master

first blood 就是你的了.

建议做git config配置.可以简化命令的输入.简化后可像这样:

    git ci -am"first blood"
    git pom

so.. 作为一个Server 架设者, 把一血还是留给自己吧..
