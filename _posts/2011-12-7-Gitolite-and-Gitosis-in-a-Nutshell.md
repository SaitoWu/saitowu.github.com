---
layout: post
title: Gitolite and Gitosis in a Nutshell
category: note
excerpt: ssh triggers save the world!
---

如果对于Gitosis/Gitolite不是很了解,有前置篇:

[How to Clone Github in 24 Hours](http://saito.im/note/How-to-Clone-Github-in-24-Hours/)

### 小历史

Gitolite作者受Gitosis的影响.用perl实作了一个lite版的Gitosis.

但是后来发现功能越来越多,现在已经比Gitosis还要强大了.也早已不lite了.

Gitosis现在也已经不再维护,或者说没有动静了. 但是他们的原理其实是一样的.

### 如果失去了Gitolite世界会怎么样

像github这样的靠Host Repos赚钱的公司,如何才能给成千上万的用户提供服务呢?

它要为每个人在github服务器上建立一个用户,然后将用户的key添加到对应服务器账户的authorized_keys里面.

如果该用户需要collaborator,ok,需要把collaborator同样添加到authorized_keys里面.

这样的话所有人clone repo就会变成类似这样的方式: `git clone saito@github.com:repo.git`

github现在clone的方式是: `git clone git@github.com:saito/repo.git`.

所以基本可以确定github肯定不至于这么圡.

如果用户只需要这个collaborator有自己一个repo的读写权限,怎么办? 对不起,没办法.

一旦给collaborator放权,他就拥有了该用户所有repo的控制权限.这样几乎是不可用的.

依靠这种为每个人建立用户的方法管理几乎是不可维护的.如果没有类似Gitolite的产品,也不可能造就github这样的公司.

### Git默认是ssh协议

既然是ssh协议,那么,用户访问服务器就要用类似ssh-copy-id的方式把自己的key加入服务器的authorized_keys里面.

从github的clone repo的方式,基本可以确定,github在服务器上是有一个名为git的用户在管理所有人的repo的.

如果只有这么一个用户,所有人的key都要被添加到git用户的authorized_keys里面.那么如何做权限控制呢?

### SSH triggers拯救世界

假想我们拿到一个正常authorized_keys文件:

`ssh-rsa key-blob git@github`

这里我省略了长长的key-blob.

Magic Thing:

`command="mkdir -p ~/ssh/boom" ssh-rsa key-blob git@github`

将authorized_keys文件行头加上`command="mkdir -p ~/ssh/boom"`

保存authorized_keys文件,重新ssh到服务器.

这时候神奇的时候发生了,用户的home下面将会得到一个~/ssh/boom目录.

利用自己的想象力,想想这个可以干什么坏事?

(提醒大家,安装软件要小心,bumblebee随时都可能出现在你身边.)

Gitlite/Gitosis用它干了什么好事呢:

截取一段Gitosis所控制的authorized_keys文件:

`command="gitosis-serve git@github",no-port-forwarding,no-X11-forwarding,no-agent-forwarding,no-pty ssh-rsa key-blob git@github`

no-pty的意思是不建立伪终端.

这里Gitosis所作的事情是调用自己写的一个函数/脚本: `gitosis-serve`, 然后传入用户的标识.

在脚本`gitosis-serve`中,Gitosis有一个参数是用户标识,还有用户来请求的命令.这里假设命令是:

`git clone git@localhost:repo.git`

gitosis-serve会利用正则截取用户要请求的repo,然后将用户标识与repo在自己的conf文件里做匹配.

看是否用户有相应的权限.

对应的命令还有pull push等也会有相应的R W权限做判断.

这样就把原本要在authorized_keys里做的权限控制,通过ssh triggers转移到了自己的脚本里面.

然后通过gitosis的配置文件来做所有项目的权限管理.

值得一题的是,Gitosis与Gitolite都将自己的管理文件也放在一个git repo里面.

所有对配置文件的操作也都变成了对一个git仓库的操作.通过hook来在更新配置后相应修改authorized_keys.

### 结语

希望可以有pure ruby的gem,这样结合grit就可以做更多更方便的管理了.

当然是对于rubyist来说..

ssh真的是常挖常新...
