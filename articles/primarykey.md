# 史上最简单的 MySQL 教程（十）「列属性 之 主键」

## 主键

主键：`primary key`，**表中主要的键，每张表只能有一个字段（复合主键，可以多个字段）使用此属性，用来唯一的约束该字段里面的数据，不能重复**。

### 增加主键

在 SQL 操作中，有 3 种方法可以给表增加主键，分别为：

**第 1 种**：在创建表的时候，直接在字段之后，添加`primary key`关键字

```
-- 增加主键
create table my_pri1(
	name varchar(20) not null comment '姓名',
	number char(10) primary key comment '学号'
)charset utf8;
```
![create-table-mypri1](https://github.com/guobinhit/mysql-tutorial/blob/master/images/primarykey/create-table-mypri1.png)

如上图所示，此方法的优点是清晰明了，缺点则是只能使用一个字段作为主键。

**第 2 种**：在创建表的时候，在所有的字段之后，使用`primary key(主键字段列表)`来创建主键（如果有多个字段作为主键，则称之为复合主键）

```
-- 复合主键
create table my_pri2(
	number char(10) not null comment '学号',
	course char(10) not null comment '课程编号',
	score tinyint unsigned default 60,
	-- 增加主键限制，学号和课程编号应该是对应的，具有唯一性
	primary key(number,course)
)charset utf8;
```

![create-table-mypri2](https://github.com/guobinhit/mysql-tutorial/blob/master/images/primarykey/create-table-mypri1.png)

**第 3 种**：当表创建完之后，额外追加主键，可以直接追加主键，也可以通过修改表字段的属性追加主键

```
-- 追加主键
create table my_pri3(
	course char(10) not null comment '课程编号',
	name varchar(10) not null comment '课程名称'
)charset utf8;
```
![create-table-mypri3](https://github.com/guobinhit/mysql-tutorial/blob/master/images/primarykey/create-table-mypri1.png)

在这里，追加主键有两种方式，分别为：

- `alter table my_pri3 modify course char(10) primary key comment '课程编号';  -- 不建议使用`

- `alter table my_pri3 add primary key(course);  -- 推荐使用`

![alter-mypri3](https://github.com/guobinhit/mysql-tutorial/blob/master/images/primarykey/alter-mypri3.png)

如上图所示，咱们追加主键成功。不过，想要用此方法，有一个前提，那就是：**表中对应字段的数据是不重复的，即保证唯一性**。

### 主键约束

主键约束，即**主键对应的字符中的数据不允许重复，如果重复，则数据操作（主要是增和改）失败**。

```
-- 主键约束（冲突）测试
insert into my_pri3 values('MATH00123','泛函分析');
insert into my_pri3 values('MATH00123','非线性分析');
```

![insert-mypri3](https://github.com/guobinhit/mysql-tutorial/blob/master/images/primarykey/insert-mypri3.png)

### 更新主键 & 删除主键

对于主键，没有办法直接更新，主键必须先删除，然后才能更新。

 - 基本语法：`alter table + 表名 + drop primary key;`

执行如下 SQL 语句，进行测试：
```
-- 删除主键
alter table my_pri3 drop primary key;
```
![desc-mypri3](https://github.com/guobinhit/mysql-tutorial/blob/master/images/primarykey/desc-mypri3.png)

如上图所示，咱们已经成功删除表`my_pri3`的主键啦！想要增加主键，只需要通过前面讲的 3 种方法即可。

### 主键分类

根据主键的字段类型，咱们可以将主键分为两类，分别为：

 - 业务主键，即使用真实的业务数据作为主键，例如学号、课程编号等等，很少使用；
 - 逻辑主键，即使用逻辑性的字段作为主键，字段没有业务含义，值有没有都没有关系，经常使用。

至此，咱们已经将主键的相关内容讲完啦！


----------

**温馨提示**：符号`+`表示连接的意思。

----------
———— ☆☆☆ —— [返回 -> 史上最简单的 MySQL 教程 <- 目录](https://github.com/guobinhit/mysql-tutorial/blob/master/README.md) —— ☆☆☆ ————
