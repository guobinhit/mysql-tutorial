# 史上最简单的 MySQL 教程（九）「列属性 之 空属性、列描述和默认值」

## 列属性

列属性：实际上，**真正约束字段的是数据类型，但是数据类型的约束比较单一，因此就需要额外的一些约束来保证数据的有效性，这就是列属性。**

列属性有很多，例如：`null`、`not null`、`default`、`primary key`、`unique key`、`auto_increment`和`comment`等。

### 空属性

空属性有两个值，分别为：`null`和`not null`.

虽然默认数据库的字段基本都为空，但是**实际上在真正开发的时候，要尽可能的保证数据不为空，因为空数据没有意义，也没办法参与运算**。

执行如下 SQL 语句，进行演示：

```
-- 空属性演示
create table my_class(
	grade varchar(20) not null,
	room varchar(20) null  -- 显式声明为空，实际上，默认就为空
)charset utf8;
```

![6](http://img.blog.csdn.net/20170523102924169)

### 列描述

列描述：`comment`，表示描述，没有实际含义，**是专门用来描述字段的，其会随着表创建语句自动保存，用来给程序员（数据库管理员）了解数据库使用。**

执行如下 SQL 语句，进行演示：

```
-- 列描述演示
create table my_friend(
	name varchar(20) not null comment '姓名',
	age tinyint not null comment '年龄'
)charset utf8;
```
![8](http://img.blog.csdn.net/20170523120146063)

### 默认值

默认值：`default`，**某一数据会经常性出现某个具体的值，因此可以在开始的时候就指定好，而在需要真实数据的时候，用户可以选择性的使用默认值。**

执行如下 SQL 语句，进行演示：

```
-- 默认值演示
create table my_default(
	name varchar(20) not null,
	age tinyint unsigned default 0,
	gender enum('男','女') default '男'
)charset utf8;
```

![9](http://img.blog.csdn.net/20170523120331562)

如上图所示，在列属性`Default`中已经展示了`age`和`gender`字段的默认值，这说明咱们设置成功啦！接下来，咱们再演示如何使用默认值：

```
-- 演示默认值的使用（即不该对应的字段赋值）
insert into my_default (name) values ('Charies');
insert into my_default values ('Guo',18,default);
```

![10](http://img.blog.csdn.net/20170523121027497)

观察上面的 SQL 语句及执行结果，相信大家已经知道如何使用默认值啦，即不给设置默认值的字段赋值或者用`default`代替相应的字段值。


----------
———— ☆☆☆ —— [返回 -> 史上最简单的 MySQL 教程 <- 目录](https://github.com/guobinhit/mysql-tutorial/blob/master/README.md) —— ☆☆☆ ————
