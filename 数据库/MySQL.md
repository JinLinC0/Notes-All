# `MySQL`

## 基本概念

`MySQL`是一种开放源代码的关系型数据库管理系统（`RDBMS`）（安装`Mysql`数据库，就是在主机安装一个数据库管理系统`DBMS`，一个数据库管理系统可以管理多个数据库），使用最常用的数据库管理语言–-结构化查询语言（`SQL`）进行数据库管理

***

### 数据库的三层结构

数据库分为三级关系：数据库服务器（`DBMS`数据库管理系统）、数据库、数据表

![image-20250430133152849](..\images\image-20250430133152849.png)

> 数据库中不是单单只有数据表，还有其他的部分，如视图等
>
> 数据库与计算机进行对比，数据库就类似一个文件夹（数据库-表的本质仍然是文件），我们在文件夹中要存储文件，这些文件就类似于我们后续要操作的数据表
>
> 数据在数据库中的存储方式是以表结构的形式进行存储的，表分为行(`row`)和列(`column`)，表的一行称之为一条记录

注意事项：

- `MySQL`的基本语句和`PHP`一样，在该行语句结束的时候，需要以分号进行结尾，不然这条语句是不执行的
- `\c`放弃当前语句执行：当我们有时候输入指令错误时，我们想要放弃这条指令的执行，我们只需要在这条指令后面输入`\c`，系统就会放弃这条指令的执行
- 退出命令行`Mysql`，我们只需要在`Mysql`命令行执行`exit`即可

当我们使用数据库软件的时候，比如`Navicat`时，我们想要通过命令行进行数据库的操作，我们可以点击左上角的新建查询按钮，选择相应的数据库连接和数据库，输入相关代码（在一行语句结束的时候一定要以分号进行结尾），点击运行即可，同时我们要知道`SQL` 的关键字和函数 名不区分大小写；`SQL`语句注释是在语句前面加`--`

***

### `SQL`语句分类

- `DDL`：数据定义语句，如创建表、库等
- `BML`：数据操作语句，如增加`insert`、修改`update`、删除`delete`
- `DQL`：数据查询语句，如查询`select`
- `DCL`：数据控制语句，管理数据库，如管理用户权限：授予权限`grand`、撤回权限`revoke`



## 连接数据库服务器

![image-20250430131336020](..\images\image-20250430131336020.png)

> 注意事项：
>
> - 连接之前要保证`Mysql`的服务已经启动
> - `-p`和密码之间不要有空格
> - `-p`后面如果没有写密码，回车就会要求输入密码

简化版本：`mysql -uroot -p`

> 输入方式也可以写成这样的形式：`mysql -u root -p`
>
> - `-u`表示是我们的账号，这个账号是`Mysql`软件的账号（不是当前登录系统的账号）
>
> - `-p`表示要输入我们的密码
>
> 回车，然后输入密码，输入密码后就会进入`MySQL`的操作管理界
>
> 该语句还有两个需要了解的参数
>
> - `-P`表示我们可以指定连接的端口，`Mysql`的默认端口是3306，不添加的时候，默认使用3306这个端口
> - `-h`表示连接到远程数据库的`ip`地址，默认情况下是连接到我们的本地主机（127.0.0.1）



## 导入外部`SQL`文件

有时候，我们会把`sql`语句存储在一个`SQL`文件里面（一般是我们备份出来的`SQL`文件，`SQL`文件是以`.sql`后缀结尾的），如果恢复备份的时候，我们需要导入这些`SQL`文件，相当于把这些`sql`语句重新执行一遍

如我们有一个`mysql.sql`的`SQL`文件，其内容为：

```sql
create database article charset utf8;
show databases;
```

在命令行进行数据库文件的导入：`mysql -uroot -p < mysql.sql`

我们也可以先进入数据库，在通过`source mysql.sql`进行导入



## 数据库管理操作

### 数据库相关

- 创建数据库：`create database 数据库的名称;`

  > 在创建语句中，我们可以指定数据库的字符集，如`create database 数据库的名称 charset utf8;`
  >
  > `utf8`表示可以显示包括中文在内的多种编码格式

- 查看创建数据库的结构：`show create database 数据库名称;`

  > 可以看到我们创建对应的数据库和及其结构和字符编码等信息

- 查看所有数据库：`show databases;`
- 使用/进入数据库：`use 数据库的名称;`
- 删除指定数据库：`drop database if exits 数据库名称;`

***

### 表相关

#### 创建表和插入数据

- 创建表：`CREATE TABLE 数据表的名称 (id int PRIMARY KEY AUTO_INCREMENT, cname varchar(30) not null, description varchar(100) null);`

  > 上述创建的数据表有3个表字段，`id`、`cname`和`description`
  >
  > - `id`是`int`数字类型；同时是一个主键（主键查找的速度比较快），通过`PRIMARY KEY`声明；`AUTO_INCREMENT`表示设置数据自增（随着数据条目的增加，`id`的值是自增的，保证`id`值的唯一性）主键在添加数据的时候我们一般不用管，它会自己进行维护，来一条数据自增一下（但是删除完对应的数据，`id`不会在后续继续使用，为了保证唯一性）
  > - `varchar`表示字符串类型的数据结构，30和100表示设置字符串的最大长度
  > - `not null`表示设置输入的内容不能为空（字段是必填项），`null`表示输入的内容可以为空

- 查看当前数据库中的表：`desc 数据表的名称`

  ![image-20241128171201609](..\images\image-20241128171201609.png)

  删除表：`drop table if EXISTS 数据表的名称`

  > `if EXISTS`这条语句建议加上，如果数据表不存在时，有这条语句后台是不会报错的

为数据表添加数据，通过命令行进行为数据表插入数据

- 插入一条数据：

  `INSERT INTO 数据库的名称 set cname = 'mysql', description = '学习mysql数据库';`

- 插入多条数据：

  `INSERT INTO class (cname, description) VALUES('Linux', '服务器知识'), ('git', null);`

> 当然，我们也可以直接在图形化界面中为数据表插入一条数据，在单元格中输入数据，最后点击保存

#### 根据其他表来快速的生成一张表

- 首先要创建一张表，同时说明这个新表的表结构来源于哪张旧表

  `create table 创建的新数据表名称 like 旧数据表的名称;`

  > `like`可以理解于数据结构来源于哪张旧表，这样创建的新表会和旧表有着一样的数据表字段和类型

- 有时我们想要在创建相同表结构的基础上还要将数据也复制过来，可以通过以下的方式：

  `insert into 创建的新数据表名称 select * from 旧数据表的名称;`

  > `select`表示查询操作；*表示所有字段；
  >
  > 有时候我们可能不想要所有的字段，我们只想要其中一个字段的数据，可以将代码进行以下的修改：
  >
  > - `insert into 创建的新数据表名称(cname) select cname from 旧数据表的名称;`

- 我们也可以将上述两个步骤合成一个，在创建的表的时候，使数据连同一起过来

  `CREATE TABLE 创建的新数据表名称 select * from 旧数据表的名称;`

  也可以创建新表的时候拿一部分字段的数据：

  `CREATE TABLE 创建的新数据表名称 (id int PRIMARY KEY AUTO_INCREMENT, cname varchar(30)) select cname from 旧数据表的名称;`

  当我们新表的字段名称和旧数据表的名称不同时，我们需要进行如下的修改：

  `CREATE TABLE 创建的新数据表名称 (id int PRIMARY KEY AUTO_INCREMENT, name varchar(30)) select cname as name from 旧数据表的名称;`

#### 查询表

查询表通过`SELECT`方式进行查询，具体的方式如下所示：

- `SELECT * FROM 要查询的数据表;`

  > `*`表示查询所有的字段

- 根据需要查询数据表中指定的字段：`SELECT cname, id FROM 要查询的数据表;`

  > 最后查询的结果返回也是根据查询时输入的字段顺序进行返回的（`cname`这一列字段在`id`这一列字段前面），有时候在多表关联的查询的时候，会出现同名的字段，这个时候就需要进行别名的设置：
  >
  > `SELECT cname, id as ids FROM 要查询的数据表;`
  >
  > 这时查询返回的字段名就是`cname`和`ids`

- 我们在实际查询操作的时候，往往是不会把一张表的所有数据都进行查询的（可能数据表的数据非常大，都拿出来不现实），我们一般使用基于条件的筛选查询（结果返回的是符合这个筛选条件的所有数据条目）：

  - 根据某个字段的数值大小进行筛选：`SELECT * FROM 数据表的名称 WHERE id > 2;`

  - 根据某个字段的内容进行查询筛选：`SELECT * FROM 数据表的名称 WHERE cname = 'mysql';`

  - 查找字段中包含某个元素的内容：`SELECT * FROM 数据表的名称 WHERE description like '%l%';`

    > 但是在实际应用中，`like`的搜索性能会有一定的差异，我们一般使用一些第三方的搜索，包括使用一些云主机的搜索，这些的搜索功能比较强悍

  上述的筛选条件进行组合在一起进行查找筛选，找到同时满足条件的数据：

  `SELECT * FROM 数据表的名称 WHERE description not like '%l%' and id > 2;`

  > `not`表示非；`and`表示与；`or`表示或
  >
  > `%l%`表示查找任何位置出现内容`l`；`%l`表示查找以`l`结尾的内容；`l%`表示查找以`l`开始的内容
  >
  > `_l%`表示查找第二个字符内容为`l`的记录
  >
  > `_`的作用范围是一个字符；`%`的作用范围是多个字符

- 在有的时候，我们需要将多个列的结果进行合并返回，我们可以使用连接函数进行连接：

  `SELECT CONCAT(cname, description) as class_info from 数据表的名称;`

  > 一般情况下，我们需要为其起别名，不然返回的字段是这整个连接函数的内容，会过于长了

##### 关键字查询

关键字查询中的关键字包括：`between`，`like`，`in`等等

在此基础上我们先新建另一个班级数据表：

`CREATE TABLE stu (id int PRIMARY key AUTO_INCREMENT, sname char(10), class_id int DEFAULT null, age SMALLINT not null);`

在新建的学生表中插入一些基本的数据：

`INSERT INTO stu(sname, class_id, age) VALUES('小明', 1, 22), ('小红', 2, 23), ('小亮', 2, 24), ('小白', null, 22);`

- 查找哪些班级有学生：`SELECT DISTINCT class_id FROM stu;`

  > `DISTINCT`的作用是去重，返回有学生的班级，班级不应该重复出现

- 查找年龄在18到23之间的数据，有以下的两种写法：

  `SELECT * FROM stu WHERE age >= 18 AND age <= 23;`

  `SELECT * FROM stu WHERE age BETWEEN 18 AND 23;`

- 查询学生班级在1班或者2班的数据，有以下的两种写法：

  `SELECT * FROM stu WHERE class_id = 1 OR class_id = 2;`

  `SELECT * FROM stu WHERE class_id in(1, 2);`

#### `Mysql`对`NULL`的处理技巧

空不能和任何的值相比较，如`stu`数据表单，我们通过`SELECT * FROM stu WHERE class_id = null;`是查询不到内容的（虽然在插入语句中有一条数据的`class_id`的内容为`null`）

针对这个问题，`Mysql`提供了相应的关键字：`SELECT * FROM stu WHERE class_id is null;`这样我们就可以查询到`('小白', null, 22)`这条记录

小案例：对于查询`stu`表的所有数据，如果有班级就显示班级的编号，如果班级字段的内容为`null`，就显示班级未分配：

