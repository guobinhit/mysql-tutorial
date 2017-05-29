# 史上最简单的 MySQL 教程（十五）「列属性 之 自动增长」
## 自动增长

自动增长：`auto_increment`，**当对应的字段，不给值，或者是默认值，或者是`null`的时候，就会自动的被系统触发，系统会从当前字段中取已有的最大值再进行`+1`操作，得到新的字段值**。

自增长通过跟主键进行搭配使用，其特点为：

 - 任何字段要做自增长，前提其本身必须是一个索引，即`key`栏有值；
 - 自增长字段必须是数字（整型）；
 - 每张表最多有一个自增长字段。

执行如下 SQL 语句，进行测试：

```
-- 自增长示例
create table my_auto(
	id int primary key auto_increment,
	name varchar(20) not null
)charset utf8;
```

![1](http://img.blog.csdn.net/20170523225451048)

### 使用自增长

当自增长给定的值为`null`或者默认值的时候，就是触发自动增长。

```
-- 触发自增长
insert into my_auto (name) values('Charies');
insert into my_auto values(null,'Guo');
insert into my_auto values(default,'ChariesGuo');
```
![2](http://img.blog.csdn.net/20170523230214445)

如上图所示，自增长的第一个元素默认是`1`，自增长每次都是自增`1`。此外，如果自增长对应的字段输入了值，那么自增长失效，但是下一次还是能够正确的自增长，即从取该字段的最大值`+1`.

那么，如何确定下一次自增长的数值是什么呢？简单，咱们可以通过查看表的创建语句看到。

```
-- 查看表的创建语句
show create table my_auto;
```
![3](http://img.blog.csdn.net/20170523230908501)

如上图所示，显然在咱们之前插入的 3 条记录之后，下一次自增长是`4`，符合咱们的结论。

### 修改自增长

自增长如果是涉及到字段改变，就必须先删除自增长，然后再增加自增长，因为每张表只能有一个自增长字段。

如果修改当前自增长字段已经存在的值，则只能修改比当前已有自增长字段中的最大值更大，不能更小，因为更小不生效。

 - 基本语法：`alter table + 表名 + auto_increment = 值;`

执行如下 SQL 语句，进行测试：

```
-- 修改自增长测试
alter table my_auto auto_increment = 2;
```

![4](http://img.blog.csdn.net/20170523233034847)

如上图所示，当咱们修改自增长小于当前自增长字段中的最大值时，虽然显示 SQL 语句执行成功，但实际上并没有修改成功。下面，在执行如下 SQL 语句，进行测试：

```
-- 修改自增长测试
alter table my_auto auto_increment = 5;
```

![5](http://img.blog.csdn.net/20170523233250704)

如上图所示，显然当咱们修改自增长大于当前自增长字段中的最大值时，修改成功。

在这里，咱们不妨思考一下，为什么自增长是从`1`开始呢？为什么每次都自增`1`呢？虽然现在咱们不知道是什么原因导致上面的表示形式，但是咱们知道，所有系统的表现（如字符集、校对集）都是由系统内部的变量进行控制的，因此咱们可以查看自增长对应的变量：

 - 基本语法：`show variables like 'auto_increment%';`

```
-- 查看自增长变量
show variables like 'auto_increment%';
```
![6](http://img.blog.csdn.net/20170523234155133)

如上图所示，其中`auto_increment_increment`表示`步长`，`auto_increment_offset`表示`初始值`.

因此，咱们可以通过修改上面的两个变量实现不同的效果，但是需要注意的是：**修改是会话级别，并且修改的是整个数据库，而不是单张表。**

 - 基本语法：`set auto_increment_increment = 值;`

执行如下 SQL 语句，进行测试：

```
-- 修改自增长步长
set auto_increment_increment = 5;
insert into my_auto values (null,'Guobinhit');
```
![7](http://img.blog.csdn.net/20170523234752168)

如上图所示，咱们已经修改成功！接下来，执行如下 SQL 语句，进行测试：
```
-- 插入测试数据
insert into my_auto values (null,'Guobinhit');
insert into my_auto values (default,'Guobinhit');
```

![8](http://img.blog.csdn.net/20170523235047044)

如上图所示，咱们插入的第一个`Guobinhit`的`id`值竟然是`6`，第二个`Guobinhit`的`id`值倒是在`6`的基础上增加了`5`为`11`正常。这个现象产生的原因是：咱们之前的修改会导致系统产生一个误差，从初始值`1`开始计算加`5`，因此第一个`Guobinhit`的`id`值是`6`. 

不过说实话，修改自增长并没有什么实际的意义，在此只是想让大家知道：自增长的初始值和步长都是可以修改的而已。

### 删除自增长

自增长是字段的一个属性，因此可以通过`modify`来进行修改。想要删除自增长的话，**只需要保证字段没有`auto_increment`即可**。

 - 基本语法：`alter table + 表名 + modify + 字段 + 类型;`

执行如下 SQL 语句，进行测试：

```
-- 删除自增长
alter table my_auto modify id int primary key;
```
![9](http://img.blog.csdn.net/20170524000518389)

如上图所示，呃，好吧，出错啦！这是因为表`my_auto`之前已经定义了主键，所以在执行上述 SQL 语句的时候，系统会认为咱们想要定义多个主键，自然就要报错啦！解决方法就是，去掉上述 SQL 语句中的`primary key`即可。

![10](http://img.blog.csdn.net/20170524000826628)

如上图所示，显然咱们已经成功删除自增长啦！

----------

**温馨提示**：符号`+`表示连接的意思。

----------
———— ☆☆☆ —— [返回 -> 史上最简单的 MySQL 教程 <- 目录](http://blog.csdn.net/qq_35246620/article/details/70823903) —— ☆☆☆ ————
