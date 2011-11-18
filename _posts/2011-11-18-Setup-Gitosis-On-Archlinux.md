---
layout: post
title: Setup Gitosis on Archlinux
category: note
excerpt: gitosis sucks
---

### install openssh

    sudo pacman -S openssh

add it to deamon /etc/rc.conf.

    DEAMON(... sshd ...)

start sshd or maybe u need restart ur machine.

### add a new user git

    sudo useradd -m -s /bin/bash git

add git to visudo

    git ALL=(ALL) NOPASSWD:ALL

### generate a ssh key

    sudo su git
    ssh-keygen -t rsa

### install gitosis

    git clone git://eagain.net/gitosis.git
    cd gitosis
    python setup.py install

### init gitosis

    sudo -H -u git gitosis-init < ~/.ssh/id_rsa.pub
    sudo chmod 755 /home/git/repositories/gitosis-admin.git/hooks/post-update

### done

    git clone git@localhost:gitosis-admin.git
