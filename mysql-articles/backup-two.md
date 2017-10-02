# 史上最简单的 MySQL 教程（三十六）「数据备份与还原（中）」

# 数据备份与还原

基础概念：

- **备份**，将当前已有的数据或记录另存一份；

- **还原**，将数据恢复到备份时的状态。

为什么要进行数据的备份与还原？

- 防止数据丢失；
- 保护数据记录。

数据备份与还原的方式有很多种，具体可以分为：数据表备份、单表数据备份、SQL备份和增量备份。

## 单表数据备份

单表数据备份，每次只能备份一张表，而且只能备份数据，不能备份表结构。

通常的使用场景为：将表中的数据导出到文件。

备份方法：从表中选出一部分数据保存到外部的文件中，

- `select */字段列表 + into outfile + '文件存储路径' + from 数据源;`

在这里，使用单表数据备份有一个前提，那就是：**导出的外部文件不存在，即文件存储路径下的文件不存在**。

执行如下 SQL 语句，进行测试：

```
-- 单表数据备份
select * into outfile 'D:/CoderLife/testMySQL/class.txt' from class;
```

![1](http://img.blog.csdn.net/20171001185200857)

如上图所示，SQL 语句已经执行成功。在这里，如果我们遇到：

> **ERROR 1290 (HY000)**: The MySQL server is running with the –secure-file-priv option so it cannot execute this statement.

这个错误，可以通过查看「[详述 MySQL 导出数据遇到 secure-file-priv 的问题](https://github.com/guobinhit/mysql-tutorial/blob/master/mysql-articles/secure.md)」进行解决。

为了验证是否真的将`class`表中的数据导出到指定位置，我们可以到该路径下进行确认：

![2](http://img.blog.csdn.net/20171001185637160)

如上图所示，显然我们已经将`class`表中的数据导出到本地啦！不过在这里，有一点需要我们特别注意，那就是：**对于从数据库导出的文件，我们最好用`EditPlus`等编辑工具打开，防止乱码**。

此外，对于上述用于导出表中数据的 SQL 语法，其实我们可以颠倒书写顺序，也没有问题，例如：

- `select */字段列表 + from 数据源 + into outfile + '文件存储路径';`

执行如下 SQL 语句，进行测试：

```
-- 单表数据备份
select * from class into outfile 'D:/CoderLife/testMySQL/class2.txt';
```

![3](http://img.blog.csdn.net/20171001190506222)

接下来，我们学习一些用于单表数据备份的高级操作，即**自己指定字段和行的处理方式**。

- **基本语法**：`select */字段列表 + into outfile + '文件存储路径' + fields + 字段处理 + lines + 行处理 + from 数据源;`

字段处理：

- `enclosed by`：指定字段用什么内容包裹，默认是` `，空字符串；
- `terminated by`：指定字段以什么结束，默认是`\t`，`Tab`键；
- `escaped by`：指定特殊符号用什么方式处理，默认是`\\`，反斜线转义。

行处理：

- `starting by`：指定每行以什么开始，默认是` `，空字符串；
-  `terminated by`：指定每行以什么结束，默认是`\r\n`，换行符。

执行如下 SQL 语句，进行测试：

```
-- 指定单表数据备份处理方式
select * into outfile 'D:/CoderLife/testMySQL/class3.txt'
-- 字段处理
fields
enclosed by '"'
terminated by '|'
lines
starting by 'START:'
from class ;
```

![4](http://img.blog.csdn.net/20171001192031438)

如上图所示，显然导出文件`class3.txt`按照我们指定的格式进行输出啦！在前面，我们已经测试了各种单表数据备份的方式，现在我们删除数据，并尝试还原数据，即**将保持在外部的数据重新恢复到数据表中**。But，由于单表数据备份进能备份数据，因此如果表结构不存在，则不能进行还原。

- **基本语法**：`load data infile + '文件存储路径' + into table + 表名 + [字段列表] + fields + 字段处理 + lines + 行处理;`

执行如下 SQL 语句，进行测试：

```
-- 删除表 class 中的数据
delete from class;

-- 查看表 class 中的数据
select * from class;

-- 还原表 class 中的数据
load  data infile 'D:/CoderLife/testMySQL/class3.txt'
into table class
-- 字段处理
fields
enclosed by '"'
terminated by '|'
lines
starting by 'START:';

-- 查看表 class 中的数据
select * from class;
```

![5](http://img.blog.csdn.net/20171001193445728)

如上图所示，显然在我们删除表`class`中的数据之后，还原数据成功。

----------

**温馨提示**：符号`[]`括起来的内容，表示可选项；符号`+`，则表示连接的意思。


----------
———— ☆☆☆ —— [返回 -> 史上最简单的 MySQL 教程 <- 目录](https://github.com/guobinhit/mysql-tutorial/blob/master/README.md) —— ☆☆☆ ————
