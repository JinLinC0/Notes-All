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

`SELECT DISTINCT class_id,name FROM stu WHERE class_id is not null;`表示将两个字段（`class_id`和`name`）的内容放到一起进行过滤去重，同一个班级，同一个姓名的内容将会被去重

#### 分组统计





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