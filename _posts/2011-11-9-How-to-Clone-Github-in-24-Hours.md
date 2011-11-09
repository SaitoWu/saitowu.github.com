---
layout: post
title: How to Clone Github in 24 Hours
category: note
excerpt: 24 minites 24 seconds
---

work very very hard before the deadline, then read me again.

### git仓库管理的局限

git在基于ssh的auth机制下.

如果 用户A 需要从 用户B 的git repo里pull或者push代码,

需要运行命令:

`ssh-copy-id`

copy id至 用户B 的.ssh目录的`authorized_keys`中.

git本身作为一个去中心化的vcs.server与本地repo并没有本质区别.(server一般以bare仓库起手.

用上面的方法来管理git repos简单方便,但是确有很大的局限.

1> 如果要做细粒度的权限控制,需要为每个repo建立一个用户.要不然没法区分不同repo的访问权限.

2> 当建立不同用户后,所有git repo的访问便会出现不一致.例如git@gitlabhq 或是 svn@github.

### git仓库管理gitosis or gitolite

由于gitosis已经年久失修,建议选择到gitolite.

gitolite主要做的事情就是可以让你在一个用户下管理所有的repo.

并且每个repo都可以做细粒度的操作授权.

使用惯例(github)是:建立一个名为 git 的用户.然后为每个用户建立单独的目录,避免不同用户的repo名冲突.

这样你就深度clone了github的repo地址:

`git clone git@github.com:SaitoWu/rules-engine.git`

做到这一步,即使没有github的页面.你已经可以声称自己可以host代码了.

### git repo viewer

用来操作git的源码树的方法大致有这么几种:

1> 利用system call调用git命令取得返回值然后parse.

2> 利用ruby-git来操作.

3> 也是你需要做的选择,利用grit.

grit提供一种对象化的操作方法来获取整个repository对象.

    require 'grit'
    
    repo = Grit::Repo.new("/home/saito/devel/blackmine")
    #获取源码树
    tree = repo.commits.first.tree
    #获取近10次提交
    commits = repo.commits
    #获取blob内容
    blob = tree.contents.first
    blob.data

拥有这些内容可以很轻松的构建源码阅读树.

拥有这个界面就可以几乎忽悠住大部分noob了.

### code review

利用grit取得每次commit的内容后,就可以进一步做code review.

review的评论通过邮件通知作者.

深度模仿github的code review的话, read me again and again.

### git request-pull.

git本身是支持pull request的.

详细需要察看 `git request-pull`

如果了解了实现原理,肯定就知道了github为什么需要有fork功能.

review pull request的时候是在fork上review的.

如果需要就直接merge.

要clone到github如此细心.read me again.

### issue driven, wiki showoff.

称之为issue驱动的开发模式.

如何issue驱动,自己写issue,自驱;别人提issue,四驱.

issue主要实现一个tag系统就可以了.然后做好基于tag的搜索.

传统软件开发流程如何适应或不适应这个issue驱动的开发模式暂且不表.

it just works for me.

wiki的实现现在markdown似乎已经成为了标准.

rdiscount or bluecloth.

### 肯定没到24小时.

你出师了!
