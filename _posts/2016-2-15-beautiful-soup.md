---
layout: post
title:  "Python笔记:  Beautiful Soup"
categories: python
tags:   python scraping html bs4
---

Beautiful Soup
-----

之前介绍过如果使用lxml来解析HTML/XML，好处是可以使用Xpath，对于熟悉Xpath的人来说，可能比较方便。

今天简单介绍下另外一个解析HTML（HTML Parsing）的工具Beautiful Soup。

####安装 
由于我使用的是Ubuntu 14.04，可以直接使用Apt-get来安装：

```bash
$ sudo apt-get install Python-bs4
```


####简单使用

```python
from bs4 import BeautifulSoup
# 使用默认解析器
soup = BeautifulSoup(open("mv.html"))
```

####搜索文档树

```python
>>> soup = BeautifulSoup(content)
# 查找文档中所有class为blog_title的div标签
>>> blog_title_list = soup.find_all("div", class_="blog_title")
>>> title = blog_title_list[0]
>>> title
'<div class="blog_title" id="t_10001_1517731560102wjw1">
<a href="http://blog.sina.com.cn/s/blog_1517731560102wjw1.html" target="_blank">【日剧】《澄和堇/澄和薰》[单剧连载中]</a>
</div>'
# 获取title标签下a标签的href的属性值
>>> title.a.get('href')
'http://blog.sina.com.cn/s/blog_1517731560102wjw1.html'
```

####指定文档解析器
如果是简单的使用场景，默认的解析器就可以解决问题，也就不必关心使用的是什么解析器。

目前bs支持的解析器和优先顺序如下：

 1. lxml
 2. html5lib
 3. html.parser

大部分的网页并不是完美的匹配HTML，在这时使用不同的解析器就有可能取到不同的结果。例如：

```python
# 显式使用lxml
>>> BeautifulSoup("<a></p>", "lxml")
# <html><body><a></a></body></html>

# 显式使用html5lib
>>> BeautifulSoup("<a></p>", "html5lib")
# <html><head></head><body><a><p></p></a></body></html>
```

不同解析器的比较如下：

解析器     | 使用方法 | 优势 | 劣势
--------   | ------- | -----| ----
Python标准库 | BeautifulSoup(markup, "html.parser") |Python的内置标准库/执行速度适中/文档容错能力强 | Python 2.7.3 or 3.2.2)前 的版本中文档容错能力差
lxml | BeautifulSoup(markup, "lxml") |  速度快 | 需要安装C语言库
html5lib | BeautifulSoup(markup, "html5lib") | 最好的容错性/以浏览器的方式解析文档/生成HTML5格式的文档 | 速度慢

在编写Kodi插件的时候就遇到了这个问题，使用Python标准库，不能够正确解析全部HTML，大部分HTML在转换成soup对象时丢掉了。

最后使用html5lib解析器完美的解决了这个问题，写代码时，最好注明使用了哪种解析器，以减少不必要的麻烦。


>参考资料：
 1. [Beautiful Soup 4.2.0 文档](http://www.crummy.com/software/BeautifulSoup/bs4/doc/index.zh.html)
