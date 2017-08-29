## 基础知识

#### Graph Fundamentals

1. Nodes: graph data records
2. Relationships: connect nodes
3. Properties: named data values
4. Labels: Nodes can be grouped together by applying a Label to each member

# Cypher 

***

## 创建结点 CREATE

```
CREATE (
   <node-name>:<label-name>
   { 	
      <Property1-name>:<Property1-Value>
      ........
      <Propertyn-name>:<Propertyn-Value>
   }
)
```

例子

```
CREATE (ee:Person { name: "Emil", from: "Sweden", klout: 99 });
```

* ``CREATE``: clause to create data
* ``()``: parenthesis to indicate a node
* ``ee:Person``: a variable 'ee' and label 'Person' for the new node
* ``{}``: brackets to add properties to the node

## 匹配结点 MATCH

```
MATCH (<node-name>:<label-name>) 
RETURN 
   <node-name>.<property1-name>,
   ........
   <node-name>.<propertyn-name>
```

例子
```
MATCH (ee:Person) WHERE ee.name = "Emil" RETURN ee;
```

* ``MATCH``: clause to specify a pattern of nodes and relationships
* ``(ee:Person)``: a single node pattern with label 'Person' which will assign matches to variable 'ee' 
* ``WHERE``: clause to constrain the results
* ``ee.name = "Emil"``: compares name property to the value "Emil"
* ``RETURN``: clause used to request particular results

## 关系

#### 没有属性的关系和现有结点

```
MATCH (<node1-label-name>:<node1-name>),(<node2-label-name>:<node2-name>)
CREATE  
	(<node1-label-name>)-[<relationship-label-name>:<relationship-name>]->(<node2-label-name>)
RETURN <relationship-label-name>
```

例子
```
MATCH (e:Customer),(cc:CreditCard) 
CREATE (e)-[r:DO_SHOPPING_WITH ]->(cc) 
```

#### 有属性的关系和现有结点
```
MATCH (<node1-label-name>:<node1-name>),(<node2-label-name>:<node2-name>)
CREATE  
	(<node1-label-name>)-[<relationship-label-name>:<relationship-name>{<define-properties-list>}]->(<node2-label-name>)
RETURN <relationship-label-name>
```

例子
```
MATCH (cust:Customer),(cc:CreditCard) 
CREATE (cust)-[r:DO_SHOPPING_WITH{shopdate:"12/12/2014",price:55000}]->(cc) 
RETURN r
```

#### 新结点无属性关系

```
CREATE  
   (<node1-label-name>:<node1-name>)-
   [<relationship-label-name>:<relationship-name>]->
   (<node1-label-name>:<node1-name>)
RETURN <relationship-label-name>
```

例子
```
CREATE (fb1:FaceBookProfile1)-[like:LIKES]->(fb2:FaceBookProfile2) 
```

#### 新节点有属性关系

```
CREATE  
	(<node1-label-name>:<node1-name>{<define-properties-list>})-
	[<relationship-label-name>:<relationship-name>{<define-properties-list>}]
	->(<node1-label-name>:<node1-name>{<define-properties-list>})
RETURN <relationship-label-name>
```

例子

```
CREATE (video1:YoutubeVideo1{title:"Action Movie1",updated_by:"Abc",uploaded_date:"10/10/2010"})
-[movie:ACTION_MOVIES{rating:1}]->
(video2:YoutubeVideo2{title:"Action Movie2",updated_by:"Xyz",uploaded_date:"12/12/2012"})
```

#### 检索

```
MATCH (<node1-label-name>)-[<relationship-label-name>:<relationship-name>]->(<node2-label-name>)
RETURN <relationship-label-name>
```

## 创建标签

#### 单个标签到结点

```
CREATE (<node-name>:<label-name>)
```

例子
```
CREATE (google1:GooglePlusProfile)
```

#### 多个标签到结点

```
CREATE (<node-name>:<label-name1>:<label-name2>.....:<label-namen>)
```

例子

```
CREATE (m:Movie:Cinema:Film:Picture)
```

#### 单个标签到关系

```
CREATE (<node1-name>:<label1-name>)-
	[(<relationship-name>:<relationship-label-name>)]
	->(<node2-name>:<label2-name>)
```

例子
```
CREATE (p1:Profile1)-[r1:LIKES]->(p2:Profile2)
```

## WHERE子句

```
WHERE <condition>

WHERE <condition> <boolean-operator> <condition>
```

condition语句
```
<property-name> <comparison-operator> <value>
```

布尔运算符

|布尔运算符|描述|
|:-------:|:--:|
|AND||
|OR||
|NOT||
|XOR||

比较运算符
|比较运算符|描述|
|:-------:|:--:|
|=|等于|
|<>|不等于|
|<|小于|
|>|大于|
|<=|小于等于|
|>=|大于等于|

例子
```
MATCH (emp:Employee) 
WHERE emp.name = 'Abc' OR emp.name = 'Xyz'
RETURN emp
```

## 删除 DELETE

**DELETE可以用来删除结点和关系**

```
DELETE <node-name-list>
DELETE <node1-name>,<node2-name>,<relationship-name>
```

例子

```
MATCH (e: Employee) DELETE e
MATCH (cc: CreditCard)-[rel]-(c:Customer) DELETE cc,c,rel
```

## 删除 REMOVE

**REMOVE可以用来删除结点或关系的属性**

```
REMOVE <property-name-list>

# property-name-list语法
<node-name>.<property1-name>,
<node-name>.<property2-name>, 
.... 
<node-name>.<propertyn-name> 

======================================

REMOVE <label-name-list> 

# label-name-list语法
<node-name>:<label2-name>, 
.... 
<node-name>:<labeln-name> 
```


例子
```
MATCH (dc:DebitCard) 
REMOVE dc.cvv
RETURN dc

MATCH (m:Movie) 
REMOVE m:Picture
```

## 设置 SET

**SET和REMOVE作用相反，向现有结点或者关系添加或者更新属性值**

语法和REMOVE相同

## 排序

```
ORDER BY  <property-name-list>  [DESC]	 
```

例子

```
MATCH (emp:Employee)
RETURN emp.empid,emp.name,emp.salary,emp.deptno
ORDER BY emp.name
```