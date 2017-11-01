# 史上最简单的 MySQL 教程（三十二）「子查询（下）」

**子查询**：`sub query`，查询是在某个查询结果之上进行的，一条`select`语句内部包含了另外一条`select`语句。

## 行子查询

行子查询，返回的结果可以使一行多列或者多行多列。

**需求**：查询学生表中，年龄最大且身高最高的学生。

**思路**：

 - 先确定数据源，学生表。
  - `select * from student where age = ? and height = ?;`
 - 然后确定最大年龄和最大身高。
  - `select max(age), max(height) from student;`

执行如下 SQL 语句，进行测试：

```
-- 列子查询
select * from student where
-- 其中，(age, height) 称之为行元素
(age, height) = (select max(age), max(height) from student);
```

![row](http://img.blog.csdn.net/20170827105939052)

## 表子查询

表子查询，返回的结果是多行多列二维表（将子查询的结果当做二维表来使用），实际上，任何查询返回的结果都可以称之为二维表。

**需求**：找出每个班身高最高的学生。

**思路**：

 - 先确定数据源，将学生按身高进行降序排序。
  - `select * from student order by height desc;`
 - 从每个班级选出第一个学生。
  - `select * from student group by c_id;`

在这里，我们可能会有些疑问：为什么要将学生表降序排序？为什么从每个班级选出第一个学生就可以？这是因为`group by`（分组）只会取表中分组字段的第一条记录，而当我们将学生表按身高降序排序时，（每组）身高最高的学生就会出现在第一位。

执行如下 SQL 语句，进行测试：

```
-- 表子查询
select * from 
-- 关键字 from 后面接表名
(select * from student order by height desc) as student 
-- 按 c_id 进行分组
group by c_id;
```

![table](http://img.blog.csdn.net/20170827112336833)

由上面的 SQL 语句可知，表子查询也是`from`子查询，即有`select`语句位于`from`之后。


## `exists`子查询

`exists`：表示是否存在的意思，因此`exists`子查询就是用来判断某些条件是否满足（跨表），`exists`是接在`where`之后，其返回的结果为`1`或`0`，满足条件为`1`，反之为`0`.

**需求**：在班级存在的前提下，查询所有的学生。

**思路**：

 - 先确定数据源。
  - `select * from student where ?;`
 - 然后确定条件是否满足。
  - `exists(select * from class);`

执行如下 SQL 语句，进行测试：

```
-- exists 子查询
select * from student where
exists(select * from class);

-- 添加限定条件，满足条件
select * from student where
exists(select * from class where id = 3);

-- 添加限定条件，不满足条件
select * from student where
exists(select * from class where id = 100);
```

![exists](http://img.blog.csdn.net/20170827120457531)

至此，我们已经将子查询学习完啦！也许大家还会有些疑惑，那就是到底在什么时候用什么子查询？对于这个问题，我们不用过于纠结，因为这根本就是我们要用什么子查询的问题，而是根据实际需求，我们将查询返回的结果按形式命名的称呼而已。

----------
**查询`class`表**：

![class](http://img.blog.csdn.net/20170826223402050)

**查询`student`表**：

![student](http://img.blog.csdn.net/20170827105224514)


----------

**温馨提示**：符号`[]`括起来的内容，表示可选项；符号`+`，则表示连接的意思。


----------
———— ☆☆☆ —— [返回 -> 史上最简单的 MySQL 教程 <- 目录](https://github.com/guobinhit/mysql-tutorial/blob/master/README.md) —— ☆☆☆ ————