```mysql
-- 方式一
SELECT sname, if(class_id, class_id, '未分配')，age from stu;
-- 方式二
SELECT sname, ifnull(class_id, '未分配'), age from stu;
```

#### 排序操作

我们可以对数据表中的某个字段进行排序操作：

对`stu`数据表中的年龄字段进行从大到小进行排序：`SELECT sname, age FROM stu order by age desc;`

> 对于排序：`desc`表示降序（从大到小）；`asc`表示升序（从小到大）
>
> `null`表示最小，如果升序排序，会出现在最前面

小案例：对于班级进行升序排序，如果班级相同则通过年龄进行升序排序：

```mysql
SELECT * FROM stu order by class_id asc, age asc;
```

> 排序可以指定多个字段
>
> 同时对于返回的数据条目，我们可以指定个数进行展示，下面例子表示取输入的最后一条数据（先进行降序排序，在进行`limit`取选取一条数据）
>
> `SELECT * FROM stu order by class_id asc limit 1;`
>
> 也可以用如下的方式进行写：`SELECT * FROM stu order by class_id asc limit 0,1;`
>
> `limit`也可以接收两个参数，第一个表示从哪一个开始，第二个表示取几条数据，如`limit 0, 2`表示取第一条和第二条数据（从第一条数据开始取，取两个）

#### 更新表数据

我们更新表数据一般使用`UPDATE`关键字，更新一般是需要加上条件的，不然就会将这张表中要更新的字段内容全部修改了

- 将`stu`数据表中`class_id`为空的字段设置为`class_id`为2：

  `UPDATE stu SET class_id = 2 WHERE class_id is NULL;`

- 将年龄小于22岁的人员班级设置为班级1：

  `UPDATE stu SET class_id = 1 WHERE age < 22;`

#### 删除表数据

我们删除表中的数据一般使用`DELETE`关键字，删除条目一般也是需要加上条件的

- 删除班级为`null`的这条数据：`DELETE FROM stu WHERE class_id is NULL;`
- 删除最后报名的两个学生：`DELETE FROM stu order by id desc limit 2;`

#### 修改数据表的结构

修改数据表的结构是一个相对低频的操作，一般在确定表后，都很少进行其结构的修改，但是也需要了解一下表结构修改的`SQL`语句

- 修改数据表的名称，将数据表`stu`的名字修改为`stus`，有以下的两种方式：

  - `ALTER TABLE stu RENAME stus;` 

  - `RENAME TABLE stu to stus;`

- 创建一个备份表，将一张数据表进行备份：`CREATE TABLE stu_bak SELECT * FROM stu;`

- 修改数据表的字符集：字符集一般是在创建表的时候确定的，后续不会轻易修改的，但也可以修改：

  `ALTER TABLE stu charset gbk;`    将字符集改为`gbk`的形式
  
- 清空表数据：`DELETE FROM stu;`   将`stu`表的数据全部清空，有些数据库管理软件会进行二级确认，这样的删除方式是一条一条进行删除，删除的速度是比较慢的，如果想要快速的进行清空表中的数据，可以使用：`TRUNCATE stu;`

#### 表字段的维护

有时候我们可能需要对数据表中字段进行维护操作，如修改表字段的名称、类型等等

- 修改表字段名称，同时修改数据类型和设置必填：

  `ALTER table stu CHANGE sname name varchar(50) not null;`

- 只是修改数据类型和是否必填：`ALTER table stu MODIFY sname varchar(50) not null;`

- 添加一个新的字段：`ALTER table stu ADD sex smallint default null;`

  > 添加一个新的性别字段，字段名称为`sex`，字段类型为`smallint`，字段的默认值为`null`

- 想让指定字段在某个位置进行存放，创建一个新字段，并且使这个新字段在`id`字段的后面：

  `ALTER table stu ADD email varchar(50) default null AFTER id;`

  让一个新字段出现在所有字段的最前面：

  `ALTER table stu ADD qq varchar(50) default null first;`

- 删除字段：`ALTER table stu drop qq;`    删除`qq`字段

#### 对表中主键的维护

对表中主键的维护是一个非常低频的操作

- 删除主键：对于自增的主键是没有办法进行直接删除的，我们需要将其自增的属性先修改掉，再删除主键

  - 将主键自增的属性去掉：`ALTER table stu MODIFY id int not null;`
  - 再进行主键的删除：`ALTER table stu drop PRIMARY key;`

  上述两个操作执行完，`id`的主键属性是消失了，但是`id`这个字段还是存在的，变成了一个普通的字段

- 添加主键：为上述删除主键的`id`重新添加回主键：`ALTER table stu add PRIMARY key (id);`

- 添加主键的自增属性：`ALTER table stu MODIFY id int not null AUTO_INCREMENT;`

- 添加主键和添加自增属性可以一起执行：

  `ALTER table stu modify id int not null AUTO_INCREMENT, add PRIMARY key (id);`



## 导入外部的`SQL`文件

有的时候，我们会把`sql`语句存储在一个`SQL`文件里面（一般是我们备份出来的`SQL`文件，`SQL`文件是以`.sql`后缀结尾的），如果恢复备份的时候，我们需要导入这些`SQL`文件，相当于把这些`sql`语句重新执行一遍

如我们有一个`mysql.sql`的`SQL`文件，其内容为：

```sql
create database article charset utf8;
show databases;
```

在命令行进行数据库文件的导入：`mysql -uroot -p < mysql.sql`

我们也可以先进入数据库，在通过`source mysql.sql`进行导入



## 数据类型
### 字符串类型

在数据库的所有类型中，字符串类型是使用最频繁的，数据库中字符串类型下有非常多种类的类型，如下表所示：

|     类型     |       大小        |              用途               |
| :----------: | :---------------: | :-----------------------------: |
|    `CHAR`    |    0-255 字节     |           定长字符串            |
|  `VARCHAR`   |   0-65535 字节    |           变长字符串            |
|  `TINYBLOB`  |    0-255 字节     | 不超过 255 个字符的二进制字符串 |
|  `TINYTEXT`  |    0-255 字节     |          短文本字符串           |
|    `BLOB`    |   0-65535 字节    |     二进制形式的长文本数据      |
|    `TEXT`    |   0-65535 字节    |           长文本数据            |
| `MEDIUMBLOB` |  0-16777215 字节  |  二进制形式的中等长度文本数据   |
| `MEDIUMTEXT` |  0-16777215 字节  |        中等长度文本数据         |
|  `LONGBLOB`  | 0-4294967295 字节 |    二进制形式的极大文本数据     |
|  `LONGTEXT`  | 0-4294967295 字节 |          极大文本数据           |

字符串通常来讲分为二进制（`mp3`，照片和视频都属于二进制类型，我们可以作为二进制存放到`mysql`中）和非二进制类型（文本内容等等）

`mysql`可以存放二进制文件，但是我们一般不存放二进制文件，这些图片视频二进制数据会使我们的数据表体积变得很多，造成读取时间变慢，所以说二进制文件一般不使用`mysql`进行存储的，这些视频照片一般存储在第三方的`oss`中，将这些较大的静态数据放到云上，加载速度会更快

> `char`和`varchar`字符串类型是我们用的比较多的字符串类型
>
> - `char` 类型是定长类型，比如定义了 20 长度的`char`类型，只存一个字符也占用 20 个长度，`char` 好处是处理速度快，缺点是空间占用大（空间换取速度，检索这一类字符的时候速度非常快），把手机号、邮箱、密码等长度相对固定的设置为 `char` 类型是不错的选择，或者牵扯到排序的字符串数据也可以设置为`char`类型
>
> - `varchar `类型与 `char` 相反，点用空间受内容影响，可以把文章标题、介绍等设置为` varchar `类型更合适

#### 字符集

字符集决定了我们在字段中能存储那类的字符，不符合字符集的文字可能会在存储的时候出现乱码

查看系统都支持哪些字符集：`show CHARACTER set;`

当使用到字符串，就一定会使用到字符集，在现实当中的字符集就是字典，字符集的种类是非常多的，有些字符集是比较全面的，包含了大多数语言，如`UTF8`字符集，在创建数据库的时候可以进行设置字符集，在建表的时候也可以设置字符集，表中的字段也有字符集，最终的数据是存储在字段中的，所以字符集以字段的为准

> 如果表没有设置字符集，那么表的字符集就会继承于数据库的
>
> 如果字段没有设置字符集，就会继承于表的字符集
>
> 但是在后续修改上层的字符集，不会使下层已有内容的字符集发生改变，但是对于新建的内容，字符集就会继承修改后的新字符集

#### 校对规则

数据库的校对规则有很多， 比如对于字符串的校对规则有：区分大小写

在默认的情况下，通过字符串进行比较查询（甚至排序）是不区分大小写的，如果我们想要让其进行区分大小写，我们就需要更改校对规则进行实现，一般校对规则是跟在字符集后面的，如`utf8_bin`，其中`utf8`是字符集；`bin`是区分字符串大小写的校对规则

总之：字符串校对规则是影响字符串比较和排序的一种算法

#### 字符串函数

字符串函数可以方便我们快速的解决一些字符串拆分和合并相关的一些问题

常见的字符串函数：

|           函数           |                             描述                             |
| :----------------------: | :----------------------------------------------------------: |
|    `left(字段名, i)`     |      从给定字段内容的从左边进行取i个字符串（包括第i个）      |
|    `right(字段名, i)`    |      从给定字段内容的从右边进行取i个字符串（包括第i个）      |
|   `mid(字段名, i, n)`    | 从给定字段内容的任何位置的第i个位置取n个字符串，如果没有输入n，表示从第i个位置进行取后面所有字符串 |
|  `substring(字段名, i)`  | 对于表中的给定字段名，从该字段内容的第i个开始取（包括第i个），一直取到最后 |
|  `char_length(字段名)`   |                  获取给定字段中字符串的长度                  |
| `concat("前缀", 字段名)` |         给选定的字符串的所有内容都连接一个字符串前缀         |

小案例：如果字段内容的长度大于8个，我们在8位之后使用`...`进行连接：

```mysql
select if (char_length(cname)>8,concat(left(cname,8),'...'),cname) as cname from class;
```

> 如果长度大于8位，8位后面使用`...`进行连接；如果长短小于8位，不做任何处理

***

### 数值类型

数值类型就是用于存储我们的数字的，常见的数值类型有以下的几个类型：

|    数据类型    |       范围（有符号）（默认情况）       |      范围（无符号）       |
| :------------: | :------------------------------------: | :-----------------------: |
|  `tinyint(n)`  |        1 个字节的范围(-128~127)        |         (0，255)          |
| `smallint(n)`  |      2 个字节的范围(-32768~32767)      |        (0，65535)         |
| `mediumint(n)` |    3 个字节的范围(-8388608~8388607)    |       (0，16777215)       |
|    `int(n)`    | 4 个字节的范围(-2147483648~2147483647) |      (0，4294967295)      |
|  `bigint(n)`   |      8 个字节的范围(+-9.22*10^18)      | (0，18446744073709551615) |

> 其中n是设置我们的显示位数
>
> 越往下范围越大，所占用的磁盘空间就越大，实际中选则合适大小的即可，如果选则的类型占用磁盘过大，数据在检索的时候时间就越长，效率就低

```mysql
-- 为class数据表中添加status字段，字段是数值类型的，具体为TINYINT
alter table class add status TINYINT;

-- 有时候我们不想让数值类型有符号，让数值类型只能存储正数的类型
alter table class add status TINYINT UNSIGNED;
```

#### n和`ZEROFILL`

