# `Mysql`

## 基本概念

`Mysql`是一种开放源代码的关系型数据库管理系统（`RDBMS`），使用最常用的数据库管理语言–-结构化查询语言（`SQL`）进行数据库管理

数据库分为三级关系：数据库服务器、数据库、数据表

数据库与计算机进行对比，数据库就类似一个文件夹，我们在文件夹中要存储文件，这些文件就类似于我们后续要操作的数据表

- `Mysql`的基本语句和`PHP`一样，在该行语句结束的时候，需要以分号进行结尾，不然这条语句是不执行的
- `\c`放弃当前语句执行：当我们有时候输入指令错误时，我们想要放弃这条指令的执行，我们只需要在这条指令后面输入`\c`，系统就会放弃这条指令的执行
- 退出命令行`Mysql`，我们只需要在`Mysql`命令行执行`exit`即可

当我们使用数据库软件的时候，比如`Navicat`时，我们想要通过命令行进行数据库的操作，我们可以点击左上角的新建查询按钮，选择相应的数据库连接和数据库，输入相关代码（在一行语句结束的时候一定要以分号进行结尾），点击运行即可，同时我们要知道`SQL` 的关键字和函数名不区分大小写；`SQL`语句注释是在语句前面加`--`



## 连接数据库服务器

`mysql -uroot -p`

> 输入方式也可以写成这样的形式：`mysql -u root -p`
>
> - -u表示是我们的账号，这个账号是`Mysql`软件的账号（不是当前登录系统的账号）
>
> - -p表示要输入我们的密码
>
> 回车，然后输入密码，输入密码后就会进入`MySQL`的操作管理界
>
> 该语句还有两个需要了解的参数
>
> - -P表示我们可以指定连接的端口，`Mysql`的默认端口是3306，不添加的时候，默认使用3306这个端口
> - -h表示连接到数据库的`ip`地址，默认情况下是连接到我们的本地主机（127.0.0.1）



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



<<<<<<< HEAD
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
=======
## 数据库中的数据类型
>>>>>>> b0417762bb1843eb6cad7a5a0b67f9d53c674ce9

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



## 正则表达式在`Mysql`中的使用

正则表达式是一个数组语言，在`Mysql`中使用可以方便我们进行简单而快速的操作，有了正则表达式，我们可以匹配到任何我们想要的内容和结果

- 查看班级表单名称字段中第二个字母是`h`的记录：

  `select * from class where cname REGEXP '^.h';`

- 查看班级表中的介绍字段中包括`php`和`mysql`内容的记录：

  `select * from class where description REGEXP 'php|mysql';`



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