
参见[官方文档](http://py2neo.org/v3/types.html)

***

## 安装

```
pip install py2neo
```

## 调用

```
from py2neo import Node, Relationship
```
# 数据类型 Graph Data Types

## 创建结点

```
class Node(*labels, **properties)
```

例子
```
a = Node("Person", name="Alice")
```


## 创建关系

```
class Relationship(start_node, type, end_node, **properties)
```

例子
```
ab = Relationship(a, "KNOWS", b)
```

## 子图

子图可以通过集合运算符号形成

```
subgraph | other | ...
subgraph & other & ...
subgraph - other - ...
subgraph ^ other ^ ...
```

例子

```
s = ab | ac
```

## Walkable Types

A Walkable is a Subgraph with added traversal information.

```
>>> w = ab + Relationship(b, "LIKES", c) + ac
>>> w
(alice)-[:KNOWS]->(bob)-[:LIKES]->(carol)<-[:WORKS_WITH]-(alice)
```

# 图数据库 Graph Databases

## 定义

```
class py2neo.database.Graph(*uris, **settings)
```

例子
```
from py2neo import Graph
graph_1 = Graph()
graph_2 = Graph(host="localhost")
graph_3 = Graph("http://localhost:7474/db/data")
```

settings:

|Keyword|Description|Types|Default|
|:-----:|:---------:|:---:|:-----:|
|bolt|Use Bolt protocol|bool,None|None|
|secure|Use a secure connection(Bolt/TLS + HTTPS)|bool|False|
|host|Database server host name|str|'localhost'|
|http_port||int|7474|
|https_port||int|7473|
|bolt_port||int|7687|
|user||str|'neo4j'|
|password||str|no default|

## 使用

* begin(autocommit=False)
  Begin a new Transaction

* create(subgraph)
  Run a ``Transaction.create()`` operation within a autocommit Transaction

* data(statement, parameters=None, **kwparameters)
  Run a ``Transaction.run()`` operation within an autocommit Transaction

  ```
  >>> from py2neo import Graph
  >>> graph = Graph(password="excalibur")
  >>> graph.data("MATCH (a:Person) RETURN a.name, a.born LIMIT 4")
  [{'a.born': 1964, 'a.name': 'Keanu Reeves'},
   {'a.born': 1967, 'a.name': 'Carrie-Anne Moss'},
   {'a.born': 1961, 'a.name': 'Laurence Fishburne'},
   {'a.born': 1960, 'a.name': 'Hugo Weaving'}]
  ```

* delete(subgraph)

* delete_all()

* find(*args, **kwargs)
  
  Parameters: * label
              * property_key
              * property_value
              * limit

* find_one(*args, **kwargs)
  
  Parameters: * label
              * property_key
              * property_value

* match(start_node=None, rel_type=None, end_node=None, bidirectional=False, limit=None)
  
  Match and return all relations with specific criteria

  Parameters: * start_node
              * rel_type
              * end_node
              * bidirectional
              * limit

  例子
  ```
  for rel in graph.match(start_node=alice, rel_type="FRIEND"):
    print(rel.end_node()["name"])
  ```

## Transactions 这我不好翻译，事务？

上面Graph的很多函数实际上都是自动commit的transaction函数

```
class py2neo.database.Transaction(autocommit=False)
```

例子:
```
>>> from py2neo import Graph, Node, Relationship
>>> g = Graph()
>>> tx = g.begin()
>>> a = Node("Person", name="Alice")
>>> tx.create(a)
>>> b = Node("Person", name="Bob")
>>> ab = Relationship(a, "KNOWS", b)
>>> tx.create(ab)
>>> tx.commit()
>>> g.exists(ab)
True
```