数值类型后面跟着的(n)表示设置我们数值的显示位数，并不是说只能输入n个数字字符

```mysql
alter table class add state int(5) ZEROFILL;
```

`int(5)`只控制显示长度，如果添加了前导零填充这个关键字`ZEROFILL`后，如果长度不够5位，会在数值前面进行补0，使其长度控制在5位，但是前导零在数据库图形化界面中有时难以显示，在命令行终端中可以正常显示

#### 浮点数和定点数

浮点数和定点数可以理解数值类型带小数的数据类型

浮点数数据类型：

|   类型    |                             精度                             |
| :-------: | :----------------------------------------------------------: |
|  `FLOAT`  |                     7位，能保证6位不损耗                     |
| `DOUBLE`  |                    16位，能保证15位不损耗                    |
| `DECIMAL` | `DECIMAL(M,D) `，M表示总位数（包括小数点前和后），D表示小数的位数 |

选择的时候根据需要进行选择，在够用的情况下选择最小的即可

`FLOAT`类型和`DOUBLE`的浮点数有精度的误差：

- `FLOAT`：一共7位（包括小数点的前和后），意味着最多有7位有效数字，在7位以上会有精度的损耗（数值就会与我们添加的不一致，可能给你四舍五入了），但绝对能保证6位，即`FLOAT`的精度为6-7位有效数字
- `DOUBLE`：一共16位，`double`数据类型的精度为15-16位
- 对于货币之类的数据，推荐使用`DECIMAL`数据类型，通过设定`M`和`D`可以精准的显示，不损耗

```mysql
alter table class add a FLOAT(10,2);
update class set a = 12345678.66 where id=1;   -- 出现损耗
alter table class add b DECIMAL(10,2);
update class set b = 12345678.66 where id=1;   -- 不会出现损耗
```

***

### 枚举类型

对于有一些数据，有多个值，但是每次我们在设置的时候，只设置一个值，类似单选框的原理，这种数据类型，我们就可以使用`EMUM`枚举类型，如对于性别类型的数据，通常有：男、女类型，这种情况，就可以设置为枚举类型，我们将原先班级表的性别字段的类型修改为枚举类型：

```mysql
ALTER TABLE stu MODIFY sex ENUM('男','女') DEFAULT NULL;
```

系统在默认的情况下会将1当作男；将2当作女，所有插入一条数据时，可以有以下的两种方式：

```mysql
INSERT INTO stu (sname, class_id, sex) VALUE('小明',1,'男');
INSERT INTO stu (sname, class_id, sex) VALUE('小明',1,1);

-- 查找所有的女同学
SELECT * from stu WHERE sex = 2;
```

***

### `SET`多选类型

对应枚举类型是一个单选的类型，`SET`类似是一个多选的类型，字段内容可以选择`SET`类型设定种的一个或多个，对应`SET`中多选内容的个数，在`mysql`中规定了最多添加64个

```mysql
ALTER TABLE stu ADD flag SET('推荐','热门','图文');
-- 添加数据
INSERT INTI stu (sname, flag) VALUES('小明','推荐,图文')
-- 查找包含某个选项的数据条目
SELECT * FROM stu WHERE FIND_IN_SET('推荐', flag);
-- 也可以使用like进行模糊匹配的方式进行查找
SELECT * FROM stu WHERE flag LIKE '%推荐%';
```

#### 二进制模糊匹配`SET`类型

对于`SET('推荐','热门','图文')`的`SET`类型，其第一个内容推荐，其二进制表示为0001（2的0次方：1）；对于第二个内容热门，其二进制表示为0010（2的1次方：2）；第三个内容图文，其二进制表示为0100（2的2次方：4）；如果有第四个，其二进制表示为1000（2的3次方：8）

我们可以通过二进制的方式进行`SET`类型的模糊匹配：

```mysql
-- 匹配flag字段内容包含推荐的数据条目
SELECT * FROM stu WHERE flag & 1;

-- 匹配flag字段内容包含推荐或者热门的数据条目
SELECT * FROM stu WHERE flag & 3;

-- 匹配flag字段内容包含热门或者图文的数据条目
SELECT * FROM stu WHERE flag & 6;
```



## 时间日期

使用图形化数据库软件，一般都要进行配置时区，全世界不同的地方的显示时间是不一样的，有些软件没有给我们默认配置时区，我们需要进行手动的配置，一般选择亚洲上海即可

### 日期时间数据类型

对于日期时间的数据类型，`mysql`提供了以下的时间日期类型：

| 日期时间类型 | 占用空间 |       日期格式        |        最小值         |        最大值         |       零值表示        |
| :----------: | :------: | :-------------------: | :-------------------: | :-------------------: | :-------------------: |
|  `DATETIME`  | `8bytes` | `YYYY-MM-DD HH:MM:SS` | `1000-01-01 00:00:00` | `9999-12-31 23:59:59` | `0000-00-00 00:00:00` |
| `TIMESTAMP`  | `4bytes` | `YYYY-MM-DD HH:MM:SS` | `1970-01-01 08:00:01` | `2038-01-19 03:14:07` |   `00000000000000`    |
|    `DATE`    | `4bytes` |     `YYYY-MM-DD`      |     `1000-01-01`      |     `9999-12-31`      |     `0000-00-00`      |
|    `TIME`    | `3bytes` |      `HH:MM:SS`       |     `-838:59:59`      |      `838:59:59`      |      `00:00:00`       |
|    `YEAR`    | `1bytes` |        `YYYY`         |        `1901`         |        `2155`         |        `0000`         |

> - 日期类型也可以存储为字符串的类型，但是如果后期要进行计算就不好计算了，时间日期类型有专门的计算函数可以提供计算使用
>
> - 选择时间日期类型，我们需要考虑数据类型的大小和格式，够用就行
>
> - `TIMESTAMP`表示时间戳类型
>
>   - 设置更新时间，默认的时间是当前的时间戳：（显示的还年月日时分秒的时间格式）
>
>     `ALTER TABLE stu ADD updated_at timestamp DEFAULT CURRENT_TIMESTAMP;`
>
>     > - 使用时间戳类型需要注意时间日期的设置范围，不要超出，不然会报错
>     >
>     > - 上面设置的更新时间，修改当前数据的其他字段的内容，更新时间字段的数据不会发生改变，对于更新时间这个字段来说，这是不合理的，我们需要当该数据有任何的字段内容发生变化后，就要触发更新时间的更新，需要做以下的修改：（当有内容更新的时候，设置更新时间也为当前时间）
>     >
>     >   `ALTER TABLE stu ADD updated_at timestamp DEFAULT CURRENT_TIMESTAMP ON UPDATE;`
>
> - `Mysql `保存日期格式使用` YYYY-MM-DD HH:MM:SS `的` ISO 8601 `标准，向数据表储存日期与时间必须使用` ISO` 格式

总结，在选择时间日期数据类型的时候，如果要牵扯到计算（计算考勤等），我们就选择上面的时间日期类型，如果只是单纯的记实时间，只有用于展示的，我们可以选择`int`类型或者字符串类型

***

### 时间日期数据类型的使用

- 添加一个字段，其类型为时间日期的数据类型：`ALTER TABLE stu ADD birthday datetime default null;`

  > 在数据表`stu`中添加一个`brithday`字段，该字段类型为`datetime`，字段的默认值允许为空

- 插入一条数据：`UPDATE stu set birthday = "2000-02-13 03:22:10" WHERE id = 1;`

***

### 格式化输出

我们可以通过时间日期的格式化输出获取任何我们想要的时间数据，我们需要使用`DATE_FORMAT`来格式化输出时间日期

格式化输出的参数：

| 格式 |                      描述                      |
| :--: | :--------------------------------------------: |
| `%a` |                   缩写星期名                   |
| `%b` |                    缩写月名                    |
| `%c` |                    月，数值                    |
| `%D` |             带有英文前缀的月中的天             |
| `%d` |              月的天，数值(00-31)               |
| `%e` |               月的天，数值(0-31)               |
| `%f` |                      微秒                      |
| `%H` |                  小时 (00-23)                  |
| `%h` |                  小时 (01-12)                  |
| `%I` |                  小时 (01-12)                  |
| `%i` |               分钟，数值(00-59)                |
| `%j` |                年的天 (001-366)                |
| `%k` |                  小时 (0-23)                   |
| `%l` |                  小时 (1-12)                   |
| `%M` |                      月名                      |
| `%m` |                月，数值(00-12)                 |
| `%p` |                    AM 或 PM                    |
| `%r` |       时间，12-小时（hh:mm:ss AM 或 PM）       |
| `%S` |                   秒(00-59)                    |
| `%s` |                   秒(00-59)                    |
| `%T` |            时间, 24-小时 (hh:mm:ss)            |
| `%U` |        周 (00-53) 星期日是一周的第一天         |
| `%u` |        周 (00-53) 星期一是一周的第一天         |
| `%V` |  周 (01-53) 星期日是一周的第一天，与 %X 使用   |
| `%v` |  周 (01-53) 星期一是一周的第一天，与 %x 使用   |
| `%W` |                     星期名                     |
| `%w` |         周的天 （0=星期日, 6=星期六）          |
| `%X` | 年，其中的星期日是周的第一天，4 位，与 %V 使用 |
| `%x` | 年，其中的星期一是周的第一天，4 位，与 %v 使用 |
| `%Y` |                    年，4 位                    |
| `%y` |                    年，2 位                    |

格式化输出的使用：`SELECT DATE_FORMAT(birthday, '%Y年%m月%d日') from stu;`

> 结果显示：`2000年02月13日`

`Date_format`格式化日期与时间显示，还有一种时间日期的格式化方法：`time_format`，只能格式化输出时间：

`SELECT TIME_FORMAT(birthday, '%h:%i:%s') from stu;`

***

### 时间处理的函数

`Mysql`提供了非常丰富的时间处理函数，具体如下所示：

|       函数       |                          说明                          |
| :--------------: | :----------------------------------------------------: |
|      `HOUR`      |                  时（范围从 0 到 23）                  |
|     `MINUTE`     |                  分（范围从 0 到 59）                  |
|     `SECOND`     |                  秒（范围从 0 到 59）                  |
|      `YEAR`      |               年（范围从 1000 到 9999）                |
|     `MONTH`      |                  月（范围从 1 到 12）                  |
|      `DAY`       |                  日（范围从 1 开始）                   |
|      `TIME`      |                        获取时间                        |
|      `WEEK`      |             一年中的第几周，从 1 开始计数              |
|    `QUARTER`     |              一年中的季度，从 1 开始计数               |
|  `CURRENT_DATE`  |                        当前日期                        |
|  `CURRENT_TIME`  |                        当前时间                        |
|      `NOW`       |                        当前时间                        |
|   `DAYOFYEAR`    |              一年中的第几天（从 1 开始）               |
|   `DAYOFMONTH`   |                月份中天数（从 1 开始）                 |
|   `DAYOFWEEK`    |                星期天（1）到星期六（7）                |
|    `WEEKDAY`     |                星期一（0）到星期天（6）                |
|    `TO_DAYS`     |           从元年到现在的天数（忽略时间部分）           |
|   `FROM_DAYS`    |       根据距离元年的天数得到日期（忽略时间部分）       |
|  `TIME_TO_SEC`   |              时间转为秒数（忽略日期部分）              |
|  `SEC_TO_TIME`   |            根据秒数转为时间（忽略日期部分）            |
| `UNIX_TIMESTAMP` |           根据日期返回秒数（包括日期与时间）           |
| `FROM_UNIXTIME`  |        根据秒数返回日期与时间（包括日期与时间）        |
|    `DATEDIFF`    | 两个日期相差的天数（忽略时间部分，前面日期减后面日期） |
|    `TIMEDIFF`    |           计算两个时间的间隔（忽略日期部分）           |
| `TIMESTAMPDIFF`  |  根据指定单位计算两个日期时间的间隔（包括日期与时间）  |
|    `LAST_DAY`    |                     该月的最后一天                     |

