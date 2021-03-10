---
title: 使用vsftpd建立FTP服务器
date: 2020-06-23 18:10:45

tags:
- FTP

categories: 
- Linux
---

VSFTPD是一个FTP服务器软件，官网的介绍是：

> **Probably the most secure and fastest FTP server for UNIX-like systems.**



一般在Linux建立FTP服务器都用他，首先要安装vsftpd：

debian系：

`sudo apt install vsftpd`

arch系：

`sudo pacman -S vsftpd`

可以使用man查看配置说明：

`man vsftpd.conf`



网上很多过时的文章说配置文件是`/etc/vsftpd/vsftpd.conf`，但现在的配置文件是/etc/vsftpd.conf，只要用man看看说明就知道了，说明里有指出配置文件的路径。



首先使用编辑器修改配置文件：

`sudo vi /etc/vsftpd.conf`

找到以下的项目并修改：

允许本地用户登录：

`local_enable=YES`

允许写操作：

`write_enable=YES`

下面三项设置控制用户能否离开默认目录：

`chroot_local_user=YES`若为YES则下面的list里指定的用户能离开默认目录

`chroot_list_enable=YES`若为NO则上面即使是YES也无效，因为等效于没有指定哪些用户可以离开。

`chroot_list_file=/etc/vsftpd.chroot_list`指定用户的列表路径



是否显示隐藏文件：

`force_dot_files=YES`



修改好后保存配置文件，并重启vsftpd服务：

`sudo systemctl restart vsftpd`



添加用于访问的用户：

`useradd <username>`

设置密码：

`sudo passwd <username>`



默认的root路径是`/home/$USER/`

所以要在/home目录里面创建一个跟<username>同名的文件夹：

`sudo mkdir <username>`

更改文件夹的权限：

`sudo chmod 755 <username>`

还要更改文件夹的所有者：

`chown -R <username> /home/<username>`



到这里就可以使用filezilla等客户端访问这个FTP服务器并传输文件了

安装filezilla之后，打开软件

去到文件-站点管理器：

![filezilla](/images/filezilla.png)

填入你的服务器地址和用户名点击连接就可以访问建立的服务器了，可以直接拖拽来传输文件。



要注意的是服务器接收的软件可能你的默认用户没有权限访问，这是因为没有这些文件的读写权限，可以使用`chmod 644 <filename>`来改变文件的权限，或者配合`find`来批量修改





参考资料：

[使用vsftpd 搭建ftp 服务器](https://segmentfault.com/a/1190000016376962)

[VSFTPD, 553 Could not create file. - permissions?](https://unix.stackexchange.com/questions/39466/vsftpd-553-could-not-create-file-permissions)