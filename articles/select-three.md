# 史上最简单的 MySQL 教程（二十五）「数据的高级操作 之 查询（下）」

## 数据的高级操作 - 查询数据（下）

### `having`子句

`having`字句：**与`where`子句一样，都是进行条件判断的**，但是`where`是针对磁盘数据进行判断，数据进入内存之后，会进行分组操作，分组结果就需要`having`来处理。思考可知，`having`能做`where`能做的几乎所有事情，但是`where`却不能做`having`能做的很多事情。

**第 1 点：分组统计的结果或者说统计函数只有`having`能够使用**

执行如下 SQL 语句，进行测试：

```
-- 求出表 student 中所有班级人数大于等于 2 的班级
select grade,count(*) from student group by grade having count(*) >= 2;
select grade,count(*) from student where count(*) >= 2 group by grade;
```

![1](http://img.blog.csdn.net/20170711085904967)

如上图所示，显然`having`子句可以对统计函数得到的结果进行筛选，但是`where`却不能。

**第 2 点：`having`能够使用字段别名，`where`则不能**

执行如下 SQL 语句，进行测试：

```
-- 求出表 student 中所有班级人数大于等于 2 的班级
select grade,count(*) as total from student group by grade having total >= 2;
select grade,count(*) as total from student where total >= 2 group by grade;
```

![2](http://img.blog.csdn.net/20170711090554273)

如上图所示，显然咱们的结论得到了验证。究其原因，`where`是从磁盘读取数据，而磁盘中数据的名字只能是字段名，别名是数据（字段）进入到内存后才产生的。值得注意的是，**在上述 SQL 语句中咱们使用了字段别名，这在无意中就优化了 SQL 并提高了效率，因为少了一次统计函数的计算**。


### `order by`子句

`order by`子句：**根据某个字段进行升序或者降序排序，依赖校对集**。

 - **基本语法**：`order by + [asc/desc];`

其中，`asc`为升序，为默认值；`desc`为降序。

执行如下 SQL 语句，进行测试：

```
-- 将表 student 中的数据按年龄 age 进行排序
select * from student order by age;
```

![3](http://img.blog.csdn.net/20170711091902136)

此外，咱们可以进行「**多字段排序**」，即**先根据某个字段进行排序，然后在排序后的结果中，再根据某个字段进行排序**。

执行如下 SQL 语句，进行测试：

```
-- 将表 student 中的数据先按年龄 age 升序排序，再按班级 grade 降序排序
select * from student order by age,grade desc;
```

![4](http://img.blog.csdn.net/20170711093201665)


### `limit`子句

`limit`子句：**是一种限制结果的语句，通常来限制结果的数量**。

 - **基本语法**：`limit + [offset] + length;`

其中，`offset`为起始值；`length`为长度。

**第 1 种：只用来限制长度（数据量）**

执行如下 SQL 语句，进行测试：

```
-- 查询表 student 中的全部记录
select * from student;
-- 查询表 student 中的 3 条记录
select * from student limit 3;
```
![00](http://img.blog.csdn.net/20170711192158394)


**第 2 种：限制起始值，限制长度（数据量）**

执行如下 SQL 语句，进行测试：

```
-- 查询表 student 中的记录
select * from student limit 0,2;
-- 查询表 student 中的记录
select * from student limit 2,2;
```

![01](http://img.blog.csdn.net/20170711192515751)

**第 3 种：主要用来实现数据的分页，目的是为用户节省时间，提高服务器的相应效率，减少资源的浪费**

大致设计：

 - 对于用户来讲，可以通过点击页码按钮，如`1`、`2`、`3`等来进行选择；
 - 对于服务器来讲，可以根据用户选择的页码来获取不同的数据。

其中，

 - `length`：表示每页的数据量，基本不变；
 - `offset`：表示每页的起始值，公式为`offset=(页码-1)*length`.

如果大家感兴趣的话，可以结合`PHP`或者其他语言进行测试。

----------

**温馨提示**：符号`[]`括起来的内容，表示可选项；符号`+`，则表示连接的意思。


----------
———— ☆☆☆ —— [返回 -> 史上最简单的 MySQL 教程 <- 目录](https://github.com/guobinhit/mysql-tutorial/blob/master/README.md) —— ☆☆☆ ————
