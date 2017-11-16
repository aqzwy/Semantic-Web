### 作为一个初学小菜鸡，一般会通过Blog和一些官方文档来学习Neo4j的相关知识点。本文主要通过使用py2neo来操纵neo4j数据库，借鉴学习他人的经验后，进行知识汇总并加入自己的理解，将相关内容记录下来。py2neo 官方文档：[http://py2neo.org/v3/types.html](http://py2neo.org/v3/types.html)</font>

## 注意点

graph.create()决定着数据是否从内存中存入到Neo4j中，若没有使用该方法，前面的数据创建只会显示出对应的属性信息，但最终不会再Neo4j中
进行记录，所以当你去Neo4j数据库中查看时，结果为空。但不影响用户对于各种方法的使用和测试，其通过print()打印出节点关系和节点信息。

```Python
     a = Node('Person', name='Alice')
     b = Node('Person', name='Bob')
     r = Relationship(a, 'KNOWS', b)
     s = a | b | r #subgraph
     print(s）
```
结果：
```
     ({(alice:Person {name:"Alice"}), (bob:Person {name:"Bob"})}, {(alice)-[:KNOWS]->(bob)})
```

## 连接数据库
通过python连接Neo4j数据库时，使用地址bolt开头(bolt://127.0.0.1:7687）或地址http://127.0.0.1:7474

当连接报错的时候，请确认neo4j服务器处于打开状态，若打开后依旧无法连接，请重新安装后设置密码即可。建议使用第一个地址进行数据库连接，缘由是英文文档中的推荐
>  “Once obtained, the Graph instance provides direct or indirect access 
to most of the functionality available within py2neo. If Bolt is 
available (Neo4j 3.0 and above) and Bolt auto-detection is enabled, this will       be used for Cypher queries instead of HTTP.　”

```Python
     # 连接 Neo4j 3.2.0
      Neo4j_graph = Graph("bolt://127.0.0.1:7687", username="neo4j", password= yourPassword)
```

## 建立节点及关系

通过Node方法创建一个简单的节点，该节点包含节点类型font>, 该节点类型相当于本体中的 描述具有同种特征的实体集合，name就是节点的名称，在Neo4j中每个节点具有唯一的ID作为唯一识别信息。 两个节点之间的关系可以通过Relationship来进行设定，既设定点与点之间的边的内容，最终通过create()方法在Neo4j数据库中进行节点及关系的创建。

```Python
    # 建立节点信息
     node_1 = Node("Person",name='Node_1')
     node_2 = Node("Person",name='Node_2')
     Neo4j_graph.create(node_1)
     Neo4j_graph.create(node_2)

    # 建立关系信息
     relation = Relationship(node_1,'KNOWS',node_2)
     Neo4j_graph.create(relation)
```	
创建关系的第二种方法,对于创建的边（关系）通过type()方法进行查看
```Python
   # 建立关系 第二种方法
     class WorksWith(Relationship):pass
     relation_1 = WorksWith(node_1,node_2)
     Neo4j_graph.create(relation_1)
     print(relation_1.type(),relation.type())
```

## 建立节点及关系的其它方法

通过字典的方式[]、设定默认值setdefault可以对节点或者边（属性）进行相应属性信息的添加。其中字典添加信息的方式会覆盖住默认值。或者通过update更新属性信息
```Python
     node_1 = Node("Person",name='Node_1')
     node_2 = Node("Person",name='Node_2')
     
    # First
     node_1['age']=20
     node_1['otherName']='Bush'

     # Second
     node_2.setdefault('location','beijing')

     # Third
     data = {
    'name': 'Amy',
    'age': 21
     }
     node_1.update(data)

     Neo4j_graph.create(node_1)
     Neo4j_graph.create(node_2)

     # 边添加属性信息
     relation = Relationship(node_1,'KNOWS',node_2)
     relation['time'] = '2017/08/31'
     Neo4j_graph.create(relation)
```
或者使用命为subgraph的方法进行关系的创建，最后记得使用create() 方法进行数据的序列化操作
```Python
     a = Node('Person', name='Alice')
     b = Node('Person', name='Bob')
     r = Relationship(a, 'KNOWS', b)
     #subgraph
     s = a | b | r
     print(s)
     Neo4j_graph.create(s)
```
以上对于节点和关系的创建总共分为两步：1. 选择一种方法建立好节点和关系信息 2. 选择一种建立节点之间关系的方法

## 查询语句

下面的语句表达的含义是查询Neo4j库中所有节点的信息并返回。
```Python
     #Cypher in Python
     result=test_graph.data('MATCH (n) RETURN n LIMIT 25')
     print(result)
```


