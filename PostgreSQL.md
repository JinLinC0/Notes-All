# PostgreSQL

## PostgreSQL数据库的下载

下载地址：[https://www.postgresql.org/](https://www.postgresql.org/)

![image-20231012153943811](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231012153943811.png)

安装完成后在出现以下程序，常用的有pgAdmin4和SQL Shell(psql)

![image-20231012154031366](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231012154031366.png)

pgAdmin4界面如下所示：

![image-20231012154144039](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231012154144039.png)

SQL Shell界面如下所示：

![image-20231012154246204](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231012154246204.png)



## 远程访问PostgreSQL数据库

我们有时需要对PostgreSQL数据库进行远程的访问操作，可以通过python或者软件Navicat进行访问。

在访问前需要更改在D:\postgresql\data中的pg_hba配置文件，在# IPv4 local connections:中加上一行允许所有ip访问的指令，如下图所示：

![image-20231012154328453](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231012154328453.png)



### 通过python远程访问

```py
import psycopg2

#连接postgresql数据库
conn = psycopg2.connect(dbname="postgres", user="postgres", password="j13579", host="10.234.75.59", port="5432")
print("Successfully connected!")
#dbname和user选择pgAdmin4自带的"postgres"数据库和用户；password是安装PostgreSQL时确定的密码；host为ip地址，可以通过cmd命令行ipconfig进行查看，port端口为默认的5432
```

![image-20231011210223841](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231011210223841.png)

dbname和user选择pgAdmin4自带的"postgres"数据库和用户；password是安装PostgreSQL时确定的密码；host为ip地址，可以通过cmd命令行ipconfig进行查看，port端口为默认的5432

`print("Successfully connected!") `   成功连接数据库，提示数据库连接成功

访问成功如下：

![image-20231011204557723](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231011204557723.png)



### 通过Navicat远程访问

Navicat是一款功能强大的数据库管理工具，支持多种类型的数据库连接，包括MySQL、PostgreSQL、Oracle等。通过Navicat我们可以方便地操作整个数据库，并备份和还原整个数据库或单个表。

![image-20231012154419770](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231012154419770.png)

![image-20231012154443530](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231012154443530.png)

在建立连接时要正确输入postgresql数据库的相关信息，确保可以连接成功。

通过Navicat我们可以快速的查看和管理postgresql中存在的数据表单，方便进行远程管理。下图是在Navicat中对postgresql中的表student1进行查看。

![image-20231012154519091](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231012154519091.png)



## PostgreSQL的基本使用及语法

### 常用语法

创建数据库：`create database mydb；`

![image-20231011204818910](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231011204818910.png)![image-20231011204826191](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231011204826191.png)

修改数据库名称：`alter database mydb to mydb1;`

删除数据库：`drop database mydb;`

查看所有数据库：`\l`

切换到当前数据库：`\c mydb`

查看当前数据库中的所有表：`\d`

查看表结构：`\d 表名`



PostgreSQL常用的数据类型：

数值数据类型：常用int

字符串数据类型：常用char(size),  varchar(size)

日期/时间数据类型:常用timestamp：日期和时间：2023-09-25 10:45:30

​                                                    data：日期：2023-09-25

​                                                    time：时间：10:45:30

创建表：`create table test(id serial primary key, name varchar(20));`

![image-20231012154723951](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231012154723951.png)

修改表名：`alter table test rename to test1;`   把表名从test改为test1

修改字段名：`alter table test rename id to myid;` 把表中字段id改为myid

删除字段：`alter table test drop column name; ` 删除表中name的字段

添加字段：`alter table test add column name varchar(20);`  添加表中name的字段

修改字段类型：`alter table test alter column name type varchar(40);`

删除表：`drop table test;`

插入数据：`insert into test(name) values('xiaoming');`

![image-20231012154910118](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231012154910118.png)

插入整条完整的数据：`insert into test values (1,'xiaogang','2022-10-10',NULL);`

指定字段插入数据：`insert into test (id,name)  values('2','xiaohong');`

SELECT批量插入

将表数据插入到新表中：`insert into test1 select * from test; ` 把表test的数据插入到test1中

指定字段批量插入： `insert into test1 (id,name) select id, name from test;`



更新/修改数据：`update test set name='xiaohong' where id=1;`

![image-20231012154930943](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231012154930943.png)

删除数据：`delete from test where id=1;`

![image-20231012154957623](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231012154957623.png)

删除id在1到3的表数据：`delete from test where id between 1 and 3;`

清空数据表：`delete from test;`



查询表：查询所有字段：`select * from test;`

批量字段查询：`select id, name from test;`

In关键字查询:查询id为1，3，5的成员：`select id, name from test where id in (1,3,5);`

between and 关键字查询，在什么之间：

`select id, name, birthday from test where birthday between '2020-10-10' and '2024-10-10';`

模糊查询,查询所有姓张的用户：`select id, name from test where name like '张%';`



### 相关函数

函数

数值函数：

​     avg() 返回某列的平均值

​     count() 返回某列的行数

​     max()  返回某列的最大值

​     min()  返回某列的最小值

​     sum()  返回某列的值之和

调用方式：`select 函数（字段名）from 表名；`

字符串函数：length(s)  计算字符串长度

​      `concat(s1,s2)`  字符串合并

调用方式：`select 函数（字段名）from 表名；`

日期时间函数：

​       `now()`  获取当前日期时间函数

​       `current_date`  获取当前日期函数

​       `current_ time`  获取当前时间函数  

​       `extract(type from d)`  获取日期指定值函数



### PostgreSQL模式（Schema）

使用Schema可以使多个用户使用一个数据库而不互相干扰。

![image-20231012155026090](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231012155026090.png)

我们登陆默认就在public这个模式下，是数据库对象的一个集合。

创建其他模式：`create schema my123;`

为所有模式创建一个表：`create table test1(id int, name varchar(20));`

![image-20231012155100666](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231012155100666.png)

执行上述代码后，就多了一个模式my123，同时所有模式下都有表test1。



## 在python中通过库psycopg2对数据库进行增删改查操作

预先通过Terminal安装好第三方库：Pip install psycopg2

psycopg2库介绍: Psycopg2是一个用于python编程语言的第三方库，用于访问PostgreSQL数据库系统。它提供了一组工具和方法，可以轻松地在Python程序中进行数据库操作，包括查询、插入、更新、删除等操作。

`import psycopg2`

### 通过库psycopg2对数据库进行增操作

```py
#创建cursor来访问数据库，psycopg2提供了一个cursor类，用来在数据库Session里执行PostgresSQL命令。
cur = conn.cursor()
#通过python为postgresql数据库创建表
#创建表，本次创建的表是student1，用于存放学生的name，age，class三种信息。
cur.execute('''create table student1(id serial primary key, name varchar(10),age int,class int);''')
print("table created successfully!")   #成功创建表，提示表创建成功
#插入数据，通过数据库插入数据语句在表中输入相关信息
cur.execute("insert into student1(name,age,class) values('xiaoming',20,3)")
cur.execute("insert into student1(name,age,class) values('xiaohong',21,1)")
cur.execute("insert into student1(name,age,class) values('xiaogang',19,2)")
print("table inserted successfully!")  #成功输入数据，提示表中数据输入成功
#查询并打印数据
cur.execute("select * from student1")
rows = cur.fetchall()
print("----------------------------------------------------")
for row in rows:
   print("id=" + str(row[0]) + " name=" + str(row[1]) + " age=" + str(row[2]) + " class=" + str(row[3]))
print("----------------------------------------------------\n")
#提交事务
conn.commit()
#关闭连接
conn.close()
```

结果显示：

Python中显示的表数据如下：

![image-20231011210345066](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231011210345066.png)

PgAdmin4中通过查询表语句：`select * from student1;`可以找到表student1及其数据，如下：

![image-20231011210402152](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231011210402152.png)

也可以通过postgresql数据库中终端命令行去查询表student1的相关信息，查询结果如下：

![image-20231011210413738](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231011210413738.png)

通过上述结果，完成了python通过库psycopg2对数据库进行增操作，与PostgreSQL数据库连接后，python中表信息的创建可以同步到数据库中。



### 通过库psycopg2对数据库进行删操作

```py
# 删除表student1某行的数据，删除部分的python代码
cur.execute("delete from student1 where id=1")
cur.execute("select * from student1")
rows = cur.fetchall()
print("----------------------------------------------------")
for row in rows:
    print('id=' + str(row[0]) + ' name=' + str(row[1]) +' age=' + str(row[2]) + ' class=' + str(row[3]))
print("----------------------------------------------------\n")
```

结果显示：

Python中显示的表数据如下：表student1中少了id=1这一行信息

![image-20231011210507291](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231011210507291.png)

PgAdmin4中通过查询表语句：select * from student1;可以找到表student1及其数据，如下：

![image-20231011210519060](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231011210519060.png)

也可以通过postgresql数据库中终端命令行去查询表student1的相关信息，查询结果如下：

![image-20231011210535080](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231011210535080.png)

通过上述结果，完成了python通过库psycopg2对数据库进行删操作，与PostgreSQL数据库连接后，python中表信息的相关删除操作可以同步到数据库中。



通过库psycopg2对数据库进行删操作还包括删除整个表，其删除表的代码如下：

`cur.execute("drop table student")`

PgAdmin4中创建的所有表如左图所示，在执行完删除表student操作后，在右图可以看出该表已经删除。

![image-20231011210607228](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231011210607228.png)![image-20231011210613946](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231011210613946.png)



### 通过库psycopg2对数据库进行改操作

```py
#更新数据，将id为2数据中的age改为23，class改为3，代码如下：
cur.execute("update student1 set age=23,class=3 where id=2")
cur.execute("select * from student1")
rows = cur.fetchall()
print("----------------------------------------------------")
for row in rows:
    print('id=' + str(row[0]) + ' name=' + str(row[1]) + ' age=' + str(row[2]) + ' class=' + str(row[3]))
print("----------------------------------------------------\n")
```

结果显示：

Python中显示的据如下：表student1中id=2这一行信息的数据发生了变化age从21变成了23，class从1变成了3。

![image-20231011210701989](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231011210701989.png)

PgAdmin4中通过查询表语句：select * from student1;可以找到表student1及其数据，如下：

![image-20231011210714376](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231011210714376.png)

也可以通过postgresql数据库中终端命令行去查询表student1的相关信息，查询结果如下：

![image-20231011210725117](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231011210725117.png)

通过上述结果，完成了python通过库psycopg2对数据库进行改操作，与PostgreSQL数据库连接后，python中表信息的相关更改操作可以同步到数据库中。



### 通过库psycopg2对数据库进行查操作

```py
#数据库中的表信息显示在python中，实现一个查找操作，代码如下：
cur.execute("select * from student1")
rows = cur.fetchall()
print("----------------------------------------------------")
for row in rows:
    print('id=' + str(row[0]) + ' name=' + str(row[1]) +' age=' + str(row[2]) + ' class=' + str(row[3]))
print("----------------------------------------------------\n")
```

结果显示：

Python中显示的表数据如下：对数据库中表0student1进行查询操作，并显示出来。

![image-20231011210821565](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231011210821565.png)

通过上述结果，完成了python通过库psycopg2对数据库进行查操作，与PostgreSQL数据库连接后，数据库中的信息可以同步到python中。

