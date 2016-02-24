---
layout: post
title:  "How to install citrix receiver on Ubuntu 14.10"
categories: ubuntu
tags:   citrix linux ubuntu 
---

How to install citrix receiver on Ubuntu 14.10
-----

#### 安装

 1. 下载最新版本的 [citrix receiver for linux](http://www.citrix.com/downloads/citrix-receiver/linux.html) ，选择64-bit deb版本，点击 Download Now
 2. 双击下载完成的deb文件（Chrome默认下载路径在~/Downloads文件夹中），默认会跳转至软件中心（Ubuntu Software Center）
 3. 点击安装（Install），等待安装完成

#### 使用

 1. 输入登录地址，登录页面如下
 2. 输入用户名密码
 3. 输入手机收到的验证码

#### 常见错误

 - 证书错误，由于Citrix receiver初始信任的证书并不多；如果使用icaclient不信任的证书，则可能遇到这个错误
  1. 进入Citrix Receiver的可信任证书目录(Trusted store)
  2. 如果对于浏览器（Chrome/Firefox）站点是可信任的话，则可到/usr/share/ca-certificates/mozilla/找到对应的证书
  3. 将证书复制到icaclient的可信证书中

```bash
ubuntu:/opt/Citrix/ICAClient/keystore/cacerts$ sudo cp  /usr/share/ca-certificates/mozilla/Staat_der_Nederlanden_Root_CA_-_G2.crt .
```

- 退出全屏在Ubuntu中很是麻烦：
    - 先按 Ctrl + F2
    - 再按 Ctrl + Win + 方向下