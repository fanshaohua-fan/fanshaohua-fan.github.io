---
layout: post
title:  "Docker 学习笔记：logs"
categories: docker
tags:   docker log
---
###Docker 学习笔记：logs

[OPTIONS]:

None

没有任何参数，打印该容器的所有日志

-f

最常用的指令，类似tail的f选项，显示日志以及更新

--since

显示从某个timestamp开始的日志

-t

同时显示timestamps

--tail=n

从日志末尾的第n行显示

下面是一些样例：

```bash
$ docker logs --help
Usage:	docker logs [OPTIONS] CONTAINER

Fetch the logs of a container
  -f, --follow=false        Follow log output
  --help=false              Print usage
  --since=                  Show logs since timestamp
  -t, --timestamps=false    Show timestamps
  --tail=all                Number of lines to show from the end of the logs

$ docker logs -f --tail=10 flask 
 2015-11-25   15:19:44  杭州市  海关放行    
 2015-11-25   15:19:45  杭州市  离开处理中心,发往台州市     
 2015-11-26   10:41:54  台州市  离开处理中心     
 2015-11-26   14:12:53  临海市分公司  安排投递    
 2015-11-26   17:25:06  临海市分公司  未妥投    
 2015-11-26   19:26:50  临海市分公司  到达处理中心    
 2015-11-27   08:43:25  临海市分公司  安排投递    
 2015-11-27   11:53:42  临海市分公司  投递并签收
--------------------------------------------------------------------------------

$ docker logs -t --since='2015-11-27' flask
2015-11-27T09:29:18.872591141Z 192.168.42.1 - - [27/Nov/2015 09:29:18] "GET / HTTP/1.1" 200 -
2015-11-27T09:29:32.059603285Z 82.217.156.213 - - [27/Nov/2015 09:29:32] "GET / HTTP/1.1" 200 -

```