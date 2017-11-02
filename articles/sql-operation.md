# 史上最简单的 MySQL 教程（四）「SQL 基本操作」

SQL 基本操作
========

基本操作：CURD，即**增删改查**。

根据操作对象的不同，咱们可以将 SQL 的基本操作分为三类，分别为：库操作、表（字段）操作和数据操作。

库操作
---
**1 新增数据库**

基本语法：`create database + 数据库名称 + [库选项];`

其中，库选项是用来约束数据库的，为可选项（有默认值），共有两种，分别为：

 - 字符集设定：`charset/ character set`+ 具体字符集，用来表示数据存储的编码格式，常用的字符集包括`GBK`和`UTF8`等。
 - 校对集设定：`collate`+ 具体校对集，表示数据比较的规则，其依赖字符集。

示例：`create database TBL_ERROR_CODE charset utf8;`

其中，数据库的名字不能用关键字（已经被占用的字符，例如 update 和 insert 等）或者保留字（将来可能会用的，例如 access 和 cast 等）。

如果非要使用数据库的关键字或者保留字作为数据库名称，那么必须用反引号将其括起来，例如：

`create database `access` charset utf8;`

如果还想使用中文作为数据库的名称，那就得保证数据库能够识别中文（强烈建议不要用中文命名数据库的名称），例如：

```
-- 设置中文名称的方法，其中 gbk 为当前数据库的默认字符集
set names gbk;
create database 北京 charset gbk;
```

