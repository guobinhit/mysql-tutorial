# 史上最简单的 MySQL 教程（二十四）「数据的高级操作 之 查询（中）」

## 数据的高级操作 - 查询数据（中）

### `where`子句

`where`字句：**用来判断数据和筛选数据，返回的结果为`0`或者`1`，其中`0`代表`false`，`1`代表`true`**，`where`是唯一一个直接从磁盘获取数据的时候就开始判断的条件，从磁盘中读取一条数据，就开始进行`where`判断，如果判断的结果为真，则保持，反之，不保存。

**判断条件**：

 - 比较运算符：`>`、`<`、`>=`、`<=`、`<>`、`=`、`like`、`between and`、`in`和`not in`；
 - 逻辑运算符：`&&`、`||`、和`!`.


执行如下 SQL 语句，进行测试：

```
-- 查询表 student 中 id 为 2、3、5 的记录
select * from student where id = 2 || id = 3 || id = 5;
select * from student where id in (2,3,5); 
```

![001](http://img.blog.csdn.net/20170627210129189)

```
-- 查询表 student 中 id 在 2 和 5 之间的记录
select * from student where id between 2 and 5;
```

![002](http://img.blog.csdn.net/20170627210410567)

如上图所示，咱们会发现：**在使用`between and`的时候，其选择的区间为闭区间，即包含端点值**。此外，`and`前面的数值必须大于等于`and`后面的数值，否则会出现空判断，例如：

![003](http://img.blog.csdn.net/20170627210829379)


### `group by`子句

`group by`子句：**根据表中的某个字段进行分组，即将含有相同字段值的记录放在一组，不同的放在不同组**。

 - **基本语法**：`group by + 字段名;`

执行如下 SQL 语句，进行测试：

```
-- 将表 student 中的数据按字段 sex 进行分组
select * from student group by sex;
```

![004](http://img.blog.csdn.net/20170627211555446)

观察上图，咱们会发现：表`student`在分组过后，数据“丢失”啦！实际上并非如此，产生这样现象原因为：**`group by`分组的目的是为了（按分组字段）统计数据，并不是为了单纯的进行分组而分组**。为了方便统计数据，SQL 提供了一系列的统计函数，例如：

 - `cout()`：统计分组后，每组的总记录数；
 - `max()`：统计每组中的最大值；
 - `min()`：统计每组中的最小值；
 - `avg()`：统计每组中的平均值；
 - `sum()`：统计每组中的数据总和。

执行如下 SQL 语句，进行测试：

```
-- 将表 student 中的数据按字段 sex 进行分组，并进行统计
select sex,count(*),max(age),min(age),avg(age),sum(age) from student group by sex;
```
![005](http://img.blog.csdn.net/20170627212554643)

其中，`count()`函数里面可以使用两种参数，分别为：`*`表示统计组内全部记录的数量；`字段名`表示统计对应字段的非`null`（如果某条记录中该字段的值为`null`，则不统计）记录的总数。此外，使用`group by`进行分组之后，展示的记录会根据分组的字段值进行排序，默认为升序。当然，也可以人为的设置升序和降序。

 - **基本语法**：`group by + 字段名 + [asc/desc];`

执行如下 SQL 语句，进行测试：

```
-- 将表 student 中的数据按字段 sex 进行分组，并排序
select sex,count(*) from student group by sex;
select sex,count(*) from student group by sex asc;
select sex,count(*) from student group by sex desc;
```
![006](http://img.blog.csdn.net/20170627213601017)

通过观察上面数个分组示例，细心的同学会发现：咱们在之前的示例中，都是用单字段进行分组。实际上，咱们也可以使用**多字段分组**，即：**先根据一个字段进行分组，然后对分组后的结果再次按照其他字段（前提是分组后的结果中包含此字段）进行分组**。

执行如下 SQL 语句，进行测试：

```
-- 将表 student 中的数据先按字段 grade 进行分组，再按字段 sex 进行分组
select *,count(*) from student group by grade,sex;
```

![007](http://img.blog.csdn.net/20170627214720585)

在这里，函数`group_concat(字段名)`可以对分组的结果中的某个字段值进行字符串连接，即保留该组某个字段的所有值。例如：

```
-- 将表 student 中的数据按字段 sex进行分组，并保留字段 name 的值
select sex,age,count(*),group_concat(name) from student group by sex;
```
![008](http://img.blog.csdn.net/20170627215231647)

此外，简单介绍**回溯统计**的概念：**利用`with rollup`关键字（书写在 SQL 语句末尾），可以在每次分组过后，根据当前分组的字段进行统计，并向上一级分组进行汇报**。例如：

```
-- 将表 student 中的数据按字段 sex进行分组，并进行回溯统计
select sex,count(*) from student group by sex with rollup;
```
![009](http://img.blog.csdn.net/20170627220823491)

观察上图，咱们会发现：**在进行回溯统计的时候，会将分组字段置空**。





----------

**温馨提示**：符号`[]`括起来的内容，表示可选项；符号`+`，则表示连接的意思。


----------
———— ☆☆☆ —— [返回 -> 史上最简单的 MySQL 教程 <- 目录](https://github.com/guobinhit/mysql-tutorial/blob/master/README.md) —— ☆☆☆ ————