基本使用：使用频率比较高的时间处理函数

- 获取时间日期数据的年月日：`SELECT YEAR(birthday), MONTH(birthday), DAY(birthday) from stu;`

  > 通过时间处理函数，可以把时间中的时间信息进行独立和局部的获取

- 获取当前时刻的日期和时间：`SELECT NOW();`

  > 我们也可以进行当前日期和时间的分开获取：`SELECT CURRENT_DATE();`和 `SELECT CURRENT_TIME();`

- 查看给定的日期时间是一年当中的第几天：`SELECT DAYOFYEAR(NOW());`

  > 同理，也可以查看给定的时间是当前月的第几天`DAYOFMONTH`；是当前周的第几天`DAYOFWEEK`（星期六是7，星期天是1）或者使用`WEEKDAY`（星期一是0，星期天是6）

小案例：如果当前时间大于更新时间`publish_time`时，所属条目的状态字段`status`就由0更新到1：

```sql
update 数据表名称 set status = 1 WHERE status = 0 and publish_time < now();
```

- 定义变量获取当前的时间：`set @time = time(now());`        输出变量：`select @time;`

- 将时间转化成秒数：`select TIME_TO_SEC(@time);`        `00:00:00`为第0秒

- 也可以将秒数转化回时间：`select SEC_TO_TIME(TIME_TO_SEC(@time));`

- 将当前时间转化为从元年到现在所经过的天数：`select TO_DAYS(now());`

- 查看出生到现在所经过的天数：`select DATEDIFF(now(), birthday) from stu;`

- 根据单位来获取时间差：`select TIMESTAMPDIFF(day, birthday, now()) from stu;`

  > 按天来进行比较，比较两段时间之间相差多少天；也可以按照月来进行比较`month`；可以按照周数`week`；可以按照分钟数据来进行比较`minute`；可以按照秒数来进行比较`second`

- 查找出生年月在某个范围内：

  `SELECT  * from stu WHERE birthday BETWEEN '1995-01-01' AND '2005-01-01';`

- 查找年龄最小的（先降序排列，再取第一个）：

  `SELECT * from stu order by birthday desc limit 1;`

- 查找在2000年出生的学生：`SELECT * from stu WHERE YEAR(birthday) = '2000';`

- 查找20岁以上的学生：`SELECT * from stu WHERE TIMESTAMPDIFF(YEAR, birthday, NOW()) > 20;`

***

### 日期时间的计算

#### 日期时间的提前和推迟

在有的时候，我们需要对日期和时间进行计算，如：在某个时间后面再加几个小时

- 在当前的时间上再加8个小时：

  `SELECT addtime(now(), '08:00:00');`或`SELECT timestamp(now(), '08:00:00');`

- 得到当前日期时间7天之后的日期时间：`SELECT date_add(now(), INTERVAL 7 DAY);`

  > 如果7前面加个负号，就表示获取7天之前的日期时间，也可以使用`date_add`的反函数`date_sub`
  >
  > 对于日期时间的提前和推迟，我们不仅可以使用天，也可以使用年月日，时分秒等等
  >
  > - 提前或推迟10个小时30分钟：`SELECT date_add(now(), INTERVAL "10:30" HOUR_MINUTE);`
  > - 提前或推迟3天8小时：`SELECT date_add(now(), INTERVAL "3 8" DAY_HOUR);`

#### 月初月末的计算

- 获得当前日期时间在当前月月末的日期：`SELECT last_day(now());`

- 获取当前日期时间上个月的最后一天的日期时间：`SELECT last_day(date_sub(now(), INTERVAL 1 MONTH));`

- 获取当前日期时间下个月的最后一天的日期时间：`SELECT date_add(last_day(now()), INTERVAL 1 DAY);`

- 获取当前日期时间在当前月月初的日期：`SELECT date_sub(now(), INTERVAL DAYOFMONTH(now())-1 DAY);`

  > `DAYOFMONTH(now())`表示当前日期时间是这个月的第几天

小案例：获取本月发表文章条目：文章的数据表为`article`，包括字段发布时间：`publish_time`

```sql
SELECT * FROM article
WHERE publish_time <= last_day(now())
ADD publish_time >= date_sub(now(), INTERVAL DAYOFMONTH(now())-1 day);
```

#### 日期对周的控制

在`mysql`中，系统提供了两个常用的函数来实现日期对周的控制：`DAYOFWEEK`（星期天（1）到星期六（7））和`WEEKDAY`（星期一（0）到星期天（6））

获取当前日期所在周星期二的日期：`SELECT date_add(now(), INTERVAL 3-DAYOFWEEK(now()) DAY);`

> 如果想要获得星期三的日期，就使用4减去`DAYOFWEEK(now())`即可
>
> 如果使用`WEEKDAY`的方法获得当前日期所在周星期二的日期：`SELECT date_add(now(), INTERVAL 1-WEEKDAY(now()) DAY);`

获取当前日期三周之前星期二的日期：

`SELECT date_sub(date_add(now(), INTERVAL 1-WEEKDAY(now()) DAY), INTERVAL 21 Day);`

***

### 日期时间常见案例

#### 月考勤

查找本月迟到的学生：创建一张考情表`attendance`，包含字段表示打卡时间`created_at`

```sql
SELECT * FROM attendance WHERE time(created_at) > '08:30:00'
AND date(created_at) > date(date_sub(now(), INTERVAL DAYOFMONTH(now())-1 DAY));
```

查找本月迟到超过两次的学生：

```sql
SELECT stu_id, count(id) FROM attendance WHERE time(created_at) > '08:30:00'
AND date(created_at) > date(date_sub(now(), INTERVAL DAYOFMONTH(now())-1 DAY))
GROUP by stu_id
HAVING count(id) >=2;
```

#### 周考勤

查找本周迟到的学生：创建一张考情表`attendance`，包含字段表示打卡时间`created_at`

```sql
SELECT * FROM attendance WHERE time(created_at) > '08:30:00'
AND date(created_at) > date(date_add(now(), INTERVAL 0-WEEKDAY(NOW()) DAY));
```

在计算日期时间时，会使用大量的函数，函数在数据库中进行操作时，会为每一条数据都执行函数，比较消耗性能，一般后续都在后端得到具体的日期，再在数据库中进行比对，这样比较高效



## 排序和统计

### 排序

排序可以对枚举类型，数值类型等进行排序；排序是先拿数据，再对数据做排序处理（因此条件`WHERE`是在排序`ORDER`之前的）

排序是我们对查询到的结果进行排序，排序分为降序（从大到小`DESC`）和升序（从小到大`ASC`）

- 对年龄进行升序排序：`SELECT * FROM stu ORDER BY age ASC;`
- 对性别进行降序排序，之后再对年龄进行升序排序：`SELECT * FROM stu ORDER BY sex DESC, age ASC;`

对于想要获取最大的数据，我们可以先进行排序，再进行选取一条记录：`LIMIT 1`

根据学生出生的月份进行降序排序：`SELECT sname, birthday, MONTH(birthday) as m FROM stu ORDER BY m DESC;`

#### 随机排序

当我们想要随机获取一些数据，我们可以使用随机函数进行生成：`SELECT RAND();`（为某条数据生成一个字段，字段中是随机数）

我们可以使用随机函数产生的随机顺序进行排序：`SELECT * FROM stu ORDER BY RAND() DESC;`

随机取一条数据：`SELECT * FROM stu ORDER BY RAND() DESC LIMIT 1;`

#### 自定义排序

自定义排序是使用集合进行排序的

`SELECT field('a','f','a','b','c');`  返回的结果为2，表示`a`在集合（`fabc`）中第2个位置出现

使用上述的特性可以进行自定义的排序，如希望某些字段在前面出现

按照学生的姓式进行排序：`SELECT sname, left(sname,1) as s FROM stu ORDER BY FIELD(s, '金') DESC;`

> 将姓金的排在最前面
>
> 增加姓式列表：'金'，’李‘，’陈‘，其他不变，那么陈就在最上面，之后是李，最后是金，（s中于金匹配的，结果就为1，s中与李匹配的，结果就是2，经过降序排列，结果大的就在上面，s中的内容如果集合中没有，结果就取0，就放在最后了）

***

### 统计

#### 统计数量

统计是对我们查询到的结果进行的一个汇总，使用函数`count()`进行汇总

- 汇总查询到所有的条目数据：`SELECT count(*) FROM stu;`       查询到几条数据就返回几

- 查询学生表中所有男同学的数量：`SELECT COUNT(*) FROM stu WHERE sex = '男';`

> *表示对所有的记录进行统计
>
> 我们可以进行有条件的统计，学生表中有的学生分配了班级，有的学生没有分配班级，我们需要统计分配了班级的学生数量：
>
> - 方法一：`SELECT COUNT(*) FROM stu WHERE class_id IS NOT NULL;`
> - 方法二：`SELECT COUNT(class_id) FROM stu;`      统计明确的字段，在统计的时候是不会记录`NULL`值的

#### 最值

对于最值的获取，我们之前是使用排序并取最上面一个的方式进行获取，但是`mysql`系统提供了两个内置的系统函数：`MIN`和`MAX`

- 获取某字段的最大值：`SELECT MAX(age) FROM stu;`
- 获取某字段的最小值：`SELECT MIN(age) FROM stu;`
- 获取年龄最小学生的出生年份：`SELECT year(max(birthday)) from stu;`      出生年份最大，年龄最小

当我们使用分组操作或者聚合函数的时候（统计、最值），如果使用额外的字段，如：`SELECT MIN(age), id FROM stu;`（其中的`id`就是额外的字段），在一些新版的`mysql`中会出现`ONLY_FULL_GROUP_BY`问题

对于这个错误，可以在一些框架的配置文件中进行配置的更改，配置为允许可以使用其他的字段

如果在`sql`命令行中继续操作，我们将`SET sql_mode=(SELECT REPLACE(@@sql_mode,'ONLY_FULL_GROUP_BY',''));`命令放到`SELECT MIN(age), id FROM stu;`命令前执行即可

#### 求和和平均

求和和平均在`mysql`中提供了两个系统函数：`SUM`和`AVG`

- 获取文章总的点击次数：`SELECT SUM(click) FROM article;`
- 获取文章平均的点击次数：`SELECT AVG(click) FROM article;`
- 获取小于平均点击数的文章信息：`SELECT * FROM article WHERE click < (SELECT AVG(click) FROM article);`

> `round()`函数表示四舍五入，一般和取平均进行结合使用

#### 去重

在我们获取班级的编号时，往往会出现一些相同的值，这个时候进行去重操作，将相同的过滤掉，我们可以使用关键字`DISTINCT`进行操作：`SELECT DISTINCT class_id FROM stu WHERE class_id is not null;`

##### 组合去重

`SELECT DISTINCT class_id,name FROM stu WHERE class_id is not null;`表示将两个字段（`class_id`和`name`）的内容放到一起进行过滤去重，只有同一个班级和同一个姓名的内容将会被去重，即将两个重复的归类到一组

#### 分组统计

比如我们要统计每个班级有多少个学生，这个时候就需要使用到分组统计的方式：先使用班级编号进行分组，再进行统计，具体语句如下：

