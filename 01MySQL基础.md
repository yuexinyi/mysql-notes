# 01MySQL基础

# 1.数据库的基础

## 1.1数据库的引入

**解决问题**：解决文件存储的问题，有效的管理数据，方便存储和查找

**文件存储的缺陷**：
（1）安全性差；
（2）不利于存储大量数据；
（3）不利于数据查询和管理；
（4）文件在程序中不方便使用；

**数据库存储介质**：磁盘、内存

## 1.2常见的数据库

- SQL Server:微软公司产品，适合中大型项目
- Oracle:甲骨文产品，适合大型项目
- MySQL:甲骨文产品，并发性好，不适合复杂业务

# 2.MySQL介绍

## 2.1连接服务器

输入：

```mysql
mysql -h 127.0.0.1 -P 3306 -u root -p
```

输出：

```mysql
Enter password: ****
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.16 MySQL Community Server - GPL

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
```

一般情况下，

- 不写-h 127.0.0.1,默认连接本地；
- 不写-P 3306,默认是连接3306端口号；

## 2.2SQL分类

- DDL数据定义语言：维护存储数据的结构

  create,drop,alter

- DML数据操纵语言：对数据进行操作

  insert,delete,update

  - DQL数据查询语言：对数据进行查询

    select

- DCL数据控制语言：负责权限管理和事务

  grant,revoke,commit

## 2.3基本使用

1.创建数据库

```mysql
create database test;
```

2.使用数据库

```mysql
use test;
```

3.创建表

```mysql
create table student(
	id int,
	name varchar(32),
	gender varchar(2)
);
```

4.显示表结构

```mysql
desc student;
```

5.表中插入数据

```mysql
insert into student(id,name,gender) values (1,'张三','男')
```

6.查询表中数据

```mysql
select * from student;
```

## 2.4MySQL存储引擎

高版本的数据库默认存储引擎是InnoDB

**存储引擎**：数据库管理系统如何存储数据、如何为存储的数据建立索引和如何更新、查询数据等技术的实现方法

MySQL支持多种存储引擎；

查看存储引擎：

```mysql
show engines;
```

![1578984665896](C:\Users\岳心怡\AppData\Roaming\Typora\typora-user-images\1578984665896.png)

存储引擎对比：

![1578984702254](C:\Users\岳心怡\AppData\Roaming\Typora\typora-user-images\1578984702254.png)

存储限制：
MyISAM、BDB、Archive —NO
Memory、NDB－YES
InnoDB—64TB

支持事务：BDB、InnoDB

锁的粒度：
Table：MyISAM、Memory
Row：InnoDB、Archive、NDB
Page:BDB

支持树索引：除了Archive

支持哈希索引：Memory、InnoDB、NDB

支持全文搜索索引：MyISAM

支持聚簇索引：InnoDB

支持数据缓存：Memory、InnoDB、NDB

支持索引缓存：MyISAM、Memory、InnoDB、NDB

支持压缩数据：MyISAM、NDB

支持加密数据：全部

存储成本：
High—InnoDB
Low—MyISAM、BDB、NDB
Very Low—Archive

记忆成本：
High—InnoDB、DBD
Medium—Memory
Low—MyISAM、BDB、Archive

批量插入速度：
VeryHigh—Archive
High—MyIASM、BDB、NDB
Low－InnoDB

复制支持：全部

外键支持：InnoDB

查询缓存支持：全部

更新数据字典的统计信息：全部



drop、delete、truncate区别：

- 速度：drop>truncate>delete
- 删除部分数据delete
- 删除表、数据库drop
- 保留表删除所有数据，与事务无关truncate
- 与事务有关，或者触发trigger使用delete