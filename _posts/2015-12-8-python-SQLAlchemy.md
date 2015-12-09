---
layout: post
title:  "Python笔记: SQLAlchemy"
categories: python
tags:   python database 
---

###Connecting

```python
>>> import sqlalchemy
>>> sqlalchemy.__version__
'1.0.9'
>>> from sqlalchemy import create_engine
>>> engine = create_engine("mysql://root:123456@127.0.0.1:32768/db?charset=utf8", echo=True)
>>> engine
Engine(mysql://root:***@localhost:32768/db)
```

###Define and Create Tables

```python
>>> from sqlalchemy import Table, Column, Integer, String, MetaData, ForeignKey
>>> metadata = MetaData()
>>> messages = Table('messages', metadata,
...     Column('id', Integer, primary_key=True),
...     Column('FromUser', String(32)),
...     Column('ToUser', String(32)),
...     Column('content', String(128)),
... )
>>> metadata.create_all(engine)
CREATE TABLE messages (
	id INTEGER NOT NULL AUTO_INCREMENT, 
	`from` VARCHAR(32), 
	`to` VARCHAR(32), 
	content VARCHAR(128), 
	PRIMARY KEY (id)
)

>>> messages.drop(engine)

```

###Insert Expressions

```python
>>> ins = messages.insert()
>>> str(ins)
'INSERT INTO messages (id, "from", "to", content) VALUES (:id, :from, :to, :content)'
>>> ins1 = messages.insert().values(FromUser='fan', ToUser='elain', content='hello')
>>> ins.compile().params 
{'content': 'hello', 'FromUser': 'fan', 'ToUser': 'elain'}
```

###Executing

```python
>>> conn = engine.connect()
>>> result = conn.execute(ins1)
2015-12-08 18:20:06,653 INFO sqlalchemy.engine.base.Engine INSERT INTO messages (`FromUser`, `ToUser`, content) VALUES (%s, %s, %s)
2015-12-08 18:20:06,654 INFO sqlalchemy.engine.base.Engine ('fan', 'elain', 'hello')
2015-12-08 18:20:06,692 INFO sqlalchemy.engine.base.Engine COMMIT

>>>conn.execute(ins, id=2, FromUser='wendy', ToUser='Wendy', content='Williams')
>>>conn.execute(ins, [
...     {'content': 'hehe', 'FromUser': 'yun', 'ToUser': 'ali'}
... 	])
```

###Table Reflecting

```python
>>> messages = Table('messages', meta, autoload=True, autoload_with=engine)
>>> customers = Table('customers', meta, autoload=True, autoload_with=engine)
```

###Selecting

```python
>>> s = select([messages])
>>> result = conn.execute(s)
2015-12-08 18:51:05,805 INFO sqlalchemy.engine.base.Engine SELECT messages.id, messages.`FromUser`, messages.`ToUser`, messages.content 
FROM messages
2015-12-08 18:51:05,805 INFO sqlalchemy.engine.base.Engine ()
>>> for row in result:
...     print row
... 
(1L, 'fan', 'elain', 'hello')
(2L, 'wendy', 'Wendy', 'Williams')
(4L, 'yun', 'ali', 'hehe')
```

多表联合查找

```python
>>> s = select([ messages, customers]).where(messages.c.FromUser == customers.c.name)

2015-12-09 15:28:21,900 INFO sqlalchemy.engine.base.Engine SELECT messages.id, messages.`FromUser`, messages.`ToUser`, messages.content, customers.id, customers.name, customers.country, customers.postcode, customers.city, customers.street, customers.mobile, customers.email 
FROM messages, customers 
WHERE messages.`FromUser` = customers.name
2015-12-09 15:28:21,900 INFO sqlalchemy.engine.base.Engine ()
```

> Written with [StackEdit](https://stackedit.io/).