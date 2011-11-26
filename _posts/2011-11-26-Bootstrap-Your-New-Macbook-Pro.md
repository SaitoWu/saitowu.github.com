---
layout: post
title: Bootstrap Your New Macbook Pro
category: note
excerpt: mac os x is awesome
---

First thing first. install XCode.

### XCode

在mac appstore中安装xcode.

### Homebrew

[homebrew](https://github.com/mxcl/homebrew.git)是利用github作为平台的包管理工具,由ruby构建.

    /usr/bin/ruby -e "$(curl -fsSL https://raw.github.com/gist/323731)"

可以通过homebrew安装 git svn mongodb nodejs npm maven scala clojure等等.

类同与Archlinux的pacman, Ubuntu的apt-get, Freebsd的port.

### Terminal

[oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)是一个利用github维护的zsh脚本集合.提供丰富的主题和插件.

    wget --no-check-certificate https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh

zsh是bash的一个扩展,并且兼容bash命令.

推荐将theme设置为`wedisagree`

### Vim

mac os lion本身已经自带了vim7.3.但是没有GUI Vim.这里需要安装Macvim

    brew install macvim

利用[janus](https://github.com/carlhuda/janus) bootstrap自己的macvim.

    curl https://raw.github.com/carlhuda/janus/master/bootstrap.sh -o - | sh

### Color-Scheme

无论是terminal还是vim都需要有好的配色才会让你身心愉悦.这里推荐[solarized](http://ethanschoonover.com/solarized).

solarized分为dark和light两种配色.可以根据自己的喜好进行选择.

janus中已经附带了solarized配色方案,可以自行设置color-scheme.

terminal可以通过[terminal.app-colors-solarized](https://github.com/tomislav/osx-lion-terminal.app-colors-solarized)来安装配色.

### Misc

关于程序员的代码本地搜索,推荐ack来替代grep.

    brew install ack

关于开发环境的问题,rubyist已经习惯使用rvm或者rbenv来管理自己的ruby版本.

这里推荐[pythonbrew](https://github.com/utahta/pythonbrew)给各位pythonist,来管理python更为分裂的版本问题.

    pybrew install 2.7.2
    pybrew switch 3.2

Unix系统使用要顺手,很多dotfiles是不可或缺的.

[dotfiles](https://github.com/SaitoWu/dotfiles) > 这里有少量的dotfiles配置.

可以酌情修改拷贝至`~`目录下.

除了homebrew,其他的工具都同样可以bootstrap自己的Linux环境.

这里有一份[janus](https://github.com/SaitoWu/janus)的port,将command快捷键全部移植到了ctrl上.

gnome terminal同样有[solarized theme](https://github.com/sigurdga/gnome-terminal-colors-solarized).

### To be continued

github is awesome!
