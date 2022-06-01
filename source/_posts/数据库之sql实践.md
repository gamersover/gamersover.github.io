---
title: 数据库之sql实践
date: 2021-04-16 23:23:36
tags:
    - 大数据
    - sql
categories: 技术
---

<!--
    view视图的概念？
    开始声明多种sql数据库的存在，然后这里使用hivesql2.x版本作为演示等等等
    1. 可首先用脑图作为辅助介绍数据库->表->字段->数据的概念
    2. 接着介绍sql关键词的书写顺序和执行顺序
    3. 再介绍一些实践操作和join等等操作
-->

# 1. 概述
数据库，顾名思义用来存放数据的地方，可以把数据库比作图书馆，图书馆里面有不同区域相当于不同的数据库（database），而每个区域又存放着不同的书籍，相当于每个database含有很多表（table），图中表现了这种关系。
<!--more-->

<img src="https://raw.githubusercontent.com/gamersover/hexo_blog_assets/main/%E6%95%B0%E6%8D%AE%E5%BA%93/No1.jpg" width="25%">

而表中有包含数据，数据是由记录（行）和字段（列）构成的，每条记录的字段的数据就是存储的数据。表即普通的表格，下表中有两条记录（两行），三个字段（三列）为`id`，`name`，`age`。

|id|name|age|
|:-:|:-:|:-:|
|0|superman|32|
|1|wonderwoman|102|

对于数据库，表，记录，字段都有增、删、改、查的操作，而sql（Structured Query Language）即结构化查询语言就是用来完成这些操作的，对于不同的对象有着不同的sql语句。大致包含对数据库的操作，对表的操作，对字段的操作，对记录的操作。在执行单条sql语句时可以不以`;`结束，但是执行多条语句时需要以`;`结束。而且sql语句大小写不敏感，即不区分大小写。

# 2. 数据库操作sql
## 2.1 查
* **show databases**：查询当前所有数据库

  ```shell
  mysql> SHOW DATABASES;
  +--------------------+
  | Database           |
  +--------------------+
  | information_schema |
  | mysql              |
  | performance_schema |
  | sys                |
  +--------------------+
  4 rows in set (0.00 sec)
  ```

* **use 数据库名**：连接到指定的数据库
  ```shell
  mysql> use mysql;
  Database changed
  ```

* **show tables**：查看该数据库中的所有表
  ```shell
  mysql> show tables;
  +------------------------------------------------------+
  | Tables_in_mysql                                      |
  +------------------------------------------------------+
  | columns_priv                                         |
  | component                                            |
  | db                                                   |
  | default_roles                                        |
  ...
  | engine_cost                                          |
  | func                                                 |
  | time_zone_name                                       |
  +------------------------------------------------------+
  35 rows in set (0.00 sec)
  ```

## 2.2 增
* **create database 数据库名**：创建数据库
  ```shell
  mysql> create database mydatabase;
  Query OK, 1 row affected (1.93 sec)

  mysql> show databases;
  +--------------------+
  | Database           |
  +--------------------+
  | information_schema |
  | mydatabase         |
  | mysql              |
  | performance_schema |
  | sys                |
  +--------------------+
  5 rows in set (0.00 sec)
  ```

## 2.3 删
* **drop database 数据库名**：删除数据库
  ```shell
  mysql> drop database mydatabase;
  Query OK, 0 rows affected (2.01 sec)

  mysql> show databases;
  +--------------------+
  | Database           |
  +--------------------+
  | information_schema |
  | mysql              |
  | performance_schema |
  | sys                |
  +--------------------+
  4 rows in set (0.00 sec)
  ```

## 2.4 改
数据库名称没有直接修改命令，只能重新创建新库然后导入旧库

# 3. 表操作sql
## 3.1 增
* **create table**： 创建表
语法：
  ```
  CREATE TABLE 表名
  (
  字段名1 数据类型,
  字段名2 数据类型,
  字段名3 数据类型,
  ...
  )
  ```
  数据库的基本数据类型可以分为三种：数字型，字符串型，日期型，而数字型又有整型和小数。由于每一种数据库的数据类型不太一样，以mysql为例
  ```shell
  mysql> create table dc_character(
         id int,
         name varchar(255),
         age int)
  Query OK, 0 rows affected (2.60 sec)
  ```

