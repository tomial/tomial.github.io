---
title: pacman、aur与yaourt的关系
date: 2019-09-25 11:36:52

tags:
- Arch

categories : 
- Linux
---

## PACMAN

Pacman在Arch wiki的说明如下：
> pacman软件包管理器是 Arch Linux 的一大亮点。它将一个简单的二进制包格式和易用的构建系统结合了起来(参见makepkg和ABS)。不管软件包是来自官方的 Arch 库还是用户自己创建，pacman 都能方便地管理。

简单来说，Linux与Windows不一样，Windows在没有Windows store之前安装软件都是通过直接下载安装包安装（不考虑某些XX管家），而Linux的发行版就像带有play商店的安卓和iOS，自带了一个play商店和App Store，能够安装/更新/管理软件包，还可以更新系统，这就是Linux的**包管理工具**，而**pacman**就是Arch这个发行版的包管理工具。

pacman可以做以下的事情：
- 从官方仓库安装/管理软件 `Pacman -S xxx`
- 安装离线的软件包 `Pacman -U xxx.tar.gz`
- 更新系统自身 `Pacman -Syu`
- ...
## AUR(Arch User Repository)
> Arch 用户软件仓库（Arch User Repository，AUR）是为用户而建、由用户主导的 Arch 软件仓库。AUR 中的软件包以软件包生成脚本（PKGBUILD）的形式提供，用户自己通过 makepkg 生成包，再由 pacman 安装。创建 AUR 的初衷是方便用户维护和分享新软件包，并由官方定期从中挑选软件包进入 community 仓库。本文介绍用户访问和使用 AUR 的方法。

可以看出，AUR里的软件包都是**用户创建**的，而pacman安装的软件包都是从**官方库/第三方镜像库**(如tuna)中安装的，从这里可以看出AUR中软件包的数量一定是十分庞大的，因为每个人都可以往里面发自己创建的软件包，但也不能保证软件包的质量。用户也可以为AUR里的软件包投票，如果一个软件包票数够高，也没有一些协议/打包质量的问题，则这个软件包有机会被官方库收录。

一般来说，安装AUR的软件包没有**Pacman -S**那么方便，用户需要在AUR中下载**软件包生成脚本（PKGBUILD 和其他相关文件）**，然后使用**makepkg**生成包，**再用pacman安装**，PKGBUILD里面包含了源码等生成软件包需要的内容，但还需要用户**自行构建软件包**。

## AUR工具

看完上面的内容你可能已经想到了，AUR工具就像AUR上的Pacman，他也可以通过一个简单的命令来帮你安装AUR上面的软件包。
比较流行的AUR工具有：
- yaourt(已经停止维护)
- yay
- aurman
- ...

具体可以查看arch官方的wiki页面:[AUR Helpers]( https://wiki.archlinux.org/index.php/AUR_helpers)

参考资料：[ArchWiki](https://wiki.archlinux.org/)