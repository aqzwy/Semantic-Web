### 作为一个初学小菜鸡，一般会通过Blog和一些官方文档来学习Neo4j的相关知识点。本文主要通过使用py2neo来操纵neo4j数据库，借鉴他人的经验，学习并理解，将相关内容记录下来。py2neo 官方文档：[http://py2neo.org/v3/types.html](http://py2neo.org/v3/types.html)</font>

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
    test_graph.create(node_1)
    test_graph.create(node_2)

    # 建立关系信息
    relation = Relationship(node_1,'KNOWS',node_2)
    test_graph.create(relation)
```	
结果：
 ![image](https://github.com/ButBueatiful/dotvim/raw/master/screenshots/vim-screenshot.jpg)




