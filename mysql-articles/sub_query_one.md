# 史上最简单的 MySQL 教程（三十一）「子查询（上）」

**子查询**：`sub query`，查询是在某个查询结果之上进行的，一条`select`语句内部包含了另外一条`select`语句。

## 分类

子查询有两种分类方式，分别为：按按结果分类和位置分类。

**按结果分类**，即根据子查询得到的数据进行分类（理论上，任何一个查询结果都可以理解为一个二维表），分别为：

 - 标量子查询：子查询得到的结果是一行一列，出现的位置在`where`之后；
 - 列子查询：子查询得到的结果是一列多行，出现的位置在`where`之后；
 - 行子查询：子查询得到的结果是多行一列（多行多列），出现的位置在`where`之后；
 - 表子查询：子查询得到的结果是多行多列，出现的位置在`from`之后。


**按位置分类**，即根据子查询（`select`语句）在外部查询（`select`语句）中出现的位置进行分类，分别为：

 - `from`子查询：子查询出现在`from`之后；
 - `where`子查询：子查询出现在`where`条件之中；
 - `exists`子查询：子查询出现在`exists`里面。

## 标量子查询

**需求**：现知道班级名称为`PM3.1`，想要获取该班的全部学生。

**思路**：

 - 先确定数据源，学生表。
  - `select * from student where c_id = ?;`
 - 然后获取班级 ID，可以通过（班级表）班级名称来确定。
  - `select id from class where grade = "PM3.1";`

执行如下 SQL 语句，进行测试：

```
-- 标量子查询
select * from student where c_id = (select id from class where grade = "PM3.1");
```

![biaol](http://img.blog.csdn.net/20170826223108914)

## 列子查询

**需求**：查询所有在读班级（学生表中存在的班级）的学生。

**思路**：

 - 先确定数据源，学生表。
  - `select * from student where c_id in ?;`
 - 然后确定全部有效的班级 ID。
  - `select id from class;`

执行如下 SQL 语句，进行测试：

```
-- 列子查询
select * from student where c_id in (select id from class);
```

![column](http://img.blog.csdn.net/20170826231655784)

如上图所示，我们完成了列子查询。在列子查询的结果为一行多列时，我们需要使用`in`作为条件进行匹配；此外，在 MySQL 中还有三个类似的条件，分别为：`all`、`some`和`any`。

 - `any`等价于`in`，表示其中一个；
 - `any`等价于`smoe`，而`any`和`some`用于否定时却有些区别；
 - `all`表示等于全部。

值得注意的是，在我们使用上面三个关键字中任何一个的时候，都需要搭配`=`使用，例如：

```
-- 列子查询
select * from student where c_id = any (select id from class);

select * from student where c_id = some (select id from class);

select * from student where c_id = all (select id from class);
```

![ass](http://img.blog.csdn.net/20170826231616645)

如上图所示，为`any`、`some`和`all`的肯定用法，下面我们来测试其否定用法：

```
-- 列子查询
select * from student where c_id != any (select id from class);

select * from student where c_id != some (select id from class);

select * from student where c_id != all (select id from class);
```

![fouze](http://img.blog.csdn.net/20170826231526787)

观察上图，我们会发现`any`、`some`和`all`在用于否定时，其会将`null`值排除掉。实际上，在真正的开发中，这三个关键字并不常用。

----------
**查询`class`表**：

![class](http://img.blog.csdn.net/20170826223402050)

**查询`student`表**：

![studnet](http://img.blog.csdn.net/20170826231758769)


----------

**温馨提示**：符号`[]`括起来的内容，表示可选项；符号`+`，则表示连接的意思。


----------
———— ☆☆☆ —— [返回 -> 史上最简单的 MySQL 教程 <- 目录](https://github.com/guobinhit/mysql-tutorial/blob/master/README.md) —— ☆☆☆ ————
