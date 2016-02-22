---
layout: post
title:  "Python笔记: 字符串Join/SHA1加密/三元表达式"
categories: python
tags:   python weixin
---
微信在验证消息真实性时, 加密/校验流程如下：

1. 将token、timestamp、nonce三个参数进行字典序排序
2. 将三个参数字符串拼接成一个字符串进行sha1加密
3. 开发者获得加密后的字符串可与signature对比，标识该请求来源于微信

使用Flask作为Web服务器,所以需要用Python实现以上的功能.下面是实现过程中遇到的一些问题:

#### 排序拼接字符串
通过使用Str类型的join方法,可以将排序后的字符串列表,拼接为一个字符串.

> join(...)
>
> S.join(iterable) -> string
>
> Return a string which is the concatenation of the strings in the iterable.  The separator between elements is S.

```python
lst = ['token value', 'timestamp value', 'nonce value']
lst.sort()
// ''表示不需要分隔符
tmp_str = ''.join(lst)
```

#### SHA1加密
使用Python内置模块hashlib

> openssl_sha1(...)
>
> Returns a sha1 hash object; optionally initialized with a string

```python
import hashlib
data = 'SHA1 encryption'
sha1 = hashlib.sha1(data)
sha1 = sha1.hexdigest()
'd8e6ade66fb7741271995f29a4f9e93090b2a00b'
```

#### 三元表达式
按照C的思维写了三元表达式之后,运行报错了:
X ? V1 : V2

才想起来Python的写法是不一样的:
V1 if X else V2

```python
'Success' if True else 'Failure'
'Success'
'Success' if False else 'Failure'
'Failure'
```


