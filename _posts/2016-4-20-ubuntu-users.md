---
layout: post
title:  "Manage users on Ubuntu"
categories: ubuntu
tags:   ubuntu linux
---

管理Ubuntu用户
-----

#### List All Users

查看用户信息：

```bash
$ more /etc/passwd
root:x:0:0:root:/root:/bin/bash
fan:x:1000:1000:,,,:/home/fan:/bin/bash
```

用户信息文件分析（每项用:隔开）

```bash
例如：fan:x:1000:1000:,,,:/home/fan:/bin/bash
fan　　	//用户名 
x　　	//口令、密码 
1000　　	//用户id（0代表root、普通新建用户从500开始）
1000　　	//所在组 
,,,　　	//描述 
/home/fan　　	//用户主目录 
/bin/bash　　	//用户缺省Shell
```

#### How To Add a User

新增用户

```bash
# add a new user
$ sudo adduser gfw
Adding user `gfw' ...
Adding new group `gfw' (1001) ...
Adding new user `gfw' (1001) with group `gfw' ...
Creating home directory `/home/gfw' ...
Copying files from `/etc/skel' ...
Enter new UNIX password: 
Retype new UNIX password: 
passwd: password updated successfully
Changing the user information for gfw
Enter the new value, or press ENTER for the default
	Full Name []: gfw
	Room Number []: 
	Work Phone []: 
	Home Phone []: 
	Other []: proxy
Is the information correct? [Y/n] y

# login as the new account
$ su - gfw
Password: 
gfw@iZu1lw5glrvZ:~$ 

# change password for the new account
$ passwd gfw
Enter new UNIX password: 
Retype new UNIX password: 
passwd: password updated successfully

```

#### How To Grant a User Sudo Privileges

设置账户权限

由于上文中的gfw账户仅用作建立ssh端口转发，不需要拥有管理系统的权限，所以不应该有系统管理(sudo)的权限。 

Ubuntu默认新增的用户是没有sudo权限的，所以不需要做任何额外的动作。

如果需要用户系统管理的权限，可以使用visudo命令，详见[参考资料2](https://www.digitalocean.com/community/tutorials/how-to-add-and-delete-users-on-an-ubuntu-14-04-vps).

> 参考资料：
> 
> 1. http://man.linuxde.net/passwd 
> 
> 2. https://www.digitalocean.com/community/tutorials/how-to-add-and-delete-users-on-an-ubuntu-14-04-vps

> Written with [StackEdit](https://stackedit.io/).