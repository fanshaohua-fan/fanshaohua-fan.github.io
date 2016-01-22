---
layout: post
title: "Ubuntu 14.04 Chromium导致黑屏"
categories: ubuntu
tags:	 ubuntu chromium
---
Ubuntu 14.04 Chromium导致黑屏
----------

自从工作电脑切换到Ubuntu之后，被这个问题困扰了好久。

一直没有静下心来研究一下是什么问题。
今天恰好有时间，搜索了一下解决方案，幸好也解决了，记录下来方便有需要的人。

解决步骤：
1. 点击设置Setting
2. 在搜索框Search Setting，输入acceleration
3. 取消硬件加速“Use hardware acceleration when available”
4. 重启浏览器Chromium

参考:
[Issue 443424: 	Ubuntu goes black screen when viewing some webpages](https://code.google.com/p/chromium/issues/detail?id=443424)



> Written with [StackEdit](https://stackedit.io/).