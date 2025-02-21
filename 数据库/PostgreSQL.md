# `PostgreSQL`

`PostgreSQL `是一个功能强大的开源关系型数据库管理系统（`RDBMS`），具有高度可扩展性和标准兼容性。它支持复杂查询、外键、触发器、视图、事务完整性、多版本并发控制（`MVCC`）等高级功能。`PostgreSQL `的设计目标是提供高度可靠性和数据完整性，适用于各种规模的应用程序

- `PostgreSQL` 更适合需要复杂查询、高度可扩展性和标准兼容性的应用场景，如数据分析、地理信息系统（`GIS`）和企业级应用
- `MySQL `则更适合需要高性能、简单易用和高并发读操作的 `Web `应用和中小型数据库



## 远程访问

我们有时需要对`PostgreSQL`数据库进行远程的访问操作，可以通过`python`或者软件`Navicat`进行访问

在访问前需要更改在`D:\postgresql\data`中的`pg_hba`配置文件，在`# IPv4 local connections:`下面加上一行，允许所有`ip`访问的指令：`host all all 0.0.0.0/0 trust`，如下图所示：

![image-20250221153342793](..\images\image-20250221153342793.png)

### 通过`python`远程访问

访问之前需要下载`psycopg2`包

```python
import psycopg2

# 连接postgresql数据库
conn = psycopg2.connect(dbname="postgres", user="postgres", password="j13579", host="10.234.75.59", port="5432")
print("Successfully connected!")
```

> `dbname`和`user`选择`pgAdmin4`自带的`"postgres"`数据库和用户；
>
> `password`是安装`PostgreSQL`时设置的密码；
>
> `host`为`ip`地址，可以通过`cmd`命令行`ipconfig`进行查看，`port`端口为默认的`5432`

***

### 通过`Navicat`远程访问

`Navicat`是一款功能强大的数据库管理工具，支持多种类型的数据库连接，包括`MySQL`、`PostgreSQL`、`Oracle`等。通过`Navicat`我们可以方便地操作整个数据库，并备份和还原整个数据库或单个表

在建立连接时要正确输入`postgresql`数据库的相关信息，确保可以连接成功

通过`Navicat`我们可以快速的查看和管理`postgresql`中存在的数据表单，方便进行远程管理



## 基本语法

### 数据库管理操作

- 连接到数据库，在命令行输入：`psql -U postgres`      后再输入密码

- 创建数据库：`create database mydb;`

- 修改数据库名称：`alter database mydb to mydb1;`

- 删除数据库：`drop database mydb;`

- 查看所有数据库：`\l`

- 切换到指定数据库：`\c mydb`

- 创建表：

  ```sql
  CREATE TABLE stu (
      id SERIAL PRIMARY KEY,
      name VARCHAR(100),
      email VARCHAR(100),
      age INTEGER
  );
  ```

- 修改字段名：`alter table stu rename id to myid;` 把`stu`表中字段`id`改为`myid`

- 删除字段：`alter table stu drop column name; ` 删除`stu`表中`name`的字段

- 添加字段：`alter table stu add column name varchar(20);`  添加`stu`表中`name`的字段

- 修改字段类型：`alter table stu alter column name type varchar(40);`

- 删除表：`drop table stu;`

- 查看当前数据库中的所有表：`\d`

- 查看表结构：`\d 表名`

- 插入数据：`insert into stu(name) values('xiaoming');`

- 插入整条完整的数据：`insert into stu values (1,'xiaogang','2022-10-10',NULL);`

- 指定字段插入数据：`insert into stu (id,name) values('2','xiaohong');`

- `SELECT`批量插入

  - 将表数据插入到新表中：`insert into test select * from stu; ` 把表`stu`的数据插入到`test`表中
  - 指定字段批量插入： `insert into test (id,name) select id, name from stu;`

- 更新/修改数据：`update stu set name='xiaohong' where id=1;`

- 删除数据：`delete from stu where id=1;`

- 删除`id`在1到3之间的表数据：`delete from stu where id between 1 and 3;`

- 清空数据表：`delete from stu;`      表还在，只是没有数据了

- 查询表：查询所有字段：`select * from stu;`

- 指定字段查询：`select id, name from stu;`

  - `In`关键字查询：查询`id`为1，3，5的成员：`select id, name from stu where id in (1,3,5);`

  - `between and `关键字查询，范围之间：

    `select id, name from stu where birthday between '2020-10-10' and '2024-10-10';`

  - 模糊查询：查询所有姓张的用户：`select id, name from test where name like '张%';`

***

### 数据类型

- 数值数据类型：常用`int`


- 字符串数据类型：常用`char(size`, ` varchar(size)`


- 日期/时间数据类型:常用`timestamp`：日期和时间：2023-09-25 10:45:30


​                                                           `data`：日期：2023-09-25

​                                                           `time`：时间：10:45:30

***

### 相关函数

#### 数值函数

|   函数    |       描述       |
| :-------: | :--------------: |
|  `avg()`  | 返回某列的平均值 |
| `count()` |  返回某列的行数  |
|  `max()`  | 返回某列的最大值 |
|  `min()`  | 返回某列的最小值 |
|  `sum()`  | 返回某列的值之和 |

调用方式：`select 函数（字段名）from 表名；`

#### 字符串函数

|      函数       |      描述      |
| :-------------: | :------------: |
|   `length(s)`   | 计算字符串长度 |
| `concat(s1,s2)` |   字符串合并   |

