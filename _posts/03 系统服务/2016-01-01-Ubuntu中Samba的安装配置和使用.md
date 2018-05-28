---
layout: post
title:  "Ubuntu中Samba的安装配置和使用"
categories: 系统服务
tags:  系统服务
author: bloglink
---

* content
{:toc}

> Samba服务在Ubuntu服务器版本中默认并没有安装,其安装使用如下:



### 1 Samba软件包的安装
使用源安装，在终端中输入如下命令：

    #sudo apt-get install samba
    #sudo apt-get install smbclient

### 2 Samba的启动

启动Samba服务器只需执行如下命令：

    #sudo /etc/init.d/samba start

关闭Samba服务器：

    #sudo /etc/init.d/samba stop

重新启动Samba服务器：

    #sudo /etc/init.d/samba restart

启动Samba服务器后，可以使用ps命令查看进程：

    #ps -aux

可以看到Samba服务会同时启动两个服务，其中smbd主要用来管理共享出来的目录，nmbd主要用来解析NetBIOS名。在Windows系统中， 主机可以被加入一个组中，这样每个主机都必须有一个名字，这个名字是用于在网上被标志的名，并非机器的主机名，将其称为NetBIOS名。其中nmbd进 程是随着smbd进程启动而启动。

### 3. 配置Samba服务

Samba服务器主要配置文件为/etc/samba/smb.conf，并且可以将NetBIOS名与主机的对应关系写在/etc/samba /lmhosts文件中。

（1）在Windows系统中不用输入密码访问Linux共享目录

在Linux共享一个目录，将建立好的目录的设置信息写入/etc/smb.conf文件即可。如：若共享/home/myth/share目录，要在 Windows系统中访问这个共享的目录，假设Windows主机的IP为192.168.0.11，Linux主机的IP为192.168.10，进行 如下操作：

    #mkdir /home/myth/share
    sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.bak       //修改配置文件之前最好做个备份
    #vim /etc/samba/smb.conf  

或者使用

    sudo gedit /etc/samba/smb.conf

打开配置文件

将文件中的内容做如下相应修改：

    #security=user

后面添加：

    security=share

在文件结尾添加如下行：

    [share]
    comment=this is Linux share directory
    path=/home/myth/share
    public=yes
    writable=yes

保存退出，启动Samba服务：

    #/etc/init.d/samba start

设置完成！

在Windows 下访问共享目录，可点击运行，输入

    \\192.168.0.10\share

这样就能以匿名用户访问共享目录share了。

*关于Windows下无写权限：chmod -R go+rwx share/*
