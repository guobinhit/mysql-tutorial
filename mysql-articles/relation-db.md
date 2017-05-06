# 史上最简单的 MySQL 教程（二）「关系型数据库」

关系型数据库
======

1 定义
----
关系型数据库，**是一种建立在关系模型（数学模型）上的数据库。**

至于关系模型，则是一种所谓建立在关系上的模型，其包含三个方面，分别为：

 - 数据结构：数据存储的形式，二维表（行和列）；
 - 操作指令集合：所有的 SQL 语句；
 - 完整性约束：表内数据约束（字段与字段）和表与表之间的约束（外键）。

2 设计
----

 - 数据库：从需要存储的数据需求中分析，如果是一类数据（实体），则应该设计成二维表；
 - 二维表：由表头（字段名，用来规定数据的名称）和数据（实际存储的内容）部分组成。

在此处，需要特别注意：**如果表中对应的某个字段值为空，但是系统依然会为其分配存储空间**，这也就是关系型数据库比较浪费空间的原因啦！

3 关键字说明
-------

 - DB：Database，数据库；
 - DBMS：Database Management System，数据库管理系统；
 - DBS：Database System = DBMS + DB，数据库系统；
 - DBA：Database Administrator，数据库管理员。
 - 行\记录：`row\record`，本质都是指表中的一行（一条记录），行是从结构角度出发，记录则是从数据角度出发。
 - 列\字段：`column\field`，本质都是指表中的一列（一个字段），列是从结构角度出发，字段则是从数据角度出发。

4 SQL
-----

SQL：Structured Query Language，结构化查询语言（数据以查询为主，99% 都是在进行查询操作）。

SQL 主要分为三种：

 - DDL：Data Definition Language，数据定义语言，用来维护存储数据的结构（数据库、表），代表指令为`create`、`drop`和`alter`等。
 - DML：Data Manipulation Language，数据操作语言，用来对数据进行操作（表中的内容）代表指令为`insert`、`delete`和`update`等，不过在 DML 内部又单独进行了一个分类，即 DQL（Data Query Language），数据查询语言，代表指令为`select`.
 - DCL：Data Control Language，数据控制语言，主要是负责（用户）权限管理，代表指令为`grant`和`revoke`等。

SQL 是关系型数据库的操作指令，是一种约束，但不强制，类似于 W3C，因此这意味着：不同的数据库产品（如 Oracle 和 MySQL）内部可能会有一些细微的区别。

----------
———— ☆☆☆ —— [返回 -> 史上最简单的 MySQL 教程 <- 目录](https://github.com/guobinhit/mysql-tutorial/blob/master/README.md) —— ☆☆☆ ————
