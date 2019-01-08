---
layout: post
title: "Linux 命令 : Xargs"
categories: linux
tags:	 linux shell, bash
---

Xargs占位符：

```bash
find . -name "*png" | xargs -I{} cp {} /c/fanshaohua-fan.github.io/images
```