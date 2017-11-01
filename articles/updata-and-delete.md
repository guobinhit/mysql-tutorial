# 史上最简单的 MySQL 教程（二十二）「数据的高级操作 之 更新 & 删除」

## 数据的高级操作

### 更新数据

 - **基本语法**： `update + 表名 + set + 字段 = 值 + [where 条件];`
 - **高级语法**： `update + 表名 + set + 字段 = 值 + [where 条件] + [limit 更新数量];`

执行如下 SQL 语句，进行测试：

```
-- 将表 my_copy 中的部分 a 更新为 c
update my_copy set name = 'c' where name = 'a' limit 3;
```

执行上述 SQL 语句前：

![1](http://img.blog.csdn.net/20170625180810447)

执行上述 SQL 语句后：

![2](http://img.blog.csdn.net/20170625180922214)


### 删除数据

与更新类似，可以通过`limit`来限制删除的数量。

 - **基本语法**： `delete + from + 表名 + [where 条件];`
 - **高级语法**： `delete + from + 表名 + [where 条件] + [limit 删除数量];`

执行如下 SQL 语句，进行测试：

```
-- 将表 my_copy 中的部分 b 删除
delete from my_copy where name = 'b' limit 10;
```

![3](http://img.blog.csdn.net/20170625181516023)

此外，在删除记录的过程中，**如果表中存在自增长的主键，那么删除之后，自增长不会还原**。执行如下 SQL 语句，进行测试：

```
-- 删除表 student 中的记录，查看自增长属性
delete from student;
show create table student;
```

![4](http://img.blog.csdn.net/20170625182444031)


如上图所示，显然在咱们删除表`student`中的全部数据之后，`id`的自增长属性值`3`并没有发生改变，这是因为数据的删除是不会改变表结构的。如果想要还原自增长属性，思路是：**先删除表，然后重新建表**。

 - **基本语法**：`truncate + 表名;`

执行如下 SQL 语句，进行测试：

```
-- 先清空 student 表，再重新创建 student 表
truncate student;
```

![5](http://img.blog.csdn.net/20170625183121678)

如上图所示，显然咱们已经测试成功啦！


----------

**温馨提示**：符号`[]`括起来的内容，表示可选项；符号`+`，则表示连接的意思。


----------
———— ☆☆☆ —— [返回 -> 史上最简单的 MySQL 教程 <- 目录](https://github.com/guobinhit/mysql-tutorial/blob/master/README.md) —— ☆☆☆ ————