```sql
SELECT count(class_id),class_id from stu GROUP BY class_id;
```

统计每个班级年龄最大的学生，即出生年月最小的：

```sql
SELECT min(birthday),class_id from stu GROUP BY class_id;
```

在分组统计的时候可以进行条件的添加，如条件每个班级中有多少个男同学：

```sql
SELECT count(class_id),class_id from stu WHERE sex = '男' GROUP BY class_id;
```

总而言之，`GROUP BY`是对查询结果进行分组，总是出现在`WHERE`条件的后面

##### 多字段的分组

多字段的分组表示分组的条件不单单只有一个，如在按照班级分组的同时，又要按照性别进行分组，如：

```sql
SELECT count(*),class_id,sex from stu GROUP BY class_id,sex;
```

> 统计每个班男生有几个，女生有几个

使用多字段分组进行统计，是按照对应字段的先后依次进行统计的

对统计完的分组进行排序，排序的位置是在分组后面，要先分组再进行排序

```sql
SELECT count(*),class_id,sex from stu GROUP BY class_id,sex ORDER BY class_id desc;
```

> 将分组的内容根据`class_id`进行大到小排序

##### 筛选分组

对分组的结果进行筛选，要使用`HAVING`方法，原先的`WHERE`条件是对查询到的结果进行筛选

```sql
SELECT count(*),class_id from stu GROUP BY class_id HAVING count(*)>=2;
```

> 对班级进行分组，并筛选出每组中人数大于等于2的组别
>
> 分组筛选用于筛选出一部分满足条件的组

查询本周迟到超过2次的同学信息：

```sql
SELECT count(*) as c stu_id FROM stu 
WHERE date(created_at)>=date(date_add(now(),interval 0-WEEKDAY(now()) day)) 
AND time(create_at)>'08:30:00'
GROUP BY stu_id HAVING c>=2;
```

查询哪个同学本周准时到校次数最多：

```sql
SELECT count(*) as c stu_id FROM stu 
WHERE date(created_at)>=date(date_add(now(),interval 0-WEEKDAY(now()) day)) 
AND time(create_at)<='08:30:00'
GROUP BY stu_id ORDER BY c desc limit 1;
```

查找本周哪天迟到的学生最多的数据：

```sql
SELECT * FROM stu 
WHERE date(created_at)>=date(date_add(now(),interval 0-WEEKDAY(now()) day)) 
AND time(create_at)>'08:30:00'
GROUP BY create_at ORDER BY count(*) desc limit 1;
```

查找哪个姓氏的学生最多：

```sql
SELECT left(sname, 1) as s from stu
GROUP BY s ORDER BY count(*) desc limit 1;
```

> 先对查询结果进行筛选，筛选完后进行分组，再按照每一组的人数进行排序，取最多的一个

查询超过2个同学的姓氏：

```sql
SELECT left(sname, 1) as s from stu
GROUP BY s HAVING count(*)>=2;
```



## 多表操作

### 多表关系

数据表的关系有：

- 一对一的关系

  学生表，学生详情资料表，一个学生对应详情表中的一条数据，有且只有一条，学生表中方的`id`与详情表中的`stu_id`对应做表语表的联系

  有一个主表，一个从表，详情数据太多的情况下，我们可以将表字段进行切分，移动到另一张表中（从表）

- 一对多的关系

  学生和班级的关系：一个学生对应一个班级，一个班级可以对应多个学生

  多的一方来记录少的一方，学生表中有个字段记录班级编号即可

- 多对多的关系

  学生和课程的关系：学生可以学多个课程，一个课程也可以被多个学生学习

  用中间表来做衔接，记录哪个编号的学生对应哪个课程，同一个编号的学生可以对应多个课程

***

### `Exists`的工作原理

`Exists`类似于过滤，对于满足条件的内容进行筛选，过滤掉我们不需要的内容，使用示例：

```sql
SELECT * FROM stu s WHERE EXISTS(SELECT * FROM stu WHERE s.id=1);
```

> `EXISTS()`里面放上我们的过滤条件，只要这个条件有结果，那这个表达式就为真，会拿查询到的每一条数据和这个表达式进行判断，符合条件的就留下，不符合条件的就过滤掉
>
> 上述语义具体描述为：对查询到的数据起一个别名，如果其id为1，就留下数据，否则就过滤掉数据
>
> `EXISTS()`的具体工作原理是将外层的结果依次传递给里面的`sql`语句

`Exists`有一个反函数：`Not Exists`，逻辑上与其相反，满足条件的被过滤掉，不满足条件的留下

`Exists`一般用于多表操作中会带来便利，如我们有两张表：一张学生信息表，一张学生所学课程表，该表的字段只有`stu_id`和`lesson_id`，我们查看哪些学生已经在学习课程了：

```sql
SELECT * FROM stu s WHERE EXISTS(SELECT * FROM lesson l WHERE s.id=l.stu_id);
```

> 返回的是学生表中有在学习课程的学生信息，如果一个学生同时学习两门课程，只会显示最后查询到的

查询学生学习课程数量大于等于2门的学生信息：

```sql
SELECT * FROM stu s WHERE EXISTS(
    SELECT id FROM lesson l WHERE s.id=l.stu_id GROUP BY id HAVING count(*)>=2
);
```

> 对课程表中的学生id进行分组，统计出学习课程数大于2的学生id
>

查询的数据是男同学，且其学生具体信息表中设置了QQ号：

```sql
SELECT * FROM stu s WHERE s.sex='男' AND EXISTS(
    SELECT * FROM stu_info sl WHERE s.id=sl.stu_id AND sl.qq IS NOT null
);
```

> `EXISTS()`的作用是过滤，是将上层查询的结果进行一个过滤，如果该数据在`EXISTS()`中有结果，就为真，这条数据就会被保留，否则就会被过滤掉

***

### 笛卡尔积

笛卡尔积是多表关联产生的一种情况，对于两张表进行关联的时候，如学生表与班级表进行关联，学生表中有10条数据，班级表有5条数据，在两表进行关联的时候没有指定条件：`SELECT * FROM stu,class;`

那`mysql`就会认为学生表中的一条记录和班级表中的所有记录都相匹配，就会产生50条记录

为了解决这个情况，我们需要加上条件，让学生表中的班级编号与班级表中的主键相匹配：

```sql
SELECT * FROM stu,class WHERE stu.class_id = class.id;
```

这样就将两张表中所有的字段组合在一起进行返回了，返回的结果是学生表中的10条数据，学生表的信息在前面，班级表的信息在后面，同时又包括对应班级表中的具体信息，根据学生表中的班级编号和班级表中的主键对应进行匹配

> 上述`sql`语句中，如果两张表中只有一张表中有`class_id`，那么条件前面的表前缀是可以去掉的，变为：
>
> ```sql
> SELECT * FROM stu,class WHERE class_id = class.id;
> ```
>
> 如果两张表都有这个字段，表前缀是不可以去掉的
>
> 一般情况下，都是要加表前缀的，防止冲突，如果表前缀比较长，我们一般是使用别名：
>
> ```sql
> SELECT * FROM stu as s,class as c WHERE s.class_id = c.id;
> ```

多表关联取数据的时候，通常是不会将各个表中的所有数据都取过来的，一般是取部分数据的

```sql
SELECT s.sname,c.cname FROM stu as s,class as c WHERE s.class_id = c.id;
```

> 上述表示取学生表中的学生名字和班级表中的班级名字

***

### 多表操作

#### 分组查询

对于上节中的学生表与班级表进行多表关联：

```sql
SELECT * FROM stu as s,class as c WHERE s.class_id = c.id;
```

一般推荐使用`INNER JOIN`和`ON`的方式进行多表关联，具体如下：

```sql
SELECT * FROM stu as s INNER JOIN class as c ON s.class_id = c.id;
```

> 这样的方式语义更加清晰，`INNER JOIN`后面的表示被关联的表，关联前后的表，`ON`后面表示条件

对于一对一的表进行关联，学生表和学生具体信息表：

```sql
SELECT * FROM stu as s INNER JOIN stu_info as si ON s.id = si.stu_id;
```

> 返回的结果数据条数是学生具体信息表的数据条数，对于没有学生具体信息的学生表中的学生，返回结果会将其过滤掉，只有完全匹配的会被拿出来

多个表关联后的结果，可以理解为其结果是一张表，对后续的查询和筛选操作可以当作对一张表进行操作，如查询班级为1班的学生：

```sql
SELECT * FROM stu as s INNER JOIN class as c ON s.class_id = c.id WHERE c.cname = '1班';
```

关联后的结果可以继续与其他表进行关联，一直关联下去，直到得到想要的任务结果：

```sql
SELECT c.cname,c.id,s.id,s.sname,a.title FROM stu as s INNER JOIN class as c
ON s,class_id = c.id
INNER JOIN article as a
ON s.id = a.stu_id;
```

也可以将`INNER JOIN`的内容连接起来写，`ON`的内容连接起来写：

```sql
SELECT c.cname,c.id,s.id,s.sname,a.title FROM stu as s 
INNER JOIN class as c
INNER JOIN article as a
ON s,class_id = c.id AND ON s.id = a.stu_id;
```

#### 分组筛选

分组筛选是在分组查询的基础上进行`WHERE`的条件筛选，将多表关联分组后，理解为一张表，再进行查询

筛选为男生写的文章：

```sql
SELECT a.title FROM stu as s 
INNER JOIN class as c
INNER JOIN article as a
ON s,class_id = c.id AND ON s.id = a.stu_id
WHERE s.sex = '男';
```

***

### 外链接在多表查询中的使用

对于一对一的表进行关联，学生表和学生具体信息表：

```sql
SELECT * FROM stu as s INNER JOIN stu_info as si ON s.id = si.stu_id;
```

> 返回的结果数据条数是学生具体信息表的数据条数，对于没有学生具体信息的学生表中的学生，返回结果会将其过滤掉，只有完全匹配的会被拿出来

对于学生具体信息没有设置的，其学生表中的学生基本信息也会被过滤掉，如果就需要找这些没有设计具体信息的学生，就需要使用到外链接，`LEFT JOIN`和`RIGHT JOIN`，左侧偏心（将左侧的表信息全部读取出来，无无论有没有匹配）和右侧偏心（将右侧的表信息全部读取出来，无论有没有匹配）

```sql
SELECT * FROM stu as s LEFT JOIN stu_info as si ON s.id = si.stu_id;
```

> 这样如果没有具体信息的学生，其基本内容也会返回出来

查找哪些同学没有在具体信息表中设置QQ：

```sql
SELECT * FROM stu as s LEFT JOIN stu_info as si ON s.id = si.stu_id WHERE si.qq is null;
```

右侧偏心关联和左侧偏心关联使用方式一样，会将右侧表的信息全部展示出来，即使没有被左侧的内容进行匹配

查找哪些班级没有学生：

```sql
SELECT c.cname FROM stu as s RIGHT JOIN class as c ON s.class_id = c.id 
WHERE s.id is null;
```

查找学生所在的班级，如果学生没有分配班级，就显示`null`：

```sql
SELECT s.sname,if(s.class_id,c.cname,'无') as cname FROM class as c 
RIGHT JOIN stu as s ON c.id = s.class_id;
```

***

### 自链接

自链接就是自己链接自己，具体使用为：与小金同学在一个班级的同学：

第一种方式是使用子查询：

```sql
SELECT * FROM stu WHERE class_id = (SELECT class_id FROM stu WHERE sname = '小金')
AND sname != '小金';
```

