# 史上最简单的 MySQL 教程（十二）「列属性 之 唯一键」

## 唯一键

唯一键：每张表往往有多个字段需要具有唯一性，数据不能重复，但是在每张表中，只能有一个主键，因此**唯一键就是用来解决表中多个字段需要具有唯一性的问题**。

唯一键的本质与主键差不多，唯一键默认的允许字段为空，而且可以多个字段为空，因此空字段不参与唯一性的比较。

### 增加唯一键

增加唯一键的方法和主键类似，有 3 种方法，分别为：

**第一种**：在创建表的时候，字段后面直接添加`unique`或者`unique key`关键字

```
-- 增加唯一键
create table my_unique(
	number char(10) unique comment '学号',
	name varchar(20) not null
)charset utf8;
```
![1](http://img.blog.csdn.net/20170524090620434)

**第 2 种**：在所有字段之后，增加`unique key(字段列表)`，可以设置复合唯一键

```
-- 测试唯一键
create table my_unique2(
	number char(10) not null,
	name varchar(20) not null,
	unique key(number)	
)charset utf8;
```
![2](http://img.blog.csdn.net/20170524091144437)

观察上图，咱们可能会发现一个问题，那就是：**咱们设置的唯一键`UNI`，怎么变成了主键`PRI`啊？这是由于当唯一键满足非空条件的时候，其性质就和主键一样啦，因此在表中显示为`PRI`**. 当然，在咱们执行如下 SQL 语句的时候，其就会表现出真正的性质：

```
-- 查看表创建语句
show create table my_unique2;
```
![3](http://img.blog.csdn.net/20170524091651347)

**第 3 种**：在创建表之后，增加唯一键

```
-- 创建未设置唯一键的表
create table my_unique3(
	id int primary key auto_increment,
	number char(10) not null,
	name varchar(20) not null
)charset utf8;
```

![4](http://img.blog.csdn.net/20170524092040004)

如上图所示，表`my_unique3`未设置唯一键。接下来，执行如下 SQL 语句，进行测试：

```
-- 增加唯一键
alter table my_unique3 add unique key(number);
```
![5](http://img.blog.csdn.net/20170524092306849)

如上图所示，咱们已经成功向表中增加唯一键啦！

### 唯一键约束

唯一键与主键本质相同，区别在于：**唯一键允许字段值为空，并且允许多个空值存在**。

```
-- 测试唯一键约束
insert into my_unique values(null,'Charies');
insert into my_unique values(null,'Guo');
```
![6](http://img.blog.csdn.net/20170524093013603)

### 更新唯一键 & 删除唯一键

在表中，更新唯一键的时候，可以不用先删除唯一键，因为表的唯一键允许有多个。

删除唯一键的语法为：

 - 基本语法：`alter table + 表名 + drop index + 索引名字;`

在这里，唯一键默认使用字段名作为索引名。

```
-- 删除唯一键
alter table my_unique3 drop index number;
```
![7](http://img.blog.csdn.net/20170524093806457)

如上图所示，显然咱们已经成功删除表中的唯一键啦！


----------

**温馨提示**：符号`+`表示连接的意思。

----------
———— ☆☆☆ —— [返回 -> 史上最简单的 MySQL 教程 <- 目录](https://github.com/guobinhit/mysql-tutorial/blob/master/README.md) —— ☆☆☆ ————
