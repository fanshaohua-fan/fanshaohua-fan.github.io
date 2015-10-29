---
layout: post
title: "Linux命令: ln"
categories: linux
tags:	 linux shell
---
#Linux命令: ln

建立软连接:

```bash
$sudo ln -s ruby2.0 ruby
$ll | grep ruby
lrwxrwxrwx  1 root root           7 Oct 20 17:46 ruby -> ruby2.0*
-rwxr-xr-x  1 root root        6304 Nov 19  2014 ruby1.9.1*
-rwxr-xr-x  1 root root        6264 Nov 19  2014 ruby2.0*
```

删除软连接:

```bash
$sudo rm ruby
```