使用自链接的方法进行查询：自己关联自己，将一张表当作两张表进行关联使用

```sql
SELECT s2.sname FROM stu as s1 INNER JOIN stu as s2
ON s1.class_id = s2.class_id
WHERE s1.sname = '小金' AND s2.sname != '小金';
```

查找出生年份比小金大的同学：（出生日期越小，年龄越大）

```sql
SELECT s2.sname FROM stu as s1 INNER JOIN stu as s2
ON YEAR(s1.birthday) > YEAR(s2.birthday)
WHERE s1.sname = '小金';
```

***

### 多表操作中的多对多关系

对于学生表和课程表之间，就是一个典型的多对多关系，一个学生可以学习多个课程，一个课程也可以被多个学生学习，多对多关系是不能让一方剔除另一方的，对于多对多关系的表，需要使用中间表来进行记录关系

查找小金学习的所有课程：

```sql
SELECT s.sname,l.name FROM stu as s INNER JOIN stu_lesson as sl ON s.id = sl.stu_id
INNER JOIN lesson as l ON l.id = sl.lesson_id WHERE s.sname = '小金';
```

查找哪个班级的学生最喜欢`mysql`课程：

```sql
SELECT c.id, count(*) as total FROM class as c INNER JOIN stu as s ON c.id = s.class_id
INNER JOIN stu_lesson as sl ON s.id = sl,stu_id
INNER JOIN lesson as l ON sl.lesson_id = l.id
WHERE l.name = 'mysql'
GROUP BY c.id
ORDER BY total desc LIMIT 1;
```

多表操作简单来说就是首先需要确定使用哪几张表，将这几张表全部连接起来，再当作一种表进行筛选处理

***

### 多表查询多个查询结果的连接和合并

默认合并，如果有重复的是会过滤掉重复的，只显示一个重复的数据

```sql
SELECT * FROM stu UNION SELECT * FROM stu;
```

如果我们不想让重复的数据被过滤，需要在`UNION`后面加上`ALL`即可：

```sql
SELECT * FROM stu UNION ALL SELECT * FROM stu;
```

这样就会得到两次完整的查询结果

使用`UNION ALL`可以进行连接不同的表，但是连接的时候，字段的数量要相等：

```sql
SELECT sname FROM stu UNION ALL SELECT cname FROM class;
```

> 两张表都提供一个字段进行连接，是可以进行连接的，连接后，两张表的内容被放到了同一列，使用的字段名是第一条记录的字段名，也就是`sname`
>
> 但是如果对不同的表进行条件的添加，需要使用括号进行包裹：
>
> ```sql
> (SELECT sname FROM stu LIMIT 3) UNION ALL (SELECT cname FROM class LIMIT 3);
> ```

对连接完后的结果，我们可以将其当作一张新表，可以进行其他的筛选处理操作：

```sql
(SELECT sname FROM stu LIMIT 3) UNION ALL (SELECT cname FROM class LIMIT 3) LIMIT 2;
```

查询学生表中年龄最大的和最小的数据，进行合并：

```sql
(SELECT * FROM stu WHERE birthday is not null ORDER BY birthday ASC LIMIT 1)
UNION ALL
(SELECT * FROM stu WHERE birthday is not null ORDER BY birthday DESC LIMIT 1);
```

***

### 多表查询数据的删除

我们可以对多表查询的结果进行删除，删除操作是有破坏性的

删除没有学习任何课程的同学：

- 方法一：使用子查询的方式进行删除：

  ```sql
  DELECT FROM stu WHERE id in(
  SELECT * FROM(
  SELECT s.id FROM stu as s LEFT JOIN stu_lesson as sl 
  ON s.id = sl.stu_id
  WHERE sl.lesson_id is null) as s
  );
  ```

  > 在实际开发中，一般是先发一条`sql`语句，在发一条语句进行处理，而不是写成一条语句，一条语句的形式往往不够清晰，不通俗易懂

- 方法二：使用多表来进行删除操作：

  ```sql
  DELECT s FROM stu as s
  LEFT JOIN stu_lesson as sl ON s.id = sl.stu_id
  WHERE sl.lesson_id is null;
  ```

  > 明确告知要删除学生表s中的数据

删除操作也可以使用存储过程来进行删除



## 正则表达式在`Mysql`中的使用

正则表达式是一个数组语言，在`Mysql`中使用可以方便我们进行简单而快速的操作，有了正则表达式，我们可以匹配到任何我们想要的内容和结果

- 查看班级表单名称字段中第二个字母是`h`的记录：

  `select * from class where cname REGEXP '^.h';`

- 查看班级表中的介绍字段中包括`php`和`mysql`内容的记录：

  `select * from class where description REGEXP 'php|mysql';`



## 事务处理

事务可以理解为`mysql`所做的事情，通过`sql`语句进行控制

有的简单事务通过一条`sql`语句就可以完成，该事务要么成功要么失败，但是有的复杂的事务，需要多条`sql`语句才能完成，如回复评论引发的其他内容的变化，这个是一条链的执行过程，如果在某个环节出现了问题，这个链就是缺损的，是不完整的。我们需要考虑，如果某个环节出现了问题，我们是否需要让这条链全部撤销掉重做？对于不同的情况，有不同的考虑方式，如回复评论引发的后续其他内容的变化，是不需要撤销重做的，我们可以不用保证每一块业务的完整性，因为后续更新的时候会保证修正操作，也就是说，某个环节的异常不会对整体产生严重影响，我们可以不需要进行撤销重做，可以进行宽松对待；对于要求比较严格的，如商城系统、库存系统等实时交易，我们必须要求该事务是一个整体，要么全部成功，要么全部失败。

### 事务存储引擎的选择

事务的方式是受存储引擎影响的

我们可以通过`SHOW engines;`来查看`mysql`支持的存储引擎，其中`innoDB`是默认的存储引擎，是支持事务处理的，如果有一些老的数据表，使用的是其他的存储引擎，我们可以进行存储引擎的修改：

```sql
ALTER TABLE stu engine = 'InnoDB';
```

> 也可以使用图形化界面进行修改

***

### 事务单独开启

开启事务将后续的`sql`语句归类为一组进行执行，要么全部成功，要么全部失败，不会出现某条`sql`语句执行成功，具体的开启事务的方式为：

```sql
BEGIN;
INSERT INTO class(canme)VALUES('研究生');
COMMIT;
ROLLBACK;
```

> 执行`BEGIN;`语句后，后面输入执行的`sql`语句就是一组
>
> 在一个用户在执行事务的时候，成功执行了一条语句后，当前`mysql`用户是可以看到具体结果的，但是另一个用户是查询不到该执行结果的，因为`mysql`认为第一个用户的事务还没有完成，可能还有其他的`sql`语句，如果想要结束，需要使用`COMMIT;`进行事务的提交，提交完后，这条数据才会真正的写入硬盘的数据表当中，其他用户就可以查询到事务中新增的数据了。
>
> 简而言之，`BEGIN;`语句将多条事务连接成一组了；`COMMIT;`语句将多条语句进行统一的执行了
>
> 除了`BEGIN;`，还有其他的开启事务的语法：`START TRANSACTION;`
>
> 如果在执行`sql`语句的时候，有一条语句出现了问题，我们可以进行回滚操作`ROLLBACK;`，回滚可以理解为放弃上一条执行的`sql`语句（撤销上一条`sql`语句的执行），如果出现异常，我们可以使用`ROLLBACK;`，让其进行回滚，但是要注意，当执行`ROLLBACK;`后，和`COMMIT;`一样，这个事务就走完了，后续还是回到默认的方式，即执行一条`sql`语句，生效一次

***

### 全局的事务开启

每执行一个事务，我们都需要进行开启事务，如果要求全部的`sql`语句必须要全部使用事务的方式，我们可以开启全局事务，不用每一次提交后，再重新开启事务，全局事务开启的设置：

```sql
SET autocommit = 0;
INSERT INTO class(canme)VALUES('研究生');
COMMIT;
```

> 表示执行`sql`语句的时候，系统不会自动的进行提交，必须进行`COMMIT;`操作才可以进行提交，将真正的数据写入到硬盘上，其他用户才能看到，否则，只是当前执行`sql`语句的用户能看到

***

### 事务隔离级别与脏读

在高并发的状态下，可能会有多个事务在同时操作一张数据表，这个时候可能就会出现脏读和幻读等问题

- 脏读：事务A读取了事务B更新的数据，然后B进行了回滚操作，那么A读取到的数据是脏数据
- 不可重复读：事务A多次读取同一数据，事务B在事务A多次读取的过程中，对数据作了更新并提交，导致事务A多次读取同一数据时（这个时候事务A还没有进行提交，按理说是看不到事务B提交的内容的），结果不一致
- 幻读：系统管理员A将数据库中所有学生的成绩从具体分数改为ABCDE等级，但是系统管理员B就在这个时候插入了一条具体分数的记录，当系统管理员A结束后发现还有一条新的记录也被改过来了，就好像发生了幻觉一样

我们希望每个事务之间是独立的，不同的事务不会互相干扰，这就需要引入事务隔离操作，事务隔离有不同的隔离级别，但是，隔离级别越好，会导致性能变差，因此设置隔离级别的时候也要兼顾性能

|          事务隔离级别          | 脏读 | 不可重复读 | 幻读 |                             说明                             |
| :----------------------------: | :--: | :--------: | :--: | :----------------------------------------------------------: |
| 读未提交（`read-uncommitted`） |  是  |     是     |  是  | 最低的事务隔离级别，一个事务还没提交时，它做的变更就能被别的事务看到 |
| 不可重复读（`read-committed`） |  否  |     是     |  是  | 保证一个事物提交后才能被另外一个事务读取。另外一个事务不能读取该事物未提交的数据 |
| 可重复读（`repeatable-read`）  |  否  |     否     |  是  | 多次读取同一范围的数据会返回第一次查询的快照，即使其他事务对该数据做了更新修改。事务在执行期间看到的数据前后必须是一致的 |
|    串行化（`serializable`）    |  否  |     否     |  否  | 事务 100% 隔离，可避免脏读、不可重复读、幻读的发生。花费最高代价但最可靠的事务隔离级别 |

查询当前使用的事务隔离级别：`select @@global.transaction_isolation,@@transaction_isolation;`

默认情况下，`mysql`使用的事务隔离级别是`repeatable-read`

我们可以进行事务隔离级别的设置：`set session transaction isolation level read uncommitted;`

> 设置事务隔离级别的时候，将级别名称的-去掉进行设置

使用串行化来阻止幻读的现象，会使用锁机制，将中间开启的事务进行锁定，当之前的事务结束时，中间开启的事务才会继续的自动往下执行，这样就防止出现幻读



## 锁机制

`Mysql`是支持多线程的，在同一时刻可以处理多个用户的请求，如果多个用户都在修改同一个数据，就会造成数据的不稳定性，锁机制的出现是为了保证数据的稳定性，如我们在同一时刻进行买书的操作，我们要锁住记录，在扣除余额的时候是一个一个进行的，而不要受并发多线程的影响来同时进行余额的扣除

### 事务处理中的锁机制

#### 行级锁

使用锁机制，我们可以将整张表进行锁定，即表级锁，也可以使用行级锁定的方式，将表中的某条数据记录进行锁定，行级锁在应付高并发的时候响应比较快，其存储性能是比较高的，在数据引擎中`InnoDB`是支持行级锁的

