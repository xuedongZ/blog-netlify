---
title: Basic SQL instruct
date: 2020-05-17 23:55:33
tags:
  - backend
---


作为后端程序员少不了要和数据库打交道。于是对 SQL 是什么，以及 SQL 和 MySql 什么关系，刚接触的人坑定一脸蒙。对于业务代码日常操作，熟悉基本操作满足大部分工作场景（不像银行，专门有人写SQL语句，对语法要求极高，写的好坏的差距就是数十台服务器，更不提海量的资金帐目）。不管怎样，知其然并知其所以然，工作便得心应手，也便于后面进阶。

## 数据库基础

数据对用户是不可见的，所以使用者往往意识不到自己不到自己请求数据库。从用户指令到数据库数据查询中间有一些过程（前后端数据传递、后端数据库语法的包装，不是本文的重点），暂且忽略，直观效果就用乎请求了数据库，并拿到了数据。

- 写邮件时查找地址薄的用户名
- 登录网站使用自己的名字和密码
- 在自动取款机上使用ATM卡，也是利用数据库进行PIN码验证和余额检查

以上的场景都是在使用数据库。
为了更好熟悉，我们先从必要术语看起：

### 术语清单

#### 1.database

数据库，是保存有组织的数据的容器（通常是一个文件或一组文件）

vs 数据库软件DBMS（数据库管理系统软件）：数据库是指通过DBMS创建和操纵的容器。

我们访问数据库并不是像excel 这样显式的笨重文件，而且我们也不是直接访问，我们使用DBMS，他替换我们访问数据库，并决定访问的对象相对与其他数据的区别。

#### 2.table

表：不是把数据直接放在容器里就完事儿了，我们要存入容器的文件里。而且特定的数据存入特定的文件。

在数据库的世界里，这样的 文件就叫做表。一个表是一个结构化的文件可以存储具体类型的数据。

一个表可以存储现实世界中的一个用户的列表，一个产品的目录，或者 其他清单式的信息。

关键点在于：表中存放表中的数据是具有某种类型，或者一条清单。你永远不能把一堆用户列表和一堆订单列表存在一个数据库的同一表中。这用做会造成检索和取用的困难。相对的，你应该将这两类数据存成两个表，一种类型存一个表。

数据库中表都有一个名字，而且往往是独一无二的。

注：名字的独立性事实上包括数据库名字的独立性和表名的独立。当然，你可以在不同的数据库里使用相同的表名。

2.1 columns

栏目：表是由每一栏栏组成。具体一栏是一特定类型的信息。

最好的理解这个，是把数据库的表展示想象为栅格，就想电子表格程序所显现的那样。

在一个用户table里面，一个栏可以体现用户的编码，另一栏名字，另一栏地址，再一栏城市等等。每一中数据都存在他们各自的栏目里面。

**分解数据**

把数据正确的分解到多个栏目里是极其重要的。比如城市，省，和邮编应该是单独的栏目，这样拆分后，就能专门针对该数据进行排序或者过滤。如果讲城市和省放在一栏里，这样是极难根据省份排序或过滤的。

数据拆分的颗粒度取决于你和你的具体需求。

eg： 如果你没有街道的排序需求，完全可以把门派号码和街道号放在一栏里面。

2.2 datatype

数据库表的每一栏都有数据类型。存数字还是字符串，需具体指明。做限制后可防止在数字字段中存入字符值。

数据类型还帮助正确的排序数据，并在优化磁盘方面起重要作用。创建表单 的时候都要指定正确的数据类型。

2.3 datatype compatibility

数据类型的兼容性。再度不同的数据库管理系统下，基本的数据类型是通用的，许多高阶的数据类型却不是。没有什么好的法子，在创建table schemas 的时候谨记便是。

2.4 Rows

行。数据在表里面是以行的形式存储的。每条记录存储在自己的行里面。

比较： Records or Rows

虽然人们使用他来指代同一个东西。大部分情况下两个词都是可替换使用。但在单数情况下，row 严格来说才是正确的术语。

2.5 primary keys

主键。表的每一行都应该有一列作为自己的唯一标识。

eg：用户的表中，可能用电话号码栏作为自己的唯一标识，而订单的表可能用订单的id作为自己的唯一标识。一个书单的表，可能使用书的ISBN码作为唯一标识。

如果没有主键，更新或者删除表中的特定行和变得极其困难，因为没有安全的方式确定要处理的就是这一行。

注：永远要记得定义主键。尽管当时主键可能不需要，但便于将来的数据的操作。

表中每一栏都可被定义为主键，只要他满足一下情形：

- 没有哪两行的主键值是一样的

- 每一行的主键必须有值（不能是null值）
- 主键的值永远不能被调整或者更新
- 主键值的永不复用。（如果一行的数据被删除了，哪这条数据的主键值在将来都不能被再次是使用）

主键一般使用单一栏，如果使用多个栏，那每个栏都要符合上述原则。



#### 3.schema

模式：描述数据库和表的展示关系和具体属性的。

提前接入了用户的概念（后面解释），先字面理解。

仓库（datebase）、 房间（shcema）、床位（table）的比喻就很好。

user管理整个仓库，对不同的房间可能有不同的操作权限：不能进；只能进去查看；能进去查看还能增删房间里面的货品（table 或者table的内容）

具体参考：https://yq.aliyun.com/articles/133194



## 什么是SQL