![1](http://img.blog.csdn.net/20170503130944273)

**2 查询数据库**

查看全部 --> 基本语法：`show databases;`

查看部分（模糊查询）--> 基本语法：`show databases like 'pattern';` 

其中，`pattern`是匹配模式，有两种，分别为：

 - `%`：表示匹配多个字符；
 - `_`：表示匹配单个字符。

此外，在匹配含有下划线`_`的数据库名称的时候，需要在下划线前面加上反斜线`\_`进行转义操作。

示例：`show databases like 'TBL%';`表示匹配所有`TBL`开头的数据库。

查看数据库的创建语句 --> 基本语法：`show create database + 数据库名称;`

在这里，查看的结果有可能与咱们书写的 SQL 语句不同，这是因为数据库在执行 SQL 语句之前会优化 SQL，系统保存的是优化后的结果。

**3 更新数据库**

在这里，需要注意：数据库的名字不可以修改。

数据库的修改仅限库选项，即字符集和校对集（校对集依赖字符集）。

基本语法：`alter database + 数据库名称 + [库选项];`

 - `charset/character set[=] 字符集;`
 - `collate[=] 校对集;`

示例：`alter database TBL_ERROR_CODE charset gbk;`表示修改此数据库的字符集为`gbk`.

**4 删除数据库**

基本语法：`drop database + 数据库名称;`

在这里，需要注意：在删除数据库之前，应该先进行备份操作，因为删除为不可逆操作，所以不要随意删除数据库。

表操作
---
**1 新增表**

基本语法：
```
create table [if not exists] + 表名(
	字段名称 数据类型,
	……
	字段名称 数据类型	/* 最后后一行，不需要加逗号 */
)[表选项];
```

其中，`if not exists`表示

 - 如果表名不存在，就执行创建代码；如果表名存在，则不执行创建代码。

表选项则是用来控制表的表现形式的，共有三种，分别为：

 - 字符集设定：`charset/ character set`+ 具体字符集，用来表示数据存储的编码格式，常用的字符集包括`GBK`和`UTF8`等。
 - 校对集设定：`collate`+ 具体校对集，表示数据比较的规则，其依赖字符集。
 - 存储引擎：`engine`+具体存储引擎，默认为`InnoDB`，常用的还有`MyISAM`.

由于任何表都归属于某个数据库，因此在创建表的时候，都必须先指定具体的数据库。在这里，指定数据库的方式有两种，分别为：

 - **第 1 种**：显式的指定表所属的数据库，示例

```
create table if not exists test.student(
	name varchar(10),
	age int,            /* 整型不需要指定具体的长度 */
	grade varchar(10)	/* 最后后一行，不需要加逗号 */
)charset utf8;
```
 - **第 2 种**：隐式的指定表所属的数据库，示例

```
use test;				/* use + 数据库名称，表示切换到指定的数据库，这句命令其实不加分号也可以，但不建议这么做 */
create table if not exists student(
	name varchar(10),
	age int,            /* 整型不需要指定具体的长度 */
	grade varchar(10)	/* 最后后一行，不需要加逗号 */
)charset utf8;
```

**2 查询表**

查看全部 --> 基本语法：`show tables;`

查看部分（模糊查询）--> 基本语法：`show tables like 'pattern';` 

其中，`pattern`是匹配模式，有两种，分别为：

 - `%`：表示匹配多个字符；
 - `_`：表示匹配单个字符。

此外，在匹配含有下划线`_`的表名的时候，需要在下划线前面加上反斜线`\_`进行转义操作。

示例：`show tables like '%t';`表示匹配所有以`t`结尾的表。

查看表的创建语句 --> 基本语法：`show create table + 表名;`

在这里，咱们也可以用`\g`和`\G`代替上述语句中的`;`分号，其中`\g`等价于分号，`\G`则在等价于分号的同时，将查的表结构旋转`90`度，变成纵向结构。

查看表中的字段信息 --> 基本语法：`desc/describe/show columns from + 表名;`

**3 更新表**

在这里，需要注意：表的修改，分为修改表本身和修改表中的字段。

 - **第 1 类**：修改表本身
	 - 修改表名，基本语法：`rename table 旧表名 to 新表名;`
	 - 修改表选项，基本语法：`alter table + 表名 + 表选项[=] + 值;`

 - **第 2 类**：修改表中的字段，新增、修改、重命名和删除
	 - 新增字段，基本语法：`alter table + 表名 + add + [column] + 字段名 + 数据类型 + [列属性][位置];`
		 - 其中，位置表示此字段存储的位置，分为`first（第一个位置）`和`after + 字段名（指定的字段后，默认为最后一个位置）`.
		 - 示例：`alter table student add column id int first;`
	 - 修改字段，基本语法：`alter table + 表名 + modify + 字段名 + 数据类型 + [列属性][位置];`
		 - 其中，位置表示此字段存储的位置，分为`first（第一个位置）`和`after + 字段名（指定的字段后，默认为最后一个位置）`.
		 - 示例：`alter table student modify name char(10) after id;`
	 - 重命名字段，基本语法：`alter table + 表名 + change + 旧字段名 + 新字段名 + 数据类型 + [列属性][位置];`
		 - 其中，位置表示此字段存储的位置，分为`first（第一个位置）`和`after + 字段名（指定的字段后，默认为最后一个位置）`.
		 - 示例：`alter table student change grade class varchar(10);`
	 - 删除字段，基本语法：`alter table + 表名 + drop+ 字段名;`
		 - 示例：`alter table student drop age;`
		 - 注意：如果表中已经存在数据，那么删除该字段会清空该字段的所有数据，而且不可逆，慎用。

**4 删除表**

基本语法：

```
-- 可以一次删除多张表
drop table + 表1, 表2 ... ;	
```

在这里，需要注意：此删除为不可逆操作，希望大家谨慎使用。


数据操作
---
**1 新增数据**

对于数据的新增操作，有两种方法。

 - **第 1 种**：给全表字段插入数据，不需要指定字段列表，但要求数据的值出现的顺序必须与表中的字段出现的顺序一致，并且凡是非数值数据，都需要用引号（建议使用单引号）括起来。
	 - 基本语法：`insert into + 表名 + values(值列表)[,(值列表)];`
	 - 示例：`insert into test valus('charies',18,'3.1');`
 - **第 2 种**：给部分字段插入数据，需要选定字段列表，字段列表中字段出现的顺序与表中字段的顺序无关，但值列表中字段值的顺序必须与字段列表中的顺序保持一致。
	 - 基本语法：`insert into + 表名(字段列表) + values(值列表)[,(值列表)];`
	 - 示例：`insert into test(age,name) valus(18,'guo');`


**2 查询数据**

查看全部 --> 基本语法：`select * from + 表名 + [where 条件];`

 - 示例：`select * from test`;

查看部分 --> 基本语法：`select + 字段名称[,字段名称] + from + 表名 + [where 条件];`

 - 示例：`select name,age,grade from test where age = '18'`;


**3 更新数据**

基本语法：`update + 表名 + set + 字段 = 值 + [where 条件];`

 - 示例：`update test set age = 20 where name = 'guo';`

在这里，建议尽量加上`where`条件，否则的话，操作的就是全表数据。

此外，判断更新操作是否成功，并不是看 SQL 语句是否执行成功，而是看是否有记录受到影响，即`affected`的数量大于`1`时，才是真正的更新成功。

**4 删除数据**

基本语法：`delete from + 表名 + [where 条件];`

 - 示例：`delete from test where grade = '3.1';`


当然，我们也可以用`drop`来实现删除操作，不过与`delete`相比，`drop`的威力更强，其在执行删除操作的时候，不仅会删除数据，还会删除定义并释放存储空间；而`delete`在执行删除操作的时候，仅会删除数据，并不会删除定义和释放存储空间。

----------

**温馨提示**：用符号`[]`括起来的内容，表示可选项；符号`+`，则表示连接的意思。

----------
———— ☆☆☆ —— [返回 -> 史上最简单的 MySQL 教程 <- 目录](https://github.com/guobinhit/mysql-tutorial/blob/master/README.md) —— ☆☆☆ ————