如果两个事务都执行一个事务，都对数据表中的同一条数据的同一个字段内容进行修改，先执行的事务可以正常修改，但是后执行的事务在执行`sql`语句的时候就会阻塞，一直到先执行的事务提交后，阻塞才会重新恢复，并执行下去。如果在锁机制阻塞的时候，我们将其终止，更新其他数据的内容，是可以更新成功的。

上述过程是一个行级锁的体现，没有锁定整张表，只是锁定了表中的某行数据

#### 索引

对于没有设置索引的字段，即使修改的不是同一条数据，但是根据这个字段取查找的：

```sql
UPDATE stu SET sname = '小金' WHERE sname = 'xiaojin';
```

其中`sname`字段没有索引数据，不同事务通过该字段进行索引从而修改内容，会导致表的记录被大面积的锁定

一般情况下，主键是默认添加了索引的，其他字段是没有添加索引的，如果使用的列不是索引列，就会将整个表的索引记录进行锁定，这样就丢失了`InnoDB`行级锁的魅力

对于查询量多的字段，我们最好可以将其设置成索引，这样就不会造成大面积的锁定

#### 查询范围对锁的影响

在查询范围内的所有记录都会被锁定，但是在范围外的记录是不会被锁定的

对于事务A，对主键id大于1和小于5之间的`num`字段内容进行修改：

```sql
SET autocommit = 0;   # 开启全局事务
UPDATE goods SET num=500 WHERE id>1 AND id<5;
```

对于事务B，在查询范围id大于1且小于5的内容进行修改，会被锁定；对范围之外的内容进行修改不会被锁定：

```sql
SET autocommit = 0;
UPDATE goods SET num=200 WHERE id=3;    # 发生阻塞
UPDATE goods SET num=200 WHERE id=5;    # 不发生阻塞
```

只有当事务A提交后，那事务B就可以该怎么操作怎么操作了，不会受到影响了

我们是希望锁的查询范围是越小越好，这样使多线程的`Mysql`应用在处理高并发的时候，性能可以更好

***

### 悲观锁

在`Mysql`中总是感觉这个数据在更新的时候，别人也在更新，我们可以使用悲观锁，在查询的时候就进行锁定

当有一个事务在查询的时候，其他事务的查询操作也被锁定了，这个情况就是悲观锁（当前一个事务在查询的时候，后面的事务就不要查询了，将查询操作阻塞了）

事务A：

```sql
SET autocommit = 0;
SELECT * FROM goods WHERE id=1 FOR UPDATE;   # 对id为1的商品进行查询，执行悲观锁
```

事务B：

```sql
SET autocommit = 0;
SELECT * FROM goods WHERE id=1 FOR UPDATE; # 也对id为1进行查询，由于事务A设置了悲观锁，当前会阻塞
```

> 当事务A执行提交或回滚操作后，即结束事务，事务A剩余执行的`sql`语句：
> ```sql
> UPDATE goods SET num=0 WHERE id=1;   # 将id为1的商品全部买完
> COMMIT;
> ```
>
> 事务B的阻塞才会终止，返回查询的结果，返回的结果：id为1的商品数量为0

***

### 乐观锁

`Mysql`中有悲观锁，对应的也有乐观锁，更新数据的时候很乐观，不认为别人也会更新数据，如果出现了问题，等出现了问题再说，一般通过其他字段进行限制，常用的是版本号字段

事务A：

```sql
SET autocommit = 0;
SELECT * FROM goods WHERE id=1;   # 对id为1的商品进行查询
# 买100件商品，同时让版本号加一
UPDATE goods SET num=num-100,version=version+1 WHERE version=0 AND id=1;  
COMMIT;
```

事务B：

```sql
SET autocommit = 0;
# 版本号已经发生改变，是索引不到这个商品的，因此也买不到这个商品
UPDATE goods SET num=num-100,version=version+1 WHERE version=0 AND id=1;  
```

通过这样的乐观锁，保持了数据的稳定性，从而不会出现数据的错乱

***

### 表锁的技巧

对于一些不能支持事务的引擎，我们可以使用锁表的机制来保证数据的稳定

#### 读锁

对表进行锁定，使该表只能进行读取，不能进行其他操作，包括当前用户：

```sql
LOCK TABLE goods READ;
SELECT * FROM goods;   # 可以读取，当前会话和其他会话都可以读取
INSERT INTO goods (name,num)VALUES('手机',200);   # 不能执行，当前会话报错，其他会话发生阻塞
UNLOCK TABLES;  # 执行解锁操作，其他会话阻塞结束，执行了之前的操作
```

#### 写锁

写锁可以使当前会话可以操作这张表，其他会话什么都不能执行，阻塞，只有当前会话解锁后，其他会话才能使用

```sql
LOCK TABLE goods WRITE;
SELECT * FROM goods;
UNLOCK TABLES;
```

> 锁表不但但只能锁一个表，可以一起锁定很多表：`LOCK TABLE goods WRITE, stu WRITE;`

这样写锁的方式会造成对整个表的锁定，对高并发的吞吐处理能力是非常有限的，一般不太建议使用写锁的方法



## 外键约束

外键约束是`Mysql`提供的一个特性，`Mysql`作为一个关系型的数据库，表之间都是有关系的

`	Mysql`的默认引擎`InnoDB`支持外键关联/约束

对于几个表之间的关联，表之间会受影响，一张表发生变化，会影响另一张表（如班级表中的某个班级被删除了，对应学生表中对应这个被删除班级的学生是应该删除呢？还是这些学生的班级为`null`呢？这些变化都是关联的变化），这就需要各个表中的某一个字段来建立联系，对于不同表的这个字段，其字段类型要保持一致；一般情况下，与主键（主表的关联键）或者外键进行关联，关联约束的字段内部有一个检索的过程，所以这些关联字段都应该设置索引属性，我们可以不用特意的去设置，在做表关联的时候，`Mysql`会自动的给我们设置这些关联字段的索引属性

### 表中定义外键约束

常用的外键约束关键词：

|     选项      |         说明         |
| :-----------: | :------------------: |
| `CONSTRAINT`  |  为外键约束定义名称  |
| `FOREIGN KEY` |  子表与父表关联的列  |
| `REFERENCES`  |  子表关联的父表字段  |
|  `ON DELETE`  | 父表删除时的处理方式 |
|  `ON UPDATE`  | 父表更新时的处理方式 |

#### 从新建表开始

新建一张表，并添加外键约束：

```sql
CREATE TABLE stu2(
	id int PRIMARY KEY AUTO_INCREMENT,  # 设置id为主键，且自增
    sname char(30) NOT NULL,
    class_id int DEFAULT NULL,
    # 声明外键和定义外键
    CONSTRAINT stu2_class
    FOREIGN KEY (class_id)
    REFERENCES class(id)
    # 定义主表发生变化时，子表的动作
    # 当班级某条数据执行删除的时候，对应班级的学生数据跟着删除
    ON DELETE CASCADE
)ENGINE=InnoDB DEFAULT CHARSET=utf8;   # 定义引擎和字符集
```

新键的学生表不单单只与班级表进行关联，还可以与其他的表进行关联，这样我们就要定义多个外键，为了区分，我们一般是给外键取一个别名，这个别名一定是要唯一的，一般情况下以当前表的名字-关联表的名字即可

在哪个表中定义外键，哪个表就是子表，外键最好是要指向另一张表的主键，可以不指向主键，但是指向的这个字段必须有索引属性

#### 基于修改表

对于存在的表，如果后续我们想要添加外键约束，我们可以进行以下操作：

```sql
ALTER TABLE stu ADD
# 声明外键和定义外键
CONSTRAINT stu_class
FOREIGN KEY (class_id)
REFERENCES class(id)
# 定义主表发生变化时，子表的动作
# 当班级某条数据执行删除的时候，对应班级的学生数据跟着删除
ON DELETE CASCADE;
```

***

### 删除外键约束

删除学生表`stu`中的外键约束：

```sql
ALTER TABLE stu DROP FOREIGN KEY stu_class;
```

***

### 外键约束的关联动作

#### `ON DELETE`

`ON DELETE`是指在删除时的处理方式，常用的处理方式有：

|         选项          |                             说明                             |
| :-------------------: | :----------------------------------------------------------: |
|  `ON DELETE CASCADE`  | 删除父表记录时，子表中关联这条父表记录的所有子表记录会同时删除 |
| `ON DELETE SET NULL`  | 删除父表记录时，子表记录对应的外键变为为 NULL（子表字段要允许 NULL） |
| `ON DELETE NO ACTION` | 不能直接删除父表记录，必须把子表外键处理（删除或修改）完才可以删除主表中对应的内容，具体来说，就是子表有数据，主表不能动 |
| `ON DELETE RESTRICT`  |                 与`ON DELETE NO ACTION`一致                  |

#### `ON UPDATE`

`ON UPDATE`是指在更新时的处理方式，常用的处理方式有：

|         选项          |                             说明                             |
| :-------------------: | :----------------------------------------------------------: |
|  `ON UPDATE CASCADE`  | 更新父表记录时，比如更改主表的主键时，子表对应的外键内容同时更新 |
| `ON UPDATE SET NULL`  | 更新父表记录时，比如更改主表的主键时，子表对应的外键被设置为 NULL |
| `ON UPDATE NO ACTION` | 不能直接更新父表记录，必须把子表外键处理（删除或修改）完才可以更新主表 |
| `ON UPDATE RESTRICT`  |                 与`ON UPDATE NO ACTION`一致                  |

父表更新时，子表对应的更新无非就是更新对应的外键内容，如更改主键的编号，改为6，那么对应匹配外键的编号也会变为6，这个就是`ON UPDATE CASCADE`更新的过程，关联受影响

更新的应用场景相较于删除是很低的



## 索引优化

### 性能优化思路

- 选择合理范围内最小的

  选择最小的数据范围，因为这样可以大大减少磁盘空间及磁盘 I/0 读写开销，减少内存占用，减少 CPU 的占用率

- 选择相对简单的数据类型

  数字类型相对字符串类型要简单的多，尤其是在比较运算中，应该选择最简单的数据类型，比如说在保存时间时，我们可以将日期存为`int(10)`，这样会更方便、合适、快速的多

#### 字符串类型的优化思路

字符串数据类型是一个万能数据类型，可以储存数值、字符串等

- 保存数值类型最好不要用字符串数据类型，这样存储的空间显然是会更大，同时，如果进行运算时` mysql `会将字符串转换为数值类型，这种转换是不会走索引的
- 如果明确数据在一个完整的集合中如男，女，可以使用 `set` 或 `enum `数据类型，这种数据类型在运算及储存时以数值方式操作，所以效率要比字符串更好，同时空间占用更少

#### 数值类型的优化思路

##### 整数

整数类型很多比如 `tinyint`、`int`、`smallint`、`bigint` 等，我们要根据自己需要存储的数据长度决定使用的类型，同时 `tinyint(10)`与` tinyint(100)`在储存与计算上并无任何差别，区别只是在显示层面上，但是我们也要选择适合合适的数据类型长度。可以通过指定 `zerofill `属性查看显示时区别

##### 浮点数与精度数值

浮点数`float` 与` double `在储存空间及运行效率上要优于精度数值类型 `decimal`，但 `float `与 `double`会有舍入错误，而 `decimal` 则可以提供更加准确的小数级精确运算不会有错误产生计算更精确，更适用于金融类型数据的存储

***

### `EXPLAIN`

`EXPLAIN` 指令可以帮助开发人员分析` SQL` 问题，`explain` 显示了 `mysql `如何使用索引来处理 `select`语句以及连接表，可以帮助选择更好的索引和写出更优化的查询语句

