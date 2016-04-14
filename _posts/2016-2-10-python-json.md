---
layout: post
title:  "Python JSON encoder and decoder"
categories: python
tags:   python json
---

#### 解析Json数据

```python
>>> import json

>>> content='{"status":200,"msg":"OK","result":{"ticket":"-Ju3np4cSoM~2d75c267eb79f281~1455121096~n~~1~McmMbYV2A84tU471","captcha_url":false,"captcha_w":false,"captcha_h":false,"wait_time":0,"valid_until":"2016-02-10 16:33:17"}}'

>>> content_js = json.loads(content)
>>> content_js.get('status')
>>> ticket = content_js.get('result').get('ticket')

# get_dl_link
>>> url = 'https://api.openload.co/1/file/dl?file=%s&ticket=%s' % (file, ticket)

```

#### 构造Json数据

```python
>>> response = {'status': self.status,
                    'msg': self.msg,
                    'cookie': self.cookie,
                    'result': self.result }

>>> print json.dumps(response, sort_keys=True)
{"cookie": "JSESSIONID=PVzdXNMMpBQQTGvbM1Tcf8YBfDdsjxLLM7VQ4T1WQhFcQzv04WFC!263288888; path=/; HttpOnly, BIGipServerweb_pool=1025311498.16671.0000; path=/", "msg": "OK", "result": "011977", "status": 200}
```

> Written with [StackEdit](https://stackedit.io/).
