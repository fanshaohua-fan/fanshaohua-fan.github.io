---
layout: post
title:  "Python笔记: HTML Scraping"
categories: python
tags:   python xlml html
---
###提取HTML 内容

很多公司都提供网页查询的功能，但是不会对公众开发API，所以很多时候，都只能通过解析html来得到我们想要的数据。大部分情况下，只有部分的HTML是有用的。
这就需要将我们需要的部分，提取出来。比较地道的英文是**HTML Scraping**，直译过来是刮削，就是这个玩意：
![Scraper](http://www.greenbookblog.org/wp-content/uploads/2010/11/scraping.jpg)
所以很多XBMC插件的中文是某某刮削器。。。

HTML提取比较流行的工具有：

 - lxml.html
 - beautifulsoup
 - re(Regular Expression)

lxml.html的样例有使用XPath，所以决定先通过lxml.html试试，能否到达想要的效果，也顺便复习一下XPatch。

####使用lxml的html 包来提取HTML中的内容

直接上代码：

```python
from lxml import html
tree = html.parse('ems.html').getroot()
delivery = tree.xpath('//div[@id="s_div"]/table/tr/td/text()')

for i, data in enumerate(delivery):
    deal[i] = data.strip()

for data in delivery:
    print data

2015-11-10
06:40:00
收寄
荷兰

2015-11-11
08:03:00
离开处理中心,发往中国 北京
荷兰

```
