---
title: 数据库之sql实践：
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

数据库，顾名思义用来存放数据的地方，可以把数据库比作图书馆，图书馆里面有不同区域相当于不同的数据库（database），而每个区域又存放着不同的书籍，相当于每个database含有很多表（table），图中表现了这种关系。而表中有包含数据，数据是由记录（行）和字段（列）构成的，每条记录的字段的数据就是存储的数据。

表即普通的表格，比如下面的表有两条记录，字段包括id，name，age。
|id|name|age|
|:-:|:-:|:-:|
|0|superman|32|
|1|wonderwoman|102|
|2|batman|30|

对于数据库，表，记录，字段都有增、删、改、查的操作，而sql（Structured Query Language）即结构化查询语言就是用来完成这些操作的，根据操作的对象不同将sql语言分为DML(data manipulation language)，DQL(Data Query Language)，DDL(data definition language)