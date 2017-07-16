# 史上最简单的 MySQL 教程（二十六）「连接查询（上）」

**连接查询**：将多张表（大于等于 2 张表）按照某个指定的条件进行数据的拼接，其最终结果记录数可能有变化，但字段数一定会增加。

**连接查询的意义**：在用户查询数据的时候，需要显示的数据来自多张表。

连接查询为`join`，使用方式为：**左表`join`右表**。

 - 左表：`join`左边的表；
 - 右表：`join`右边的表。

**连接查询分类**：在 SQL 中将连接查询分为四类，分别为内连接、外链接、自然连接和交叉连接。


## 交叉连接

**交叉连接**：`cross join`，从一张表中循环取出每一条记录，每条记录都去另外一张表进行匹配，匹配的结果都保留（没有条件匹配），而连接本身的字段会增加，最终形成的结果为笛卡尔积形式。

 - **基本语法**：`左表 cross join 右边;`

其结果与多表查询相同。

执行如下 SQL 语句，进行测试：

```
-- 将表 student 与 class 进行交叉连接
select * from student cross join class;
-- 将表 student 与 class 进行多表查询
select * from student,class;
```

![01](http://img.blog.csdn.net/20170711212738238)

实际上，笛卡尔积形式（交叉连接和多表查询）的结果并没有什么实际意义，应该尽量避免，其存在的价值就是保证连接这种结构的完整性。

## 内连接


**内连接**：`inner join`，从左表中取出每一条记录，和右表中的所有记录进行匹配，并且仅当某个条件在左表和右表中的值相同时，结果才会保留，否则不保留。

 - **基本语法**：`左表 + [inner] + join + 右表 + on + 左表.字段 = 右表.字段;`

其中，关键字`on`表示连接条件，两表中的条件字段有着相同的业务含义。

执行如下 SQL 语句，进行测试：

```
-- 将表 student 与 class 进行内连接
select * from student inner join class on student.grade = class.grade;
select * from student join class on student.grade = class.grade;
```

![innerjoin](http://img.blog.csdn.net/20170714225952197)

在这里，值得注意的是：**如果两表中有某个表的条件字段名唯一，那么在书写连接条件的时候，可以省略表名，直接书写字段名，MySQL 会自动识别唯一字段名，但不建议这么做**。此外，咱们会发现，在上面的结果中有同名字段，这会给咱们理解数据的意义造成一定的困扰，这时就需要使用字段别名和表别名做区别啦！

执行如下 SQL 语句，进行测试：

```
-- 将表 student 与 class 进行内连接，起别名
select s.*,c.id as c_id,c.grade as c_grade,room from student as s inner join class as c on s.grade = c.grade;
```

![alias](http://img.blog.csdn.net/20170714232531767)

最后，内连接可以没有连接条件，即可以没有`on`及之后的内容，这时内连接的结果全部保留，与交叉连接的结果完全相同。而且在内连接的时候可以使用`where`关键字代替`on`，但不建议这么做，因为`where`没有`on`的效率高。

执行如下 SQL 语句，进行测试：

```
-- 将表 student 与 class 进行内连接，不加连接条件
select s.*,c.id as c_id,c.grade as c_grade,room from student as s inner join class as c;

-- 将表 student 与 class 进行交叉连接 
select s.*,c.id as c_id,c.grade as c_grade,room from student as s cross join class as c;

-- 使用 on 关键字进行内连接
select s.*,c.id as c_id,c.grade as c_grade,room from student as s inner join class as c on s.grade = c.grade;

-- 使用 where 关键字进行内连接
select s.*,c.id as c_id,c.grade as c_grade,room from student as s inner join class as c where s.grade = c.grade;
```

![onon](http://img.blog.csdn.net/20170714233908019)
![whereon](http://img.blog.csdn.net/20170714233930361)


----------

**温馨提示**：符号`[]`括起来的内容，表示可选项；符号`+`，则表示连接的意思。


----------
———— ☆☆☆ —— [返回 -> 史上最简单的 MySQL 教程 <- 目录](http://blog.csdn.net/qq_35246620/article/details/70823903) —— ☆☆☆ ————