## 3.2 查
* **desc 表名**：查看表详情
  ```shell
  mysql> desc dc_character;
  +-------+--------------+------+-----+---------+-------+
  | Field | Type         | Null | Key | Default | Extra |
  +-------+--------------+------+-----+---------+-------+
  | id    | int          | YES  |     | NULL    |       |
  | name  | varchar(255) | YES  |     | NULL    |       |
  | age   | int          | YES  |     | NULL    |       |
  +-------+--------------+------+-----+---------+-------+
  3 rows in set (0.00 sec)
  ```

## 3.3 改
* **alter table 旧表名 rename to 新表名**：修改表名
  ```shell
    mysql> alter table dc_character rename to dc;
    Query OK, 0 rows affected (2.17 sec)

    mysql> show tables;
    +----------------------+
    | Tables_in_mydatabase |
    +----------------------+
    | dc                   |
    +----------------------+
    1 row in set (0.00 sec)
  ```

## 3.4 删
* **drop table 表名**：删除表
  ```shell
  mysql> drop table dc;
  Query OK, 0 rows affected (2.14 sec)

  mysql> show databases;
  +--------------------+
  | Database           |
  +--------------------+
  | information_schema |
  | mydatabase         |
  | mysql              |
  | performance_schema |
  | sys                |
  +--------------------+
  5 rows in set (0.00 sec)
  ```

## 3.5 字段操作sql
字段即表的列属性，而表字段本身包含5个属性：
1. **Field**：字段的名称
2. **Type**：字段数据类型
3. **Null**：字段的数据是否可以为`Null`，取值可以是`null`或者`not null`，后者就表示字段取值不可以是空
4. **KEY**：字段是否是主键或外来键，取值有`primary key`，`unique Key`, `key` 和 `foreign Key`
5. **Default**：字段的默认值
6. **Extra**：字段的一些额外信息

### 3.5.1 增
创建字段时可以加入属性，基本语法为
```
  字段名 字段类型 [Null取值] [KEY取值] [Default 默认值] [Extra信息]
```

比如
```shell
  mysql> create table dc(
    -> id int primary key auto_increment,
    -> name varchar(255) not null,
    -> age int default 30);
  Query OK, 0 rows affected (1.35 sec)

  mysql> desc dc;
  +-------+--------------+------+-----+---------+----------------+
  | Field | Type         | Null | Key | Default | Extra          |
  +-------+--------------+------+-----+---------+----------------+
  | id    | int          | NO   | PRI | NULL    | auto_increment |
  | name  | varchar(255) | YES  |     | NULL    |                |
  | age   | int          | YES  |     | 30      |                |
  +-------+--------------+------+-----+---------+----------------+
```

创建了表，其中`id`字段设置为主键，并设置为自增；`name`字段设置数据不能为`Null`；`age`字段设置默认值为30。

