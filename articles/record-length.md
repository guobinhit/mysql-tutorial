# 史上最简单的 MySQL 教程（八）「记录长度」

##  记录长度

MySQL 中规定：**任何一条记录最长不超过 65535 个字节，这意味着`varchar`永远达不到理论最大值**。

那么，`varchar`实际存储长度能达到多大呢？这由编码字符集决定。

下面，以`varchar`在`UTF-8`和`GBK`的情况为例，执行如下 SQL 语句，进行演示：

```
-- 求出 varchar 在 utf8 和 gbk 字符集下的实际最大值
create table my_utf8(
	name varchar(65535)
)charset utf8;

create table my_gbk(
	name varchar(65535)
)charset gbk;
```

![1](http://img.blog.csdn.net/20170522161909823)

观察上面的结果，发现咱们定义的字段`name`的长度超过限制啦，并且提示了其在`utf8`和`gbk`字符集下各自的最大值。那么，咱们修改 SQL 语句如下，并再次执行：

```
-- 求出 varchar 在 utf8 和 gbk 字符集下的实际最大值
create table my_utf8(
	name varchar(21845)
)charset utf8;

create table my_gbk(
	name varchar(32767)
)charset gbk;
```

![2](http://img.blog.csdn.net/20170522162320934)

观察上面的执行结果，好吧，仍然在报错，为什么呢？观察如下 SQL 语句，并执行：

```
-- 求出 varchar 在 utf8 和 gbk 字符集下的实际最大值
create table my_utf8(
	name varchar(21844)  -- 21844 * 3 + 2 = 65534
)charset utf8;

create table my_gbk(
	name varchar(32766)  -- 32766 * 2 + 2 = 65534
)charset gbk;
```

![3](http://img.blog.csdn.net/20170522163928644)

如上图所示，咱们已经创建成功啦！至于什么定义字段`name`的长度为`21844`和`32766`是由于：

 - `21845 * 3 + 2 = 65537 > 65535`
 - `32767 * 2 + 2 = 65536 > 65535`

因此，在提示的最大值的基础上各自减`1`. 至于，为什么还要加`2`，则是因为`varchar`为变长字符串，在其定义的时候，也就是说在分配存储空间的时候，都会自动多分配`1`到`2`个字节空间，因为咱们想要算最大的存储范围，所以加`2`.

在这里，细心的同学会发现一个问题，那就是：在咱们创建表`my_utf8`和`my_gbk`的时候，咱们仅用了`65534`个字节，还剩余一个字节。现在，如果咱们想要将`65535`个字节都用了，怎么办呢？好说，增加一个`tinyint`类型的字段即可：

```
-- 求出 varchar 在 utf8 和 gbk 字符集下的实际最大值
create table my_utf81(
	stuno tinyint,       -- 1
	name varchar(21844)  -- 21844 * 3 + 2 = 65534
)charset utf8;

create table my_gbk1(
	stuno tinyint,       -- 1
	name varchar(32766)  -- 32766 * 2 + 2 = 65534
)charset gbk;
```

![4](http://img.blog.csdn.net/20170522165051589)

观察上面的结果，呃，竟然又出错啦！为什么啊？`65534 + 1 = 65535`，并没有超出范围啊！其实吧，之所以会出现这样的问题，是因为：**在 MySQL 的记录中，如果有任何一个字段允许为空，那么系统就会自动从整个记录中保留一个字节来存储`null`，若想释放`null`所占的字节，则必须保证所有字段都不允许为空。**

```
-- 求出 varchar 在 utf8 和 gbk 字符集下的实际最大值
create table my_utf82(
	stuno tinyint not null,       -- 1
	name varchar(21844) not null  -- 21844 * 3 + 2 = 65534
)charset utf8;

create table my_gbk2(
	stuno tinyint not null,       -- 1
	name varchar(32766) not null  -- 32766 * 2 + 2 = 65534
)charset gbk;
```

![5](http://img.blog.csdn.net/20170522165703155)

如上图所示，咱们已经成功创建了表`my_utf82`和`my_gbk2`. 

此外，在 MySQL 中，`text`文本字符串不占用记录长度，额外存储，但是`text`文本字符串也是属于记录的一部分，无论是在`utf8`还是在`gbk`字符集之中，其都占用记录中的`10`个字节长度，用来保存数据的地址以及长度。






----------
———— ☆☆☆ —— [返回 -> 史上最简单的 MySQL 教程 <- 目录](https://github.com/guobinhit/mysql-tutorial/blob/master/README.md) —— ☆☆☆ ————
