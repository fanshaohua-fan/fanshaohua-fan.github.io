---
layout: post
title:  "Python OCR 验证码识别"
categories: python
tags:   python ocr pytesseract
---

#Python OCR 验证码识别（pytesseract）

####安装

 1. install Python Imaging Library
 2. Install google tesseract-ocr
 3. Install pytesseract with pip


```bash
sudo apt-get install python-imaging
sudo apt-get install tesseract-ocr
sudo pip install pytesseract
```

####最小应用

```python
from PIL import Image
import pytesseract

ocr = Image.open('./Pictures/rand.jpg')
pytesseract.image_to_string(ocr)
'240323'
```

####将识别校验码的内容封装为ocr.py

```python
import pytesseract
import httplib2
from PIL import Image
from StringIO import StringIO


def process_image(url):
    image = _get_image(url)
    return pytesseract.image_to_string(image)

def _get_image(url):
    httplib2.debuglevel = 1
    h = httplib2.Http('.cache')
    response, content = h.request(url)
    return Image.open(StringIO(content))

```

####调用ocr的process_image方法
以EMS的校验码为例：

```python
import ocr
url = 'http://www.ems.com.cn/ems/rand'
ocr.process_image(url)

DEBUG log:
connect: (www.ems.com.cn, 80) ************
send: 'GET /ems/rand HTTP/1.1\r\nHost: www.ems.com.cn\r\naccept-encoding: gzip, deflate\r\nuser-agent: Python-httplib2/0.8 (gzip)\r\n\r\n'
reply: 'HTTP/1.1 200 OK\r\n'
header: Cache-Control: no-cache
header: Connection: close
header: Date: Wed, 11 Nov 2015 17:19:47 GMT
header: Content-Type: image/jpeg
header: Set-Cookie: JSESSIONID=Yfr9WD4TJbvpyPhqPdkndlJng6bH5P7LJKCqfygjZn4PvF9Kdlk8!-968903311; path=/; HttpOnly
header: X-Powered-By: Servlet/2.5 JSP/2.1
header: Set-Cookie: BIGipServerweb_pool=67830538.37151.0000; path=/
'443929'

```
