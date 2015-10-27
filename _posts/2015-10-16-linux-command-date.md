---
layout: post
title:  "Linux 命令 : Date"
categories: linux
tags:   linux shell
---
#Linux命令 : Date

显示系统时间

```bash
$date
Fri Oct 16 16:05:23 CEST 2015
```

加减天数

```bash
$date -d "+1 day"
Sat Oct 17 16:05:47 CEST 2015
```

显示Unix 时间戳timestamp

```bash
$date +%s
1445004467
```
