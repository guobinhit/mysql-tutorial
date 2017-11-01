# 史上最简单的 MySQL 教程（四）「SQL 基本操作 之 库操作」

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

----------

**温馨提示**：用符号`[]`括起来的内容，表示可选项；符号`+`，则表示连接的意思。

----------
———— ☆☆☆ —— [返回 -> 史上最简单的 MySQL 教程 <- 目录](https://github.com/guobinhit/mysql-tutorial/blob/master/README.md) —— ☆☆☆ ————
