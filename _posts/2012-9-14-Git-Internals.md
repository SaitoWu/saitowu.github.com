```bash
mkdir helloworld
cd helloworld
git init
echo hello world > readme.md
git add .
git commit -am"add readme.md"
```
`git add .`
```bash
helloworld git:master ❯ git ls-files -s readme.md
100644 3b18e512dba79e4c8300dd08aeb37f8e728b8dad 0 readme.md
helloworld git:master ❯ git show 3b18e512dba79
hello world
```
`git commit -am"add readme.md"`
```bash
helloworld git:master ❯ git show 7394b8cc9ca
tree 7394b8cc9ca
readme.md
helloworld git:master ❯ git cat-file -p 7394b8cc9ca
100644 blob 3b18e512dba79e4c8300dd08aeb37f8e728b8dad  readme.md
```
```bash
helloworld git:master ❯ git show 84073a0
commit 84073a0bffd4c80598dbc4941a5f84d78cc2adcd
Author: Saito <saitowu@gmail.com>
Date:   Fri Sep 14 01:12:07 2012 +0800
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

`helloworld git:master ❯ git tag 1.0 -m"create a tag"`
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
而 branch 是在同级的 `.git/refs` 目录里面的. 而 refs 目录里面, 每条 ref 记录, 实际上也是只记录了一个 commit SHA1 而已.