SQL 发音就是单个字母拼读，是结构化查询语言的缩写（structured query language）。

SQL是专门用来于数据库通信的语言。和其他语言不同的是，sql语言由很少的词构成，这是有意而为的。他专门被设计来做好一件事情，提供一种高效的方式读写数据库的数据。

- sql 不是某个数据库供应商的专有语言。几乎所有的DBMS都支持SQL。
- sql 简单易学。由描述性很强的英语单词组成。
- 看上去简单，却是一种强有力的语言，灵活组合其语言元素，可进行非常复杂和高级的数据库操作。

不同的DBMS实现的SQL都不完全一样。但存在一个标准委员会，在定义可供所有DBMS使用的sql语法。

- ANSI SQL  （委员会制定的标准）
- PL-SQL （oracle 定义的标准）
- Tranact-SQL （微软定义的标准）



### 安装
那安装那个SQL呢，有几个选择：

1.mysql 

一个非常好的选择：

- 免费，
- 支持主要的操作系统，
- 便于安装，
- 最受欢迎之一，
- 提供命令行工具来进入数据库（最好还是安装 MySQL Workbench）

> 到这里，我们已经知道：原来Mysql是数据库软件（DBMS）。因为数据的所有存储、检索、管理和处理实际上是由DBMS完成。

2.Microsoft SQL 

- windows用户的选择，免费
- 有用户友好的用户端界面工具-- SQL Server Management Studio

当然，还有其他的选择，这里不一一列出。

## 操作数据

### 获取

1.取一栏或多栏数据

`select column_name from Table_name;`

命令结尾需要分号；（DBMS在单条语句之后，不一定要加分号；多条语句一定要加分号；使用mysql语句必须加上分号。总结：就分号肯定没坏处）

SQL 声明语句是不分大小写的。（SELECT、select、Select 等价）除非你用的DBMS规定了大小写敏感。

但如果所有SQL关键字都大写，column 和table名都小写，便于代码的阅读和排错；



2.取所有栏数据

因为取具体栏的数据一般还是用的不多。

获取所有栏（不单独挑选）更常见

语法： `SELECT  * from Products;``

- 使用了通配符，就表示取表单的所有栏。除非确实需要阅读每一栏，否则不要这么做。因为获取不必要的栏会减慢查询的速度。


3.限定数量搜索哦

`SELECT TOP 5 from prod_name FROM Products;`

4.排序

使用从句：ORDER BY

```mysql
SELECT prod_name 
FROM Products 
ORDER BY prod_name;
```

注：使用ORDER BY 从句，必须是select 声明的最后一个从句，否则会报错。

4.1 根据多栏排序
```sql
SELECCT prod_id, prod_price, prod_name
FROM Products
ORDER BY prod_price, prod_name;

```

 先根据价格排序，再根据名字排序。

4.2 根据clolumn position排序

也可以达到根据多栏名称排序的效果。

```sql
SELECCT prod_id, prod_price, prod_name
FROM Products
ORDER BY 2,3;
```

根据SELECT list中 第2栏和第3栏排序。好处：省去打名字；坏处：1）选错栏 2）换列表的时候忘记改从句。



### 过滤数据

1.使用WHERE 从句。

在SELECT 声明中，数据的过滤是通过在WHERE从句中明确具体搜索标准的。

```sql
SECECT prod_name, prod_price
FROM Products
WHERE prod_price = 3.49;
```

这个声明语句从products 表中取了2栏，但并没有返回表的所有数据，而是只返回了价格为3.49的数据。

1.1 使用其他具体operator

Operator | description

= | 等于

<> | 不等于

!= | 不等于

< | 小于

<= | 小于等于

!< | 不小于

BETWEEN |  在两个具体值之间

IS NULL|  为空值

### 高级的过滤

在WHERE从句里面使用 AND；
```sql
SELECT prod_id, prod_price, prod_name
FROM products
WHERE vend_id = 1003 AND prod_price <= 10;
```

在WHERE从句里面使用 OR；
```sql
SELECT prod_name, prod_price
FROM products
WHERE vend_id = 1003 or prod_price <= 10;
```

在WHERE从句里面使用 IN；
```sql
SELECT prod_name, prod_price
FROM products
WHERE vend_id IN (1002,1003);
ORDER BY prod_name;
```

在WHERE从句里面使用 NOT；NOT的作用，否定他之后跟的任何条件
```sql
SELECT prod_name, prod_price
FROM products
WHERE vend_id NOT IN (1002,1003);
ORDER BY prod_name;
```
复杂的子句，NOT非常有用。

### 添加（列）
ALTER TABLE 语句用于在已有的表中添加、删除或修改列。
```sql
ALTER TABLE inventory_debit_sheets
ADD COLUMN transporterId INT NOT NULL AFTER code;
```
### 更新
```sql
UPDATE enterprise_config set configJson = '',updatedAt = '2020-06-10 15:18:45' where uid =123;
```
### 创建
```sql
CREATE TABLE customers
(
  cust_id       int       NOT NULL AUTO_INCREMENT,
  cust_name     char(50)  NOT NULL,
  cust_address  char(50)  NOT NULL,
  cust_zip      char(50)  NOT NULL,
  PRIMSRY KEY (cust_id)
) ENGINE = InnoDB;
```

## 总结：

数据库的基本操作，涉及到查询部分均是安全操作，平时多加练习，也不担心破坏数据库。
