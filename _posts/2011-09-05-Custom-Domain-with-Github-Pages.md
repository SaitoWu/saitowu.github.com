---
layout: post
title: Custom Domain with Github Pages
category: note
excerpt: use your own domain with github pages
---

### 先决条件:

* 自己已经有可以访问的github pages.

访问路径为: http://usename.github.com

自己的pages需要在github里面有一个名为 usename.github.com 的repo.

里面放入静态有页面,就可以被github serve起来了.

可以参考我的repo: git://github.com/SaitoWu/saitowu.github.com.git

* 有自己独立的域名.(这一条件是为了custom自己的域名.

like this: http://saito.im

### 实施:

* first step:

在自己的repo的root下建立一个CNAME的文件:

    touch CNAME
    
文件的内容为:

    saito.im
    
替换为你的域名.

* second step:

在自己域名服务商的网站里更改自己的域名DNS.

    HostName    RecordType    Address
    @           A(Address)    207.97.227.245
    
207.97.227.245是github pages的地址.

* third step:

耐心等待,有两方面的等待.

一个是自己域名服务商dns flush的等待.确保自己ping到自己域名的地址与207.97.227.245相同.

另一个则是github自身的延迟.这个大概有10分钟左右.

域名服务商dns的刷新因人而异.

### 完成:

等待过后就可以用新域名访问了.

you are the one!

thanks github! 
