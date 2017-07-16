# 史上最简单的 MySQL 教程（二十七）「连接查询（下）」

**连接查询**：将多张表（大于等于 2 张表）按照某个指定的条件进行数据的拼接，其最终结果记录数可能有变化，但字段数一定会增加。

**连接查询的意义**：在用户查询数据的时候，需要显示的数据来自多张表。

连接查询为`join`，使用方式为：**左表`join`右表**。

 - 左表：`join`左边的表；
 - 右表：`join`右边的表。

**连接查询分类**：在 SQL 中将连接查询分为四类，分别为内连接、外链接、自然连接和交叉连接。


## 外连接

**外连接**：`left\right join`，以某张表为主表，取出里面的所有记录，然后让主表中的每条记录都与另外一张表进行连接，不管能否匹配成功，其最终结果都会保留，匹配成功，则正确保留；匹配失败，则将另外一张表的字段都置为`NULL`.

 - **基本语法**：`左表 + left\right + join + 右表 + on + 左表.字段 = 右表.字段;`

其中，关键字`on`表示连接条件，两表中的条件字段有着相同的业务含义。在这里，以主表为依据，外连接分为两种，分别为：

 - `left join`：左外连接（左连接），以左表为主表；
 - `right join`：右外连接（右连接），以右表为主表。

执行如下 SQL 语句，进行测试：

```
-- 将表 student 与 class 进行左连接
select s.*,c.id as c_id,c.grade as c_grade,room from student as s left join class as c on s.grade = c.grade;

-- 将表 student 与 class 进行右连接
select s.*,c.id as c_id,c.grade as c_grade,room from student as s right join class as c on s.grade = c.grade;
```

![outer](http://img.blog.csdn.net/20170715000220792)

实际上，无论以那张表为主表，其外连接的结果（记录数量）都不会少于主表的记录总数。此外，虽然左连接与右连接有主表差异，但显示的结果都是：**左表的数据在左边，右表的数据在右边**。


## 自然连接


**自然连接**：`nature join`，自然连接其实就是自动匹配连接条件，系统以两表中同名字段作为匹配条件，如果两表有多个同名字段，那就都作为匹配条件。在这里，自然连接可以分为自然内连接和自然外连接。

### 自然内连接

 - **基本语法**：`左表 + nature + join + 右表;`

执行如下 SQL 语句，进行测试：

```
-- 将表 student 与 class 进行自然内连接
select * from student natural join class;

-- 将表 student 与 class 进行内连接，连接条件为 id 和 grade
select * from student inner join class on student.id = class.id and student.grade = class.grade;
```

![natural](http://img.blog.csdn.net/20170715103845312)

观察上图，咱们会发现：**自然连接自动使用同名字段作为连接条件，而且在连接完成之后合并同名字段**。

### 自然外连接

 - **基本语法**：`左表 + nature + left/right + join + 右表;`

执行如下 SQL 语句，进行测试：

```
-- 将表 student 与 class 进行自然左外连接
select * from student natural left join class;

-- 将表 student 与 class 进行自然右外连接
select * from student natural right join class;
```

![naturalwai](http://img.blog.csdn.net/20170715104058311)

实际上，自然连接并不常用。而且，咱们可以用内连接和外连接来模拟自然连接，模拟的关键就在于使用同名字段作为连接条件及合并同名字段。

 - **基本语法**：`左表 + inner/left/right + join + 右表 + using(字段名);`

其中，`using`内部的字段名就是作为连接条件的字段，也是需要合并的同名字段。

执行如下 SQL 语句，进行测试：

```
-- 将表 student 与 class 进行自然左外连接
select * from student natural left join class;

-- 用左外连接模拟自然左外连接
select * from student left join class using(id,grade);
```

![moni](http://img.blog.csdn.net/20170715105006248)

----------

**温馨提示**：符号`[]`括起来的内容，表示可选项；符号`+`，则表示连接的意思。


----------
———— ☆☆☆ —— [返回 -> 史上最简单的 MySQL 教程 <- 目录](https://github.com/guobinhit/mysql-tutorial/blob/master/README.md) —— ☆☆☆ ————
