---
layout: post
title: Drawbacks of Git
category: note
excerpt: Git Encoding Submodule HTTP
---

# Git:

### Encoding:

  Encoding 是 VCS 系统该考虑还是不该考虑的?

  Git 告诉我们不需要: [ git commit log ](http://www.kernel.org/pub/software/scm/git/docs/v1.5.0.2/git-log.html) .

  > At the core level, git is character encoding agnostic.

  > The pathnames recorded in the index and in the tree objects are treated as uninterpreted sequences of non-NUL bytes. What readdir(2) returns are what are recorded and compared with the data git keeps track of, which in turn are expected to be what lstat(2) and creat(2) accepts. There is no such thing as pathname encoding translation.

  > The contents of the blob objects are uninterpreted sequence of bytes. There is no encoding translation at the core level.

  > The commit log messages are uninterpreted sequence of non-NUL bytes.

  > Although we encourage that the commit log messages are encoded in UTF-8, both the core and git Porcelain are designed not to force UTF-8 on projects. If all participants of a particular project find it more convenient to use legacy encodings, git does not forbid it. However, there are a few things to keep in mind.


  对比一下其他 VCS 系统, [HG](http://mercurial.selenic.com/wiki/EncodingStrategy) 与 [SVN](http://www.tigris.org/scdocs/SVNEncoding) .

  简单来说: 一个没有对 commit log 与 path 做 encoding 的 VCS 系统会有两方面的问题.

  第一, 万恶的 windows 存在各种奇葩的 encoding, 这需要不同语言之间做兼容, 不然会因为编码的原因无法 clone 成功或者无法知道 commit log 具体是什么内容.( 一个 workaround 的办法是: 大家都用 msysgit patch 过的 utf8 版本的 Git Client) .

  第二, 作为一个 Github Like 系统的开发人员会做的很辛苦. 因为没人知道 push 到仓库的代码是什么编码, 甚至路径与提交信息都是别种的编码. 所以几乎处处都需要 detect 编码, 以及转换, 效率很差. 因为 Git 客户端没有在最适合的时候做最应该做的处理, 所以导致越外层效率越差, 而且效果不好. [Github](https://github.com/holman/feedback/issues/191)

### SubModule:

  submodule 作为一个相对于 svn 的 co 子目录被开发出来. 现阶段除了在 vim 插件领域大家应用广泛之外. 在现实开发领域很少有人会使用. ( 有使用经验的可以举手. 分享一下经验. )

  值得欣喜的是 subtree 已经被开发出来并 merge 到了 master. 这个值得期待. 是否实用暂时不清楚.

### HTTP Support:

  Git 在 1.6 之后才开始支持 smart http transport 协议, 并且在最近的一个发布版里面增强了 http 协议的功能, 但是还是不够. 在最近的 1.7 发布版里面 Git 默认支持了短时效的认证信息缓存功能, 默认有15分钟. 对于 Mac 版则可以将认证信息存储在 key-chain 里面 ( 貌似需要打patch. ).

  现阶段大家在 workaround 的方法一般都是: 在 home 下面 新建 .netrc 文件, 利用 curl 的附带认证信息的能力, 完成默认的认证. 否则每次提交都需要重新输入密码. ( 如果觉得每次输入密码这样安全的话, 可以忽略这一条. 或者你本身 ssh-keygen 之前都是需要输入密码的, 也可以略过. )


### Feature:

最后, 我最期待的是: Git 2.0 能解决 Encoding 的问题, 当然这个涉及到很多打破兼容性的问题, 虽然可能性非常小, 但是真的值得做.


最后, 欢迎补充与各种批评指正.