### 3.5.2 查
查询表的字段信息与前面的[表操作查询](https://gamersover.github.io/2021/04/16/数据库之sql实践/#3-2-查)一样

### 3.5.3 改
* **alter table 表名 add 新字段名 数据类型**：增加字段
  ```shell
  mysql> alter table dc add power bigint;
  Query OK, 0 rows affected (2.14 sec)
  Records: 0  Duplicates: 0  Warnings: 0

  mysql> desc dc;
  +-------+--------------+------+-----+---------+-------+
  | Field | Type         | Null | Key | Default | Extra |
  +-------+--------------+------+-----+---------+-------+
  | id    | int          | YES  |     | NULL    |       |
  | name  | varchar(255) | YES  |     | NULL    |       |
  | age   | int          | YES  |     | NULL    |       |
  | power | bigint       | YES  |     | NULL    |       |
  +-------+--------------+------+-----+---------+-------+
  4 rows in set (0.00 sec)
  ```

* **alter table 表名称 change 旧字段名 新字段名 新字段类型**：修改字段
  ```shell
  mysql> alter table dc change power power_level int;
  Query OK, 2 rows affected (2.36 sec)
  Records: 2  Duplicates: 0  Warnings: 0

  mysql> desc dc;
  +-------------+--------------+------+-----+---------+-------+
  | Field       | Type         | Null | Key | Default | Extra |
  +-------------+--------------+------+-----+---------+-------+
  | id          | int          | YES  |     | NULL    |       |
  | name        | varchar(255) | YES  |     | NULL    |       |
  | age         | int          | YES  |     | NULL    |       |
  | power_level | int          | YES  |     | NULL    |       |
  +-------------+--------------+------+-----+---------+-------+
  4 rows in set (0.00 sec)
  ```

* **alter table 表名称 modify 字段名称 新字段类型**：只修改字段类型
  ```shell
  mysql> alter table dc modify power_level bigint;
  Query OK, 2 rows affected (2.56 sec)
  Records: 2  Duplicates: 0  Warnings: 0

  mysql> desc dc;
  +-------------+--------------+------+-----+---------+-------+
  | Field       | Type         | Null | Key | Default | Extra |
  +-------------+--------------+------+-----+---------+-------+
  | id          | int          | YES  |     | NULL    |       |
  | name        | varchar(255) | YES  |     | NULL    |       |
  | age         | int          | YES  |     | NULL    |       |
  | power_level | bigint       | YES  |     | NULL    |       |
  +-------------+--------------+------+-----+---------+-------+
  4 rows in set (0.00 sec)
  ```

### 3.5.4 删
* **alter table 表名 drop 字段名**：删除字段
  ```shell
  ysql> alter table dc drop power_level;
  Query OK, 0 rows affected (1.14 sec)
  Records: 0  Duplicates: 0  Warnings: 0

  mysql> desc dc;
  +-------+--------------+------+-----+---------+-------+
  | Field | Type         | Null | Key | Default | Extra |
  +-------+--------------+------+-----+---------+-------+
  | id    | int          | YES  |     | NULL    |       |
  | name  | varchar(255) | YES  |     | NULL    |       |
  | age   | int          | YES  |     | NULL    |       |
  +-------+--------------+------+-----+---------+-------+
  3 rows in set (0.00 sec)
  ```

## 3.6 数据操作sql
假设表`dc`的结构为
```shell
mysql> desc dc;
+------------+--------------+------+-----+---------+----------------+
| Field      | Type         | Null | Key | Default | Extra          |
+------------+--------------+------+-----+---------+----------------+
| id         | int          | NO   | PRI | NULL    | auto_increment |
| name       | varchar(255) | NO   |     | NULL    |                |
| age        | int          | YES  |     | 30      |                |
| powerlevel | int          | YES  |     | NULL    |                |
+------------+--------------+------+-----+---------+----------------+
4 rows in set (0.00 sec)
```

### 3.6.1 查
查询表数据是最重要，也是最常用的sql语句。

#### 3.6.1.1 基本语法
* **select 字段1 [as 别名1], 字段2 [as 别名2], ... from 表名 [where 条件] [order by 字段名 [desc|asc]] [limit 条数]**：从表中选择满足条件的数据的指定字段，再按照某个字段排序后并限制前面多少条输出
  ```sql
  mysql> select * from dc; -- 最简单的查询语句，查询所有记录，* 表示所有字段
  +----+-------------+------+------------+
  | id | name        | age  | powerlevel |
  +----+-------------+------+------------+
  |  1 | superman    |   35 |         10 |
  |  2 | wonderwoman |   32 |          8 |
  |  3 | batman      |   35 |          4 |
  |  4 | flash       |   30 |          7 |
  |  5 | lantern     |   30 |          6 |
  |  6 | supergirl   |   33 |          8 |
  |  7 | cyborg      |   32 |          5 |
  +----+-------------+------+------------+
  7 rows in set (0.00 sec)

  mysql> select * from dc where age>=35; -- 加入条件查询年龄age大于等于25的记录
  +----+----------+------+------------+
  | id | name     | age  | powerlevel |
  +----+----------+------+------------+
  |  1 | superman |   35 |         10 |
  |  3 | batman   |   35 |          4 |
  +----+----------+------+------------+
  2 rows in set (0.00 sec)

  mysql> select name, age, powerlevel from dc where age>= 35 order by powerlevel; -- 在上一个查询基础上加入对结果按powerlevel排序，默认按升序（asc），加入关键词desc，可以使结果按降序
  +----+----------+------+------------+
  | id | name     | age  | powerlevel |
  +----+----------+------+------------+
  |  3 | batman   |   35 |          4 |
  |  1 | superman |   35 |         10 |
  +----+----------+------+------------+
  2 rows in set (0.00 sec)

  mysql> select * from dc limit 3; -- 查询记录只显示前面3条
  +----+-------------+------+------------+
  | id | name        | age  | powerlevel |
  +----+-------------+------+------------+
  |  1 | superman    |   35 |         10 |
  |  2 | wonderwoman |   32 |          8 |
  |  3 | batman      |   35 |          4 |
  +----+-------------+------+------------+
  3 rows in set (0.00 sec)
  ```

#### 3.6.1.2 聚合语法
* **select [字段1, 字段2, ...] 聚合函数3(字段3) [ [as] 别名3], 聚合函数4(字段4) [ [as] 别名4], ... from 表名 [group by 字段1, 字段2, ...]**：对某几个字段聚合，即相同的归一类，然后对其他字段求聚合函数，比如最大最小值等
  ```sql
  mysql> select max(powerlevel) from dc where age<35; -- 不加group by 对所有记录取聚合函数
  +-----------------+
  | max(powerlevel) |
  +-----------------+
  |               8 |
  +-----------------+
  1 row in set (0.00 sec)

  mysql> select age, max(powerlevel) as mp from dc group by age order by mp desc; -- 查看年龄相同的数据中分别最大的powerlevel，并按照最大值降序排序，取别名 as 可以省略，直接 max(powerlevel) mp
  +------+------+
  | age  | mp   |
  +------+------+
  |   35 |   10 |
  |   32 |    8 |
  |   33 |    8 |
  |   30 |    7 |
  +------+------+
  4 rows in set (0.00 sec)
  ```

#### 3.6.1.3 多表联合查询
有时候需要联合多个表的数据来统计结果，这时需要用到`join`语句，与前面两种语法相比，只需要改变`from`后面的语法即可。

* **select ... from 表1 [ [as] 别名1 ] [left|right|outer|inner] join 表2 [ [as] 别名2 ] on ([表1|别名1].字段1=[表2|别名2].字段1 [and 表1.字段1=表2.字段1]...)**：基于相同字段的值联合多个表查询

假设新增一个表`dc_name`，其数据为
```sql
mysql> select * from dc_name;
+------+------------+------+
| id   | name       | iq   |
+------+------------+------+
|    1 | lex luthor |  100 |
|    2 | batman     |  100 |
+------+------------+------+
2 rows in set (0.00 sec)
```

##### inner join
> 表示两个表取交集，即两个表都存在的数据取出

```sql
select * from dc t1 inner join dc_name t2 on (t1.name=t2.name); -- 默认会输出两个表的所有字段，若只写join，则默认是inner join
+----+--------+------+------------+------+--------+------+
| id | name   | age  | powerlevel | id   | name   | iq   |
+----+--------+------+------------+------+--------+------+
|  3 | batman |   35 |          4 |    2 | batman |  100 |
+----+--------+------+------------+------+--------+------+
1 row in set (0.00 sec)

mysql> select t1.*, t2.iq from dc t1 inner join dc_name t2 on (t1.name=t2.name); -- 只要t1的所有字段，t2的iq字段
+----+--------+------+------------+------+
| id | name   | age  | powerlevel | iq   |
+----+--------+------+------------+------+
|  3 | batman |   35 |          4 |  100 |
+----+--------+------+------------+------+
1 row in set (0.00 sec)
```

##### outer join
> 表示两个表取并集，即两个表存在的数据都会取出，全称 `full outer join`，不过有些mysql版本不支持，具体可以使用下面介绍到的`union`语法替代


##### left join
> 左表的数据不会丢失，将右表的数据添加到左表中；即左表有而右边没有的数据填null，而左表没有右表有的数据丢弃，左右表都有的数据直接添加

```sql
mysql> select t1.*, t2.iq from dc t1 left join dc_name t2 on (t1.name=t2.name); -- 一般left join 或者 right join 会加入where条件过滤，比如where iq is not null，则效果相当于inner join的效果
+----+-------------+------+------------+------+
| id | name        | age  | powerlevel | iq   |
+----+-------------+------+------------+------+
|  1 | superman    |   35 |         10 | NULL |
|  2 | wonderwoman |   32 |          8 | NULL |
|  3 | batman      |   35 |          4 |  100 |
|  4 | flash       |   30 |          7 | NULL |
|  5 | lantern     |   30 |          6 | NULL |
|  6 | supergirl   |   33 |          8 | NULL |
|  7 | cyborg      |   32 |          5 | NULL |
+----+-------------+------+------------+------+
7 rows in set (0.00 sec)
```
##### right join
> 与left join正好相反，右表的数据不会丢失

```sql
mysql> select t1.*, t2.iq from dc t1 right join dc_name t2 on (t1.name=t2.name);
+------+--------+------+------------+------+
| id   | name   | age  | powerlevel | iq   |
+------+--------+------+------------+------+
| NULL | NULL   | NULL |       NULL |  100 |
|    3 | batman |   35 |          4 |  100 |
+------+--------+------+------------+------+
2 rows in set (0.00 sec)
```

注意：2个以上表的join语法
```sql
select * from a
join b on (a.a1=b.b1)
join c on (a.a1=c.c1)
```

#### 3.6.1.4 子查询
以上语法中`from`语法后的表名都可以替换为`select`查询语句得到的中间结果，相当于从中间查询结果中再次查询，具体语法是

* **select ... from (select ... from ...) ...**：嵌套查询，从中间查询结果中再次查询，一般会将中间查询结果与原表`join`后再查询
  ```sql
  mysql> select dc.* from dc inner join (select age, max(powerlevel) as mp from dc group by age) t1 on (dc.age=t1.age and dc.powerlevel=t1.mp); -- 查询年龄age相同的数据中，powerlevel最大的记录并包含名字name等所有原始信息的数据
  +----+-------------+------+------------+
  | id | name        | age  | powerlevel |
  +----+-------------+------+------------+
  |  1 | superman    |   35 |         10 |
  |  2 | wonderwoman |   32 |          8 |
  |  4 | flash       |   30 |          7 |
  |  6 | supergirl   |   33 |          8 |
  +----+-------------+------+------------+
  4 rows in set (0.00 sec)
  ```

#### 3.6.1.5 查询结果合并
union语法，可以合并多个`select`语句的查询结果，具体语法：
  
* **select ... from ... [distinct] union select ... from ...**

```sql
mysql> select * from dc t1 left join dc_name t2 on (t1.name=t2.name)
    -> union
    -> select * from dc t1 right join dc_name t2 on (t1.name=t2.name);  -- 使用union语句合并left join与right join的结果，相当于full outer join
+------+-------------+------+------------+------+------------+------+
| id   | name        | age  | powerlevel | id   | name       | iq   |
+------+-------------+------+------------+------+------------+------+
|    1 | superman    |   35 |         10 | NULL | NULL       | NULL |
|    2 | wonderwoman |   32 |          8 | NULL | NULL       | NULL |
|    3 | batman      |   35 |          4 |    2 | batman     |  100 |
|    4 | flash       |   30 |          7 | NULL | NULL       | NULL |
|    5 | lantern     |   30 |          6 | NULL | NULL       | NULL |
|    6 | supergirl   |   33 |          8 | NULL | NULL       | NULL |
|    7 | cyborg      |   32 |          5 | NULL | NULL       | NULL |
| NULL | NULL        | NULL |       NULL |    1 | lex luthor |  100 |
+------+-------------+------+------------+------+------------+------+
8 rows in set (0.00 sec)
```

### 3.6.2 增
* **insert into 表名 (字段1, 字段2, ...) values (字段1数据, 字段2数据, ...)**：向表中添加一行数据
  ```shell
  mysql> insert into dc values (0, 'superman', 35, 10);
  Query OK, 1 row affected (0.08 sec)

  mysql> select * from dc;
  +----+----------+------+------------+
  | id | name     | age  | powerlevel |
  +----+----------+------+------------+
  |  1 | superman |   35 |         10 |
  +----+----------+------+------------+
  1 row in set (0.00 sec)
  ```
  注意到:
  * 如果不填字段名称，默认所有字段按顺序匹配
  * 由于`id`是自增的，所以`id`默认从1开始，添加数据的时候也可以指定字段，这样`id`会自动加1。

  ```shell
  mysql> insert into dc (name, age) values ('wonderwoman', 34);
  Query OK, 1 row affected (0.12 sec)

  mysql> select * from mydatabase.dc;
  +----+-------------+------+------------+
  | id | name        | age  | powerlevel |
  +----+-------------+------+------------+
  |  1 | superman    |   35 |         10 |
  |  2 | wonderwoman |   34 |       NULL |
  +----+-------------+------+------------+
  2 rows in set (0.00 sec)
  ```
  注意到`id`自增1，`powerlevel`会取默认值`NULL`。


* **insert (into|overwrite) 表名 select查询语句**：从另一表中中导入数据
  * into是追加方式
  * overwirte是重写方式

  ```sql
  mysql> create table dc_name (id int, name varchar(255)); -- 创建新表dc_name
  Query OK, 0 rows affected (2.56 sec)

  mysql> show tables;
  +----------------------+
  | Tables_in_mydatabase |
  +----------------------+
  | dc                   |
  | dc_name              |
  +----------------------+
  2 rows in set (0.00 sec)

  mysql> insert into dc_name select id, name from dc; -- 从dc表中导入数据给新表dc_name
  Query OK, 2 rows affected (0.92 sec)
  Records: 2  Duplicates: 0  Warnings: 0

  mysql> select * from dc_name;
  +------+-------------+
  | id   | name        |
  +------+-------------+
  |    1 | superman    |
  |    2 | wonderwoman |
  +------+-------------+
  2 rows in set (0.00 sec)
  ```
### 3.6.3 删
* **delete from 表名 [where 条件]**：根据条件从表中删除数据
  ```sql
  mysql> delete from dc_name where id=1; -- 删除id=1的记录
  Query OK, 1 row affected (0.10 sec)

  mysql> select * from dc_name;
  +------+-------------+
  | id   | name        |
  +------+-------------+
  |    2 | wonderwoman |
  +------+-------------+
  1 row in set (0.00 sec)

  mysql> delete from dc_name; -- 不加条件，默认删除所有记录
  Query OK, 1 row affected (0.08 sec)

  mysql> select * from dc_name;
  Empty set (0.00 sec)
  ```

### 3.6.4 改
* **update 表名 set 字段1=值1 [, 字段2=值2, ...] where 条件**：修改某个数据
  ```sql
  mysql> select * from dc;
  +----+-------------+------+------------+
  | id | name        | age  | powerlevel |
  +----+-------------+------+------------+
  |  1 | superman    |   35 |         10 |
  |  2 | wonderwoman |   34 |       NULL |
  +----+-------------+------+------------+
  2 rows in set (0.00 sec)

  mysql> update dc set age=30, powerlevel=8 where id=2; -- 修改id=2的age字段与powerlevel字段
  Query OK, 1 row affected (0.08 sec)
  Rows matched: 2  Changed: 2  Warnings: 0

  mysql> select * from dc;
  +----+-------------+------+------------+
  | id | name        | age  | powerlevel |
  +----+-------------+------+------------+
  |  1 | superman    |   35 |         10 |
  |  2 | wonderwoman |   30 |          8 |
  +----+-------------+------+------------+
  2 rows in set (0.00 sec)

  mysql> update dc set age=35; -- 不加where条件，默认修改所有行的age字段值
  Query OK, 1 row affected (1.94 sec)
  Rows matched: 2  Changed: 1  Warnings: 0

  mysql> select * from dc;
  +----+-------------+------+------------+
  | id | name        | age  | powerlevel |
  +----+-------------+------+------------+
  |  1 | superman    |   35 |         10 |
  |  2 | wonderwoman |   35 |          8 |
  +----+-------------+------+------------+
  2 rows in set (0.00 sec)
  ```