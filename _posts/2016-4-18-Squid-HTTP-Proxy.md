---
layout: post
title:  "Build Your Own HTTP Proxy Using Squid"
categories: proxy
tags:   http-proxy squid ubuntu
---

使用Squild搭建HTTP正向代理
-----

由于老婆回国后，还想继续使用Gmail。之前一直用的Goagent貌似也因为大部分的Google App Engine（GAE） IP地址被屏蔽，大部分情况下没法正常使用，Github上的项目也已经停止维护。

网上搜索到的的其他的免费代理，又总感觉不安全。

所以就想自己搭建一个HTTP代理服务器，方便家人使用。

#### 下载安装

我的代理服务器是搭在硅谷的阿里云上，服务器版本是Ubuntu 14.4 LTS，直接使用apt-get下载安装。

```bash
# installation
$ sudo apt-get install squid
```

#### 更改配置（可选）

安装完成之后，Squid3已经被注册为Linux系统服务，运行在后台了。

只不过基于Squid3默认的配置，只允许本地Localhost使用该代理，并且监听3128端口作为HTTP 代理。

可以通过修改配置文件，修改监听端口，增加访问控制等。

不过对于本次翻墙的需求，完全使用的是默认的配置。

```bash
# 配置文件路径
$ sudo vi /etc/squid3/squid.conf

# HTTP监听端口
http_port 61112

# 基于IP地址的权限控制
# 将请求源地址为31.149.197.0 ～ 31.149.197.255的计算机归类为yourownclient组
acl yourownclient src 31.149.197.0/24

# INSERT YOUR OWN RULE(S) HERE TO ALLOW ACCESS FROM YOUR CLIENTS
# 允许yourownclient组的请求使用该代理服务器
http_access allow yourownclient

# Shutdown squid3 
$ sudo service squid3 stop

$ sudo chromium-browser --proxy-server="47.88.xxx.xxx:61112"

```

#### 建立加密通道

但是仅仅使用代理服务器， 由于请求内容是明文，依然会被GFW拦截掉。 想想我大天朝为了愚民，也真是煞费苦心啊。

所以需要在客户机与代理服务器之间，建立一条加密通道。

一般的推荐的方案是： Squid + Stunnel

 1. openssl生成证书
 2. 在服务器端，使用Squid搭建代理
 3. 使用第一步生成的证书，监听HTTPS端口 
 4. 在客户端，安装Stunnel，使用同样的证书加密数据。
 5. 由Stunnel监听本地端口，将请求加密后请求至Squid代理服务器。

缺点是：

- 需要在客户端安装Stunnel，并且还有修改配置。
对于我老婆这种电脑盲来说，还不如要了她的命。

所以我选择了使用[SSH端口转发][1]的方式，来构建加密通道，与第一种方式的不同在于：

 1. 不需要下载配置Stunnel
 2. 下载SSH客户端[Putty.exe](https://the.earth.li/~sgtatham/putty/latest/x86/putty.exe)
 3. 将如下命令保存为批处理文件Gfw.bat
 start putty.exe -L 8888:127.0.0.1:3128 gfw@47.88.XX.XX -pw 123456
 4. 运行gfw.bat，即可建立加密通道

缺点也很明显：

 - 脚本中包含的服务器的用户名密码
   需要服务端[新建一个没有任何权限的新用户][2]
 - 有一定的安全风险


[1]: www.baidu.com 
[2]: http://fanshaohua-fan.github.io/ubuntu/ubuntu-users/


> Written with [StackEdit](https://stackedit.io/).