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

### 概述
数据库，顾名思义用来存放数据的地方，可以把数据库比作图书馆，图书馆里面有不同区域相当于不同的数据库（database），而每个区域又存放着不同的书籍，相当于每个database含有很多表（table），图中表现了这种关系。
<!--more-->

<img src="https://raw.githubusercontent.com/gamersover/hexo_blog_assets/main/%E6%95%B0%E6%8D%AE%E5%BA%93/No1.jpg" width="25%">

而表中有包含数据，数据是由记录（行）和字段（列）构成的，每条记录的字段的数据就是存储的数据。表即普通的表格，下表中有两条记录（两行），三个字段（三列）为`id`，`name`，`age`。

|id|name|age|
|:-:|:-:|:-:|
|0|superman|32|
|1|wonderwoman|102|

对于数据库，表，记录，字段都有增、删、改、查的操作，而sql（Structured Query Language）即结构化查询语言就是用来完成这些操作的，对于不同的对象有着不同的sql语句。大致包含对数据库的操作，对表的操作，对字段的操作，对记录的操作。在执行单条sql语句时可以不以`;`结束，但是执行多条语句时需要以`;`结束。而且sql语句大小写不敏感，即不区分大小写。

### 数据库操作sql
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

### 表操作sql
* **创建表**
