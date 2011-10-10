---
layout: post
title: How to Be a Successful Archer
category: note
excerpt: em..must install archlinux first!
---

### 前情提要:

为什么选择arch,而不是ubuntu,fedora,linux mint,gentoo or others?

三个条件:

Binary based : 没那么多时间等编译.

Rolling relese : 免去半年重装的尴尬.

Repository : 新版本的软件用不上的寂寞谁人知.

### 开始安装:

备齐一份 2011.08.19的iso文件.

基本的iso启动后安装看英文就能解决.

修改/etc/pacman.conf
    #yaourt
    [archlinuxfr]
    Server = http://repo.archlinux.fr/x86_64

安装gnome 3.2:
    pacman -Syu
    # or xf86-video-nv
    pacman -S xorg-xinit xorg-server xf86-video-ati
    pacman -S gnome gnome-extra
    
修改/etc/rc.conf:
    DAEMONS=(... dbus ... gdm @networkmanager)
    MODULES=(fuse)

修改/etc/inittab:
    # 同类型只保证这一个打开,下同
    id:5:initdefault:
    x:5:respawn:/user/sbin/gdm -nodaemon

新建用户:
    useradd -m -s /bin/bash saito
    passwd saito

新建xinit文件:
    su - saito
    echo 'exec gnome-session' >> .xinitrc

reboot:
    sudo reboot

### 安装成功:

empthy:
    sudo pacman -S telepathy telepathy-gabble telepathy-butterfly

touchpad驱动:
    sudo pacman -S xf86-input-synaptics

ibus(开机启动项添加ibus-deamon):
    sudo pacman -S ibus ibus-sunpinyin

wireless(b43系列):
    pacman -R broadcom-wl
    yaourt b43-firmware
    #rc.conf添加: DEAMONS(... b43 ...) 确保没有wl

### 字体设置:

效果到现在仍然不是很理想:

常用字体:
    yaourt ttf-ms-fonts
    pacman -S font-bh-ttf ttf-arphic-ukai ttf-arphic-uming ttf-bitstream-vera ttf-cheapskate ttf-dejavu ttf-fireflysung ttf-ms-fonts wqy-bitmapfont wqy-zenhei

打过ubuntu补丁的渲染:
    yaourt -S cairo-ubuntu libxft-ubuntu freetype2-ubuntu fontconfig-ubuntu

通用字体配置文件:
    # http://www.box.net/shared/bflybsxjf6
    tar zxf fontconfig-7.tar.gz
    cd fontconfig
    makepkg -si

选择mac的文字渲染效果:
    sudo ln -sf /etc/fonts/conf.avail/48-localmac.conf /etc/fonts/conf.d/48-localmac.conf

### bug or feature:

Gnome 3.2 登陆界面 单击用户头像后假死:
    直接登陆,跳过密码输入阶段后解决

Gnome 3.2 音量控制每次重新开机后都为mute状态:
    暂时未解决.

Gedit 的 Gmate插件集不能用了:
    转战vim.
