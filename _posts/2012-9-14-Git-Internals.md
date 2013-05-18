---
layout: post
title: Git Internals
category: note
excerpt: Git Tag Commit Tree Blob
---

Git 一共有 4 种 SHA1. tag, commit, tree, blob. 每一种 SHA1 其实也代表了 Git 内部的一个基本对象.

稍微解释一下就知道了.

### 如果做下面的简单操作:

```bash
mkdir helloworld
cd helloworld
git init
echo hello world > readme.md
git add .
git commit -am"add readme.md"
```

### 在这里看来? 哪一步会创建哪些对象?

答案很简单.

`git add .`

在这里操作中会添加一个 blob 对象, 代表readme 文件本身. 这也是为什么你需要在 add 过一个对象之后, 撤销需要执行 rm 与 cache 相关命令的原因.

### blob

而文件的内容则如下所示.

```bash
helloworld git:master ❯ git ls-files -s readme.md
100644 3b18e512dba79e4c8300dd08aeb37f8e728b8dad 0 readme.md
helloworld git:master ❯ git show 3b18e512dba79
hello world
```

### tree

当执行下面这句的时候:

`git commit -am"add readme.md"`

这一步中将创建两个对象, 一个是当前work tree 的映射 tree 对象, 还有一个就是 commit 对象.

tree 对象是可以包含另外的 tree 以及 blob 的.

```bash
helloworld git:master ❯ git show 7394b8cc9ca
tree 7394b8cc9ca

readme.md
helloworld git:master ❯ git cat-file -p 7394b8cc9ca
100644 blob 3b18e512dba79e4c8300dd08aeb37f8e728b8dad  readme.md
```

上面是一个 tree 对象的具体内容.  tree 里面实际上就是描述了当前 tree 的内容以及 blob 的引用.

### commit

同理 commit 也可以用相同的方式查看:

```bash
helloworld git:master ❯ git show 84073a0
commit 84073a0bffd4c80598dbc4941a5f84d78cc2adcd
Author: Saito <saitowu@gmail.com>
Date:   Fri Sep 14 01:12:07 2012 +0800

    add readme

diff --git a/readme.md b/readme.md
new file mode 100644
index 0000000..3b18e51
--- /dev/null
+++ b/readme.md
@@ -0,0 +1 @@
+hello world
helloworld git:master ❯ git cat-file -p 84073a0
tree 7394b8cc9ca916312a79ce8078c34b49b1617718
author Saito <saitowu@gmail.com> 1347556327 +0800
committer Saito <saitowu@gmail.com> 1347556327 +0800

add readme
```

可以看到, commit 里面实际上保存的实际上是一个 tree 的 SHA1 以及 commit 的 author commiter 以及最终的 commit message.

### tag

最后我们给当前状态打一个 1.0 版本的tag.

`helloworld git:master ❯ git tag 1.0 -m"create a tag"`

再来看看 tag 的 SHA1 内容.

```bash
helloworld git:master ❯ git show 1518c0a02530b3
tag 1.0
Tagger: Saito <saitowu@gmail.com>
Date:   Fri Sep 14 01:24:39 2012 +0800

creat a tag

commit 84073a0bffd4c80598dbc4941a5f84d78cc2adcd
Author: Saito <saitowu@gmail.com>
Date:   Fri Sep 14 01:12:07 2012 +0800

    add readme

diff --git a/readme.md b/readme.md
new file mode 100644
index 0000000..3b18e51
--- /dev/null
+++ b/readme.md
@@ -0,0 +1 @@
+hello world
helloworld git:master ❯ git cat-file -p 1518c0a02530b3
object 84073a0bffd4c80598dbc4941a5f84d78cc2adcd
type commit
tag 1.0
tagger Saito <saitowu@gmail.com> Fri Sep 14 01:24:39 2012 +0800

create a tag
```

可以看到, tag 里面主要存储的是 commit 的 SHA1 值, tag name 以及 tagger, 还有, 如果有 -m 参数的话会有 tag message.


### 为什么没有 branch

因为 branch 不在这套体系内. 是体系外的.

跟基本对象相关的东西都存在 `.git/objects` 里面.

而 branch 是在同级的 `.git/refs` 目录里面的. 而 refs 目录里面, 每条 ref 记录, 实际上也是只记录了一个 commit SHA1 而已.

### 结论

四种对象创建完毕.. 回头再来看, 什么时候会有什么 SHA1 已经很清楚的.
