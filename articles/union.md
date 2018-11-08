# 史上最简单的 MySQL 教程（二十二）「联合查询」

**联合查询**：`union`，将多次查询（多条`select`语句）的结果，在字段数相同的情况下，在记录的层次上进行拼接。

## 基本语法

联合查询由多条`select`语句构成，每条`select`语句获取的字段数相同，但与字段类型无关。

 - **基本语法**：`select 语句1 + union + [union选项] + select 语句2 + ...;`
  - **union 选项**：与`select`选项一样有两种
     - `all`：无论重复与否，保留所有记录；
     - `distinct`：表示去重，为默认选项。

执行如下 SQL 语句，进行测试：

```
-- 联合查询，默认去重
select * from class
union distinct
select * from class;

-- 联合查询，保留所有记录
select * from class
union all
select * from class;
```

![select-union-class](https://github.com/guobinhit/mysql-tutorial/blob/master/images/union/select-union-class.png)

如上图所示，展示了联合查询的两种形式。特别地，联合查询只要求字段数相同，而跟类型无关。执行如下 SQL 语句，进行测试：

```
-- 联合查询，字段数相同，类型不同
select id, grade, room from class
union distinct
select name, age, id from student;
```

![select-union-distinct](https://github.com/guobinhit/mysql-tutorial/blob/master/images/union/select-union-distinct.png)

如上图所示，联合查询只保留了第一张表的字段，而不保留第二张表的字段。

## 意义

联合查询的意义有两种，分别为：

 - 查询同一张表，按时需要不同，例如查询学生信息，要求男生按年龄升序排序，女生按年龄降序排序；
 - 多表查询，多张表的结构是完全一样的，保持的数据结构也是一样的。

此外，如果数据量非常的大，就要进行分表（垂直分表和水平分表），而分表的依据无外乎数据多不多和常不常用。

## 排序

首先，让我们看看`student`表中的数据：

![select-student](https://github.com/guobinhit/mysql-tutorial/blob/master/images/union/select-student.png)

接下来，给出一个需求：**在`student`表中，让男生按年龄升序排序，让女生按年龄降序排序**。根据我们刚刚学到的联合查询，貌似很容易啊！执行如下 SQL 语句，进行测试：

```
-- 在 student 表中，按年龄，男升女降
select * from student where gender = "boy" order by age asc
union
select * from student where gender = "girl" order by age desc;
```

![select-union-error](https://github.com/guobinhit/mysql-tutorial/blob/master/images/union/select-union-error.png)

如上图所示，呃，好吧，貌似出错啦！这是因为要想在联合查询中使用`order by`，我们必须将`select`语句用括号括起来。执行如下 SQL 语句，进行测试：

```
-- 在 student 表中，按年龄，男升女降
(select * from student where gender = "boy" order by age asc)
union
(select * from student where gender = "girl" order by age desc);
```

![union-order](https://github.com/guobinhit/mysql-tutorial/blob/master/images/union/union-order.png)

如上图所示，啊，我们都已经将`select`语句用括号括起来了，但是男生和女生也没有按我们的目的实现啊！好吧，这是因为要想在联合查询中使`order by`生效，我们必须将其与`limit`搭配使用，而`limit`的限定数，我们设置为一个非常大的数即可。执行如下 SQL 语句，进行测试：

```
-- 在 student 表中，按年龄，男升女降
(select * from student where gender = "boy" order by age asc limit 666)
union
(select * from student where gender = "girl" order by age desc limit 666);
```

![union-order-2](https://github.com/guobinhit/mysql-tutorial/blob/master/images/union/union-order-2.png)

如上图所示，显然，这次咱们的目的实现啦！

----------

**温馨提示**：符号`[]`括起来的内容，表示可选项；符号`+`，则表示连接的意思。


----------
———— ☆☆☆ —— [返回 -> 史上最简单的 MySQL 教程 <- 目录](https://github.com/guobinhit/mysql-tutorial/blob/master/README.md) —— ☆☆☆ ————
