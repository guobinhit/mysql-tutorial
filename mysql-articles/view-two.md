# 史上最简单的 MySQL 教程（三十四）「视图（下）」

**视图数据操作**：虽然我们说视图可以称之为`select`语句的别名，但实际上，它和别名并不一样，因为视图是可以进行数据写操作的，只不过有很多限制而已。

## 新增数据

在这里，新增数据就是指通过视图直接对基表进行数据的新增操作。

 - **限制 1**：多表视图不能进行新增数据。

执行如下 SQL 语句，进行测试：

```
-- 查询视图 my_v3 结构
desc my_v3;

-- 多表视图新增数据
insert into my_v3 values(7,'Gates','boy',2,170,'PM3.5','A315');
```

![1](http://img.blog.csdn.net/20170909134753471)

 - **限制 2**：可以向单表视图新增数据，但视图中包含的字段必须有基表中所有不能为空的字段。

执行如下 SQL 语句，进行测试：

```
-- 查询 student 表结构
desc student;

-- 创建视图 my_v4
create view my_v4 as
select id,name,age,c_id from student;

-- 单表视图新增数据
insert into my_v4 values(7,'Gates',25,2);
```

![2](http://img.blog.csdn.net/20170909141917265)

如上图所示，在我们新建的视图`my_v4`中，没有包含不能为空的字段`gender`，因此在我们向`my_v4`中新增数据的时候，报错。其实，这也很好理解，试想，在 MySQL 尝试将视图中新增的数据（一条记录）插入到基表的时候，忽然发现一个本不能为`null`的字段的值为默认值`null`，自然就会报错啦！反之，如果单表视图中包含了基表中的全部非空字段，自然可以插入成功。执行如下 SQL 语句，进行测试：

```
-- 查询 class 表数据
select * from class;

-- 创建视图 my_v5
create view my_v5 as
select * from class;

-- 单表视图新增数据
insert into my_v5 values(2,'PM2016','A315');

-- 查询 class 表数据
select * from class;
```

![3](http://img.blog.csdn.net/20170909143447416)

如上图所示，显然我们通过单表视图向基表中插入数据成功啦！


## 删除数据

与新增数据类似，

 - 多表视图不能删除数据；
 - 单表视图可以删除数据。

执行如下 SQL 语句，进行测试：

```
-- 查询多表视图 my_v3
select * from my_v3;

-- 删除多表视图 my_v3 中记录
delete from my_v3 where id = 2;

-- 查询单表视图 my_v5
select * from my_v5;

-- 删除单表视图 my_v5 中记录
delete from my_v5 where id = 2;

-- 查询单表视图 my_v5
select * from my_v5;
```

![4](http://img.blog.csdn.net/20170909145056989)

## 更新数据

理论上，无论多表视图还是单表视图，都可以进行数据的更新。

执行如下 SQL 语句，进行测试：

```
-- 查询单表视图 my_v5
select * from my_v5;

-- 更新单表视图 my_v5
update my_v5 set grade = 'PM2014' where id = 5;

-- 查询单表视图 my_v5
select * from my_v5;-- 查询单表视图 my_v5
select * from my_v5;

-- 更新单表视图 my_v5
update my_v5 set grade = 'PM2014' where id = 5;

-- 查询单表视图 my_v5
select * from my_v5;
```

![5](http://img.blog.csdn.net/20170909150303075)

此外，更新视图数据并不总是成功的，这是因为有**更新限制**的存在。那么何为更新限制呢？

 - 更新限制：`with check option`，如果创建视图的时候，设置了某个字段的限制，那么对视图进行更新操作的时候，系统就会进行验证，要保证更新之后，数据依然可以被查出来，否则不让更新。

执行如下 SQL 语句，进行测试：

```
-- 创建单表视图 my_v6
create view my_v6 as
select * from student where height > 170 with check option;

-- 查询单表视图 my_v6
select * from my_v6;

-- 更新单表视图 my_v6
update my_v6 set height = 165 where id = 6;
```

![6](http://img.blog.csdn.net/20170909151612737)


如上图所示，在更新视图的时候，更新失败，这是因为其违反了我们设置的更新限制。那么，视图之外的数据，我们能不能修改呢？执行如下 SQL 语句，进行测试：

```
-- 查询单表视图 my_v6
select * from my_v6;

-- 更新单表视图 my_v6
update my_v6 set height = 188 where id = 4;

-- 查询单表视图 my_v6
select * from my_v6;
```

![7](http://img.blog.csdn.net/20170909152042006)

如上图所示，更新视图`my_v6`之外数据的时候，显示成功。但是，待我们重新查询视图`my_v6`数据的时候，发现并没有真正更新成功。这是为什么呢？原因就在于我们不能通过视图去操作视图之外的数据。举一个不太恰当的例子，我们不能用自己手去操作别人兜里的钱啊！


## 视图算法

视图算法，即系统对视图以及外部查询视图的`select`语句的一种解析方式。视图算法有三种，分别为：

 - `undefined`：未定义（默认的），这不是一种实际使用的算法，而是一个“推卸责任”的算法。在未定义的情况下，告诉系统，视图没有定义算法，请自己选择。
 - `temptable`：临时表算法，系统先执行视图的`select`语句，后执行外部查询语句。
 - `merge`：合并算法，系统先将视图对应的`select`语句与外部查询视图的`select`语句进行合并，然后再执行。此算法比较高效，且在未定义算法的时候，经常会默认选择此算法。

对于视图的算法，我们需要在创建视图的时候指定，

 - **基本语法**：`create + [algorithm = temptable/merge/undefined] + view + 视图名 + as + select语句;`

执行如下 SQL 语句，进行测试：

```
-- 查看视图 my_v2 的默认算法
show create view my_v2;

-- 指定视图 my_v7 算法为 temptable
create algorithm = temptable view my_v7 as
select * from student;

-- 查看视图 my_v7 的指定算法
show create view my_v7;
```

![8](http://img.blog.csdn.net/20170909163651397)

如上图所示，我们指定了视图`my_v7`的算法为`temptable`，但是对于算法的选择，我们该如何判断呢？答案是：如果视图的`select`语句中包含一个查询子句（五子句，包括`where`、`group by`、`order by` 、`having`和`limit`），而且很有可能查询子句的顺序比外部的查询语句的顺序要靠后（五子句的顺序），那么一定要使用`temptable`算法，其他情况可以不用指定，默认即可。

至此，我们已经将视图的内容大致讲完了。有一点需要我们特别注意，那就是：**视图最重要的功能就是查询，其他如增、删、改的操作一般不会使用，也不建议通过视图来操作基表的数据**。


----------

**温馨提示**：符号`[]`括起来的内容，表示可选项；符号`+`，则表示连接的意思。


----------
———— ☆☆☆ —— [返回 -> 史上最简单的 MySQL 教程 <- 目录](https://github.com/guobinhit/mysql-tutorial/blob/master/README.md) —— ☆☆☆ ————