通过`EXPLAIN`进行查询，返回的查询结果字段介绍（从左到右依次进行介绍）：

|      字段       |               说明               |
| :-------------: | :------------------------------: |
|      `id`       |           索引执行顺序           |
|  `select_type`  |             查询类型             |
|     `table`     |              操作表              |
|     `type`      |             使用类型             |
| `possible_keys` | 可能用到的索引，不一定被真正使用 |
|      `key`      |          最终使用的索引          |
|    `key_len`    |            索引字节数            |
|      `ref`      |          列与索引的比较          |
|     `rows`      |        预计读出的记录条数        |
|     `Extra`     |             查询说明             |

> 对于使用类型`type`，有下面常见的值：
>
> - `const`：使用主键值比较，匹配唯一行检索，速度快
>
>   ```sql
>   explain select * from stu where id = 3;
>   ```
>
> - `ref`： 前面表中的非唯一数据 
>
> - `eq_ref`：前面表中非唯一数据，使用了唯一索引字段，如表关联时使用主键
>
> - `range`：索引区间获得，如使用` IN(1,2,3)`筛选
>
>   ```sql
>   explain select * from stu where class_id in(1,2,3);
>   ```
>
> - `all`：全表遍历
>
>   ```sql
>   explain select * from stu where birthday = '20000213';
>   ```
>
> - `index`：与 `all` 类似只是扫描所有表，而非数据表
>
>   ```sql
>   explain select * from stu order by id;
>   ```

***

### 基础索引

合适的索引优化可以大幅度的提高`Mysql`的检索速度，索引就像书中的目录一样让我们更快的寻找到自己想要的数据，但是我们也不能过多的创建目录页（索引），原因是如果某一篇文章删除或修改将发变所有页码的顺序，就需要重新创建目录

索引优化是针对于海量数据的数据库，优化其数据的查找速度，增强其性能，减少开销

索引的弊端：

- 创建索引会使查询操作变得更加快速，但是会降低增加、删除、更新操作的速度，因为执行这些操作的同时会对索引文件进行重新排序或更新
- 创建过多列的索引会大大增加磁盘空间开销
- 不要盲目的创建索引，只为查询操作频繁的列创建索引

#### 索引类型

|          索引          |                 说明                 |
| :--------------------: | :----------------------------------: |
|   `UNIQUE` 唯一索引    |  不可以出现相同的值，可以有 NULL 值  |
|    `INDEX` 普通索引    |        允许出现相同的索引内容        |
| `PRIMARY KEY` 主键索引 | 不允许出现相同的值，且不能为 NULL 值 |

#### 索引维护

- 设置索引：`ALTER TABLE stu ADD INDEX sname_index(sname)`

  为 `stu` 学生表中的 `sname` 字段设置索引，`sname_index`为设置索引的别名

- 删除索引：`ALTER TABLE stu DROP INDEX sname_index`

  > 删除主键索引，首先需要移除` auto_increment `（自增属性）然后删除主键索引：
  >
  > ```sql
  > ALTER TABLE stu MODIFY id int;
  > ALTER TABLE stu DROP PRIMARY KEY
  > ```

- 查看索引：`show index from stu;`

***

### 性能分析

索引是加快查询操作的重要手段，如果当发生查询过慢时，为这个字段添加上索引后，会发现速度大大改观

- 对于没有添加索引属性的字段进行查找，会执行全盘扫描，性能是最差的

  ```sql
  EXPLAIN SELECT * FROM stu WHERE class_id=5 LIMIT 1;
  ```

  > 查找`stu`数据表中，`class_id`等于5的第一条数据
  >
  > 结果显示` type=ALL` 看出执行了全表扫描，`rows`数量较多

- 如果为`class_id`字段添加索引，后再执行查找：

  ```sql
  ALTER TABLE stu ADD INDEX class_id(class_id);
  EXPLAIN SELECT * FROM stu WHERE class_id=5 LIMIT 1;
  ```

  > 从`type `字段看出已经走了索引，本次查询遍历了少量的记录，`rows`数量较小

对于多表操作，连接操作多个表时，如果没有添加索引性能会非常差

- 对于没有使用索引的情况：

  ```sql
  explain select * from a join b on a.id=b.id join c on b.id=c.id;
  ```

  > 从结果中会看到每张表都遍历了所有记录，其`type`值都为`ALL`，遍历大量数据，性能较差

- 为其都添加索引后的情况：

  ```sql
  ALTER TABLE a ADD INDEX id(id);
  ALTER TABLE b ADD INDEX id(id);
  ALTER TABLE c ADD INDEX id(id);
  explain select * from a join b on a.id=b.id join c on b.id=c.id;
  ```

  > 执行的结果会看到使用了索引，并且并没有进行全表遍历

***

### 前缀和组合索引

#### 前缀索引

` text`/`长varchar `字段创建索引时，会造成索引列长度过长，从而生成过大的索引文件影响检索性能。使用前缀索引方式进行索引，可以有效解决这个问题。前缀索引应该控制在一个合适的点，控制在前百分之30即可

下面是取前缀索引的计算公式，有时也根据字段保存内容确定，比如标题 100 可以取 30 个字符为前缀索引：

```sql
select count(distinct(left(title,10)))/count(*) from news
```

为文章表 `article` 的` title` 字段添加 30 个长度的前缀索引：

```sql
ALTER TABLE article ADD INDEX title(title(30));
```

#### 组合索引

组合索引是为多个字段统一设计索引

- 可以较为每个字段设置索引文件体积更小
- 使用速度优于多个索引操作
- 前面字段没出现，只出现后面字段时不走索引

为学生表中的班级字段` class_id `与学生状态` status `设置组合索引：

```sql
Alter table stu add index class_id_status(class_id,status);
```

> 使用 `class_id `时会走索引，因为 `class_id` 在组合索引最前面：
>
> ```sql
> explain select * from stu where class_id=3;
> ```
>
> 查询结果`type`为`ref`，表明走了索引，走的索引`key`为`class_id`
>
> 只使用` status `字段不会走索引：
>
> ```sql
> explain select * from stu where status=1;
> ```
>
> 查询结果`type`为`ALL`，表明进行了全盘查找
>
> 当 `class_id `与` status `字段一起使用时会走索引：
>
> ```sql
> explain select * from stu where status=1 and class_id=5;
> ```
>
> 查询结果`type`为`ref`，表明走了索引，走的索引`key`为`class_id_status`

***

### 字段选择

#### 维度思考

- 维度的最大值是数据列中不重复值出现的个数

  > 如数据表中存在8行数据 `a,b,c,d,a,b,c,d `，则这个表的维度为 4

#### 索引规则

- 对` where`，`on` 或 `group by` 及 `order by `中出现的列（字段）使用索引
- 对较小的数据列使用索引，这样会使索引文件更小，同时内存中也可以装载更多的索引键
- 为较长的字符串使用前缀索引
- 要为维度高的列创建索引
- 性别这样的列不适合创建索引，因为维度过低
- 不要过多创建索引，除了增加额外的磁盘空间外，对于` DML` 操作的速度影响很大

***

### 查询优化

- 解析器

  `Mysql `的解析器非常智能，会对发出的每条` SQL `进行分析，决定是否使用索引或是否进行全表扫描

- 表达式影响

  索引列参与了计算的` SQL` 语句不会使用索引：

  ```sql
  explain select * from stu where status+1=1;
  ```

  > `type`为`ALL`

  索引列使用了函数运算的` SQL` 语句不会使用索引：

  ```sql
  explain select * from stu where left(sname,1)='金';
  ```

  > `type`为`ALL`

  索引列使用模糊匹配的` SQL` 语句不会使用索引：

  ```sql
  explain select * from stu where sname like '%1%';
  ```

  > `type`为`ALL`

  索引列使用正则表达式的` SQL` 语句不会使用索引：

  ```sql
  explain select * from stu where sname regexp '^1';
  ```

  > `type`为`ALL`

- 类型比较

  相同类型比较时走索引：

  ```sql
  explain select * from stu where sname="xiaojin";
  ```

  > `sname`的类型为字符串类型，与之比较的内容类型也是字符串类型，因此走的索引

  字符串类型与数值比较时不走索引：

  ```sql
  explain select * from stu where sname=1;
  ```

  > `type`为`ALL`

- 排序

  排序中尽量使用添加索引的列进行

***

### 慢查询

当 `Mysql` 性能下降时，通过开启慢查询来了解哪条` SQL `语句造成响应过慢，后续可以进行分析处理。当然开启慢查询会带来` CPU` 损耗与日志记录的` IO` 开销，所以我们要间断性的打开慢查询日志来查看 `Mysql`运行状态

慢查询能记录下所有执行时间超过设定 `long_query_time `时间的` SQL `语句, 用于找到执行慢的` SQL`语句, 方便我们对这些 `SQL`语句进行优化

#### 运行配置

##### 会话配置

通过以下指令开启全局慢查询（重起` Mysql `后需要重新执行）

```txt
set global slow_query_log='ON';
```

设置慢查询时间为 1 妙，即超过 1 秒将会被记录到慢查询日志

```txt
set session long_query_time=1;
```

##### 全局配置

通过修改配置 `mysql `配置文件` my.cnf` 来开启全局慢查询配置，在配置文件中修改以下内容

```txt
slow_query_log = ON
slow_query_log_file = /usr/local/mysql/data/slow.log
long_query_time = 1
```

重起 MYSQL 服务

```txt
service mysqld restart
```

#### 状态查看

查看开启慢查询状态：`show variables like 'slow_query%';`

查看慢查询的设置时间：`show variables like "long_query_time";`



## 问题记录

- 2024/11/28

  问题详情：忘记`mysql`中安装时，自定义的`root`密码

  问题解读：这种情况我们需要对`mysql`进行重置密码操作

  问题解决：针对于`MySQL Server 8.0`这个版本

  1. 以管理员的权限打开`cmd`，关闭`mysql`服务：`net stop MySQL80`

     可以在任务管理器的服务和应用程序中进行查看，是否关闭`mysql`服务（也可以在服务里面手动关闭）

  2. 在以管理员权限启动的`cmd`中输入：`mysqld --console --skip-grant-tables --shared-memory`   

     当出现光标停留在最后一行闪动，则运行成功

     这一步可能会出现两个问题：

     - 报错：`'mysqld' `不是内部或外部命令，也不是可运行的程序

       这个问题可以到`mysql`安装目录的`bin`目录下进行运行，也可以将`mysqld`添加到环境变量中

     - 如果进程直接结束了，且光标没有停留在最后闪动

       一般是因为以前安装`MySQL`的时候自定义了安装路径，导致安装目录下没有`Data`文件夹

       我们需要将`C:\ProgramData\MySQL\MySQL Server 8.0`路径下的`Data`文件夹剪切到`Mysql`的安装目录中（与`bin`文件同级）（记得后续修改完密码，重新启动前，将其剪切回去）

  3. 再次以管理员身份打开一个新的`cmd`窗口（第一个`cmd`窗口无法进行操作）

     1. 输入`mysql -u root -p`

     2. 不用输入密码，直接敲回车进入`mysql`命令行

     3. 输入`use mysql;`

     4. 输入`flush privileges;`

     5. 输入`alter user root@localhost identified by 'password';`

        其中，`password` 换成自己想要的密码

     6. 输入`exit`，退出`mysql`

  4. 将`Data`文件剪切回去

  5. 打开`MySQL`服务，输入`net start MySQL80`

  6. 输入`mysql -u root -p`，回车后输入修改后的密码即可登录