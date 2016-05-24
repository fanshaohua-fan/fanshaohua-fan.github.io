---
layout: post
title:  "ELK: logstash"
categories: ELK
tags:   ELK logstash grok
---

ELK学习笔记之logstash
-----

#### Installing logstash

``` bash
# Logstash 依赖Java7，检测Java版本
$ java -version
java version "1.7.0_101"
OpenJDK Runtime Environment (IcedTea 2.6.6) (7u101-2.6.6-0ubuntu0.14.04.1)
OpenJDK 64-Bit Server VM (build 24.95-b01, mixed mode)

# Installing from Package Repositoriesedit
# Download and install the Public Signing Key
$ wget -qO - https://packages.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -

# Add the repository definition
$ echo "deb https://packages.elastic.co/logstash/2.3/debian stable main" | sudo tee -a /etc/apt/sources.list
$ sudo apt-get update && sudo apt-get install logstash
```

#### Basic Logstash example

``` bash
$ cd /opt/logstash
$ bin/logstash -e 'input { stdin { } } output { stdout {} }'
Hello World!
2016-05-24T13:34:42.423Z ubuntu Hello World!
```

#### Logstash pipeline

A Logstash pipeline has two required elements, input and output, and one optional element, filter. 

![](https://www.elastic.co/guide/en/logstash/current/static/images/basic_logstash_pipeline.png)

#### Input plugin: File

以特定文件为输入：

```
input {
#   stdin {}
    file {
        path => "/home/fan/sourcecode/elk/grok/access.log"
        start_position => beginning 
        ignore_older => 0 
    }
}

```

#### Output plugin: Stdout

先将输出指向到console，以方便前期验证。

```
output {
    stdout {
        # 格式化输出
        codec=>rubydebug
    }
}
```

#### Filter plugin: Grok

Grok is currently the best way in logstash to parse crappy unstructured log data into something structured and queryable.

Grok是提供了很多[基础模板][1]，大部分的情况都可以使用基础的模板来匹配。

``` Json
# text: www.google.com
# pattern: %{HOSTNAME:host}
output:
{
  "host": [
    [
      "www.google.com"
    ]
  ]
}
```

当然也可以自定义模板，具体的方式如下：
```
# (?<field_name>the pattern here) 
# text: text/html
# pattern: (?<mime>%{WORD:mime_type}/%{WORD:mime_subtype}|-)
output:
{
  "mime": [
    [
      "text/html"
    ]
  ],
  "mime_type": [
    [
      "text"
    ]
  ],
  "mime_subtype": [
    [
      "html"
    ]
  ]
}
```

可以通过[Grokdebug网站][2]验证新建的Grok模板是否正常工作。

下面的例子展示了如何通过匹配Squild3默认的Access日志：

``` Json
# example
# 1463987745.513  55287 127.0.0.1 TCP_MISS/200 147235 CONNECT ssl.gstatic.com:443 - HIER_DIRECT/216.58.221.99 -
# 1463884248.230   2251 127.0.0.1 TCP_MISS/404 491 GET http://tp.client.xunlei.com/update/xml/1.1.2.259_0.xml - HIER_DIRECT/119.188.94.188 text/html

# Grok Pattern
# %{NUMBER:timestamp}\s+%{NUMBER:duration} %{IPV4:client} (%{WORD:squid_request_status}/%{NUMBER:http_status}) %{NUMBER:reply_size} %{WORD:method} (?<request_url>%{URI}|%{URIHOST}) - (%{WORD:squid_hierarchy_status}/%{IPV4:server_ip}) (?<mime>%{WORD:mime_type}/%{WORD:mime_subtype}|-)

# output
{
                   "message" => "1463987745.513  55287 127.0.0.1 TCP_MISS/200 147235 CONNECT ssl.gstatic.com:443 - HIER_DIRECT/216.58.221.99 -",
                  "@version" => "1",
                "@timestamp" => "2016-05-24T12:35:32.870Z",
                      "host" => "ubuntu",
                 "timestamp" => "1463987745.513",
                  "duration" => "55287",
                    "client" => "127.0.0.1",
      "squid_request_status" => "TCP_MISS",
               "http_status" => "200",
                "reply_size" => "147235",
                    "method" => "CONNECT",
               "request_url" => "ssl.gstatic.com:443",
                      "port" => "443",
    "squid_hierarchy_status" => "HIER_DIRECT",
                 "server_ip" => "216.58.221.99",
                      "mime" => "-"
}

{
                   "message" => "1463884248.230   2251 127.0.0.1 TCP_MISS/404 491 GET http://tp.client.xunlei.com/update/xml/1.1.2.259_0.xml - HIER_DIRECT/119.188.94.188 text/html",
                  "@version" => "1",
                "@timestamp" => "2016-05-24T12:35:42.517Z",
                      "host" => "ubuntu",
                 "timestamp" => "1463884248.230",
                  "duration" => "2251",
                    "client" => "127.0.0.1",
      "squid_request_status" => "TCP_MISS",
               "http_status" => "404",
                "reply_size" => "491",
                    "method" => "GET",
               "request_url" => "http://tp.client.xunlei.com/update/xml/1.1.2.259_0.xml",
    "squid_hierarchy_status" => "HIER_DIRECT",
                 "server_ip" => "119.188.94.188",
                      "mime" => "text/html",
                 "mime_type" => "text",
              "mime_subtype" => "html"
}
```

> 参考资料

> - https://www.elastic.co/guide/en/logstash/current/plugins-filters-grok.html#_custom_patterns
> - http://www.squid-cache.org/Doc/config/logformat/

[1]:https://github.com/logstash-plugins/logstash-patterns-core/blob/master/patterns/grok-patterns
[2]:http://grokdebug.herokuapp.com/

> Written with [StackEdit](https://stackedit.io/).