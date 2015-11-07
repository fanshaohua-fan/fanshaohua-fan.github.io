---
layout: post
title:  "Python OCR 验证码识别"
categories: python
tags:   python ocr pytesseract
---

#Python OCR 验证码识别（pytesseract）

安装：
 1. Install google tesseract-ocr:
 2. Install pytesseract with pip


```bash
sudo apt-get install tesseract-ocr
sudo pip install pytesseract
```

最小程序：

```python
from PIL import Image
import pytesseract

ocr = Image.open('./Pictures/rand.jpg')
pytesseract.image_to_string(ocr)
'240323'
```