调用方式：`select 函数（字段名）from 表名；`

#### 日期时间函数

|          函数          |         描述         |
| :--------------------: | :------------------: |
|        `now()`         | 获取当前日期时间函数 |
|     `current_date`     |   获取当前日期函数   |
|    `current_ time`     |   获取当前时间函数   |
| `extract(type from d)` |  获取日期指定值函数  |

函数部分具体可以参考`Mysql`笔记中的函数部分

***

### `PostgreSQL`模式（`Schema`）

使用`Schema`可以使多个用户使用一个数据库而不互相干扰，我们登陆默认是在`public`这个模式下，是数据库对象的一个集合

`Schema`模式的好处：

- 允许多个用户使用一个数据库而不会干扰其它用户
- 把数据库对象组织成逻辑组，让它们更便于管理
- 第三方的应用可以放在不同的模式中，这样它们就不会和其它对象的名字冲突

表空间是实际的数据存储的地方，一个数据库`schema`可能存在于多个表空间，相似地，一个表空间也可以为多个`schema`服务。通过使用表空间，管理员可以控制磁盘的布局。表空间的最常用的作用是优化性能，例如，一个最常用的索引可以建立在非常快的硬盘上，而不太常用的表可以建立在便宜的硬盘上，比如用来存储用于进行归档文件的表

我们可以创建其他模式：`create schema my123;`

为所有模式创建一个表：`create table test(id int, name varchar(20));`

执行上述代码后，就多了一个模式`my123`，同时所有模式下都有表`test`



## `psycopg2`库的增删改查操作

预先通过`Terminal`安装好第三方库：`pip install psycopg2`

`Psycopg2`是一个用于`python`编程语言的第三方库，用于访问`PostgreSQL`数据库系统。它提供了一组工具和方法，可以轻松地在`Python`程序中进行数据库操作，包括查询、插入、更新、删除等操作

使用时需要进行包的导入：`import psycopg2`

### 增操作

```python
# 创建cursor来访问数据库，psycopg2提供了一个cursor类，用来在数据库Session里执行PostgresSQL命令
cur = conn.cursor()

# 通过python为postgresql数据库创建表
# 创建表，本次创建的表是student1，用于存放学生的name，age，class三种信息
cur.execute('''create table student1(id serial primary key, name varchar(10),age int,class int);''')
print("table created successfully!")   # 成功创建表，提示表创建成功

# 插入数据，通过数据库插入数据语句在表中输入相关信息
cur.execute("insert into student1(name,age,class) values('xiaoming',20,3)")
cur.execute("insert into student1(name,age,class) values('xiaohong',21,1)")
cur.execute("insert into student1(name,age,class) values('xiaogang',19,2)")
print("table inserted successfully!")  # 成功输入数据，提示表中数据输入成功

# 查询并打印数据
cur.execute("select * from student1")
rows = cur.fetchall()
print("----------------------------------------------------")
for row in rows:
   print("id=" + str(row[0]) + " name=" + str(row[1]) + " age=" + str(row[2]) + " class=" + str(row[3]))
print("----------------------------------------------------\n")

# 提交事务
conn.commit()

# 关闭连接
conn.close()
```

通过上述代码，完成了`python`通过库`psycopg2`对数据库进行增操作，在与`PostgreSQL`数据库连接后，`python`中表信息的创建可以同步到数据库中

***

### 删操作

```python
# 删除表student1某行的数据，删除部分的python代码
cur.execute("delete from student1 where id=1")
cur.execute("select * from student1")
rows = cur.fetchall()
print("----------------------------------------------------")
for row in rows:
    print('id=' + str(row[0]) + ' name=' + str(row[1]) +' age=' + str(row[2]) + ' class=' + str(row[3]))
print("----------------------------------------------------\n")
```

通过上述代码，完成了`python`通过库`psycopg2`对数据库进行删操作，在与`PostgreSQL`数据库连接后，`python`中表信息的相关删除操作可以同步到数据库中

通过库`psycopg2`对数据库进行删操作还包括删除整个表，其删除表的代码如下：

`cur.execute("drop table student")`

***

### 改操作

```python
# 更新数据，将id为2数据中的age改为23，class改为3，代码如下：
cur.execute("update student1 set age=23,class=3 where id=2")
cur.execute("select * from student1")
rows = cur.fetchall()
print("----------------------------------------------------")
for row in rows:
    print('id=' + str(row[0]) + ' name=' + str(row[1]) + ' age=' + str(row[2]) + ' class=' + str(row[3]))
print("----------------------------------------------------\n")
```

通过上述代码，完成了`python`通过库`psycopg2`对数据库进行改操作，在与`PostgreSQL`数据库连接后，`python`中表信息的相关更改操作可以同步到数据库中

***

### 查操作

```python
# 数据库中的表信息显示在python中，实现一个查找操作，代码如下：
cur.execute("select * from student1")
rows = cur.fetchall()
print("----------------------------------------------------")
for row in rows:
    print('id=' + str(row[0]) + ' name=' + str(row[1]) +' age=' + str(row[2]) + ' class=' + str(row[3]))
print("----------------------------------------------------\n")
```

通过上述代码，完成了`python`通过库`psycopg2`对数据库进行查操作，在与`PostgreSQL`数据库连接后，数据库中的信息可以同步到`python`中
