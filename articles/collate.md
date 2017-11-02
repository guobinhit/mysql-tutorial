# 史上最简单的 MySQL 教程（六）「校对集问题」

校对集问题
=====

**校对集，其实就是数据的比较方式。**

校对集，共有三种，分别为：

 - `_bin`：binary，二进制比较，区分大小写；
 - `_cs`：case sensitive，大小写敏感，区分大小写；
 - `_ci`：case insensitive，大小写不敏感，不区分大小写。

查看（全部）校对集 --> 基本语法：`show collation;`

![10](http://img.blog.csdn.net/20170505184606504)

如上图所示，MySQL 数据库支持百多种校对集。

接下来，咱们在一起看看校对集的应用，因为只有当数据进行比较的时候，校对集才会生效。在这里，咱们用`utf8`的`_bin`和`_ci`两种校对集进行比较：

```
-- 创建两张使用不同校对集的表
create table my_collate_bin(
	name char(10)
)charset utf8 collate utf8_bin;

create table my_collate_ci(
	name char(10)
)charset utf8 collate utf8_general_ci;

```

![11](http://img.blog.csdn.net/20170505185330045)

如上图所示，咱们创建了两张表，分别为`my_collate_bin`和`my_collate_ci`，其校对集分别为`_bin`和`_ci`. 然后，分别向这两张表中添加数据：

```
-- 向表中添加数据
insert into my_collate_bin values ('a'),('A'),('B'),('b');
insert into my_collate_ci values ('a'),('A'),('B'),('b');
```

再分别查看两张表中的数据：

```
-- 查看表中的数据
select * from my_collate_bin;
select * from my_collate_ci;
```
![13](http://img.blog.csdn.net/20170505190118166)

下面，咱们根据表中的某个字段（在这里`my_collate_bin`和`my_collate_ci`都仅有一个字段）进行排序，其基本语法为：

```
order by + 字段名 + [asc/desc];
```
其中，`asc`表示升序，`desc`表示降序，默认为升序。执行如下 SQL 语句：

```
-- 排序比较
select * from my_collate_bin order by name;
select * from my_collate_ci order by name;
```

![14](http://img.blog.csdn.net/20170505190700955)

如上图所示，显然校对集生效啦！

此外，咱们需要特别注意的是：	**校对集必须在没有数据之前声明好，如果有了数据之后，再进行校对集的修改，则修改无效。**


----------

**温馨提示**：用符号`[]`括起来的内容，表示可选项；符号`+`，则表示连接的意思。

----------
———— ☆☆☆ —— [返回 -> 史上最简单的 MySQL 教程 <- 目录](https://github.com/guobinhit/mysql-tutorial/blob/master/README.md) —— ☆☆☆ ————



