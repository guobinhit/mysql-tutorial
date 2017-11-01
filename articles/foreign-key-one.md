# 史上最简单的 MySQL 教程（二十八）「外键（上）」

## 外键

**外键**：`foreign key`，外面的键，即不在自己表中的键。如果一张表中有一个非主键的字段指向另外一张表的主键，那么将该字段称之为外键。每张表中，可以有多个外键。

### 新增外键

外键既可以在创建表的时候增加，也可以在创建表之后增加（但是要考虑数据的问题）。

**第 1 种：在创建表的时候，增加外键**

 - **基本语法**：`foreign key(外键字段) + references + 外部表名(主键字段);`

执行如下 SQL 语句，进行测试：

```
-- 创建外键
create table my_foreign1(
	id int primary key auto_increment,
	name varchar(20) not null comment '学生姓名',
	c_id int comment '班级表ID',
	-- 增加外键
	foreign key(c_id) references class(id)	
)charset utf8;
```

![foreign1](http://img.blog.csdn.net/20170819224902221)

观察上图可知，字段`c_id`的`key`显示为`MUL`，表示多个键的意思。这是因为外键要求字段本身是一个索引（普通索引）如果字段本身没有索引，外键就会先创建一个索引，然后才创建外键本身。此外，`CONSTRAINT`后面的`my_foreign_ibfk_1`表示外键的名字。

**第 2 种：在创建表之后，增加外键**

 - **基本语法**：`alter table + 表名 + add[constraint + 外键名字] + foreign key(外键字段) + references + 外部表名(主键字段);`

执行如下 SQL 语句，进行测试：

```
-- 创建外键
create table my_foreign2(
	id int primary key auto_increment,
	name varchar(20) not null comment '学生姓名',
	c_id int comment '班级表ID'
)charset utf8;

-- 增加外键
alter table my_foreign2 add
-- 指定外键名
constraint test_foreign
-- 指定外键字段
foreign key(c_id)
-- 引用外部表主键
references class(id);
```

![foreign2](http://img.blog.csdn.net/20170819230041629)

如上图所示，显然咱们已经增加外键成功啦！



### 修改外键 & 删除外键

外键不能修改，只能先删除后增加。

 - **基本语法**：`alter table + 表名 + drop foreign key + 外键名字;`

执行如下 SQL 语句，进行测试：

```
-- 删除外键
alter table my_foreign1 drop foreign key my_foreign1_ibfk_1;
```

![foreign3](http://img.blog.csdn.net/20170819231014775)

观察上图可知，删除外键不能通过查看表结构来体现，而是应该通过创建表的语句来查看。

----------

**温馨提示**：符号`[]`括起来的内容，表示可选项；符号`+`，则表示连接的意思。


----------
———— ☆☆☆ —— [返回 -> 史上最简单的 MySQL 教程 <- 目录](https://github.com/guobinhit/mysql-tutorial/blob/master/README.md) —— ☆☆☆ ————
