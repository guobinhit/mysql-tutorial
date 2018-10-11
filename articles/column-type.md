# 史上最简单的 MySQL 教程（七）「列类型」

列类型（数据类型）
=========
所谓的列类型，其实就是指数据类型，即对数据进行统一的分类，从系统的角度出发是为了能够使用统一的方式进行管理，更好的利用有限的空间。

在 SQL 中，将数据类型分成了三大类，分别为：**数值型、字符串型和日期时间型。**

![column-type-info](https://github.com/guobinhit/mysql-tutorial/blob/master/images/column-type/column-type-info.png)

对于数值型数据，可以进一步将其划分为**整数型**和**小数型**。

整数型
---

在 SQL 中，由于要考虑节省磁盘空间的问题，因此系统又将整型细分成五类，分别为：

 - `tinyint`：迷你整型，使用 1 个字节存储数据（常用）；
 - `smallint`：小整型，使用 2 个字节存储数据；
 - `mediumint`：中整型，使用 3 个字节存储数据；
 - `int`：标准整型，使用 4 个字节存储数据（常用）；
 - `bigint`：大整型，使用 8 个字节存储数据。

接下来，输入如下 SQL 语句进行测试：

```
-- 创建整型表
create table my_int(
	int_1 tinyint,
	int_2 smallint,
	int_3 int,
	int_4 bigint
)charset utf8;
```

![create-table-myint](https://github.com/guobinhit/mysql-tutorial/blob/master/images/column-type/create-table-myint.png)

如上图所示，咱们已经成功创建`my_int`表，再插入数据：

```
-- 插入数据
insert into my_int values (1,2,3,4);
insert into my_int values ('a','b','c','d');
insert into my_int values (255,2,3,4);
```

![insert-myint](https://github.com/guobinhit/mysql-tutorial/blob/master/images/column-type/insert-myint.png)

如上图所示，通过列类型，咱们可以限定插入数据的类型以及长度范围。

至于为什么在对`int_1`赋值时，报出超出范围的错误，则是因为在 SQL 中数值类型默认是有符号位的，即分正负。如果需要使用无符号的数据，这就需要咱们自己对数据类型进行声明啦，即在声明数据类型时，追加`unsigned`关键字。例如：

```
-- 在 my_int 表中，添加 int_5 字段，设置其数据类型为 tinyint unsigned
alter table my_int add int_5 tinyint unsigned;
```
![alter-myint](https://github.com/guobinhit/mysql-tutorial/blob/master/images/column-type/alter-myint.png)

如上图所示，添加`int_5`字段成功，继续插入数据：

```
-- 插入数据
insert into my_int values (1,2,3,4,255);
```

![null-myint](https://github.com/guobinhit/mysql-tutorial/blob/master/images/column-type/null-myint.png)

如上图所示，当咱们将`tinyint`限定为`unsigned`之后，已经可以插入`0~255`之间的任何整数啦！但是，回过头来，让咱们仔细看看下面这张图：

![field-type-null-key](https://github.com/guobinhit/mysql-tutorial/blob/master/images/column-type/field-type-null-key.png)

通过观察上面这张图，咱们会发现：**每个字段的数据类型后面都会跟着一对括号，并且里面含有数字。**这些数字啊，其实并没有什么特别的含义，只是表示数据的显示宽度。实际上，咱们可以修改显示的宽度，但是这种修改并不会改变数据本身的大小。

**显示宽度的意义**：在于当数据不够显示宽度的时候，会自动让数据变成对应的显示宽度，通常需要搭配一个前导`0`来增加宽度，其不改变数据值的大小，即用`zerofill`进行零填充，并且零填充会导致数值自动变成无符号。

下面，执行如下 SQL 语句：

```
-- 在 my_int 表中，添加 int_6 字段，设置其数据类型为 tinyint zerofill
alter table my_int add int_6(3) tinyint zerofill;
```

![zerofill-myint](https://github.com/guobinhit/mysql-tutorial/blob/master/images/column-type/zerofill-myint.png)

再插入数据，进行测试：

```
-- 插入数据
insert into my_int values (1,2,3,4,5,6);
```

![two-null-myint](https://github.com/guobinhit/mysql-tutorial/blob/master/images/column-type/two-null-myint.png)

如上图所示，**零填充的意义**：在于保证数据的格式。

小数型
---
小数型，即**带有小数点或者范围超出整型的数值类型**。

在 SQL 中，将小数型细分为**浮点型**和**定点型**两种，其中：

 - 浮点型：小数点浮动，精度有限，容易丢失精度；
 - 定点型：小数点固定，精度固定，不会丢失精度。

**第 1 种**：浮点型

浮点型数据是一种精度型数据，因为超出指定范围之后，其会丢失精度，自动进行四舍五入操作。理论上，浮点型分为两种精度：

 - `float`：单精度，占用 4 个字节存储数据，精度范围大概为 7 位左右；
 - `double`：双精度，占用 8 个字节存储数据，精度范围大概为 15 位左右。

**浮点型的使用方式**：如果直接用`float`，则表示没有小数部分；如果用`float(M,D)`，其中`M`代表总长度，`D`代表小数部分长度，`M-D`则为整数部分长度。

执行如下 SQL 语句创建浮点数表，进行测试：

```
-- 创建浮点数表
create table my_float(
	f1 float,
	f2 float(10,2),
	f3 float(6,2)
)charset utf8;
```
在咱们向浮点数表`my_float`插入数据的时候，可以直接插入小数，也可以插入用科学计数法表示的数据。此外，插入浮点型数据时，整数部分是不能超出长度范围的，但是小数部分是可以超出长度范围的，系统会自动进行四舍五入的操作。特别是，如果浮点数是因为系统进位（四舍五入）导致整数部分超出指定的长度，那么系统是允许成立的。

```
-- 插入测试数据
insert into my_float values (2.15e4,20.15,9999.99);
insert into my_float values (20151120,123456789.99,9999.99);
insert into my_float values (5211314,123456.99,99.99999);
```

![create-table-myfloat](https://github.com/guobinhit/mysql-tutorial/blob/master/images/column-type/create-table-myfloat.png)

如上图所示，咱们的结论得到了验证。

**第 2 种**：定点型

定点型数据，绝对的保证整数部分不会被四舍五入，也就是说不会丢失精度，但小数部分有可能丢失精度，虽然理论上小数部分也不会丢失精度。

执行如下 SQL 语句创建定点数表，以浮点数做对比，进行测试：

```
-- 创建定点数表
create table my_decimal(
	f1 float(10,2),
	d1 decimal(10,2)
)charset utf8;
```
当咱们插入数据的时候，定点数的整数部分一定不能超出长度范围（进位也不可以），小数部分的长度则可以随意超出，没有限制，系统会自动进行四舍五入的操作：

```
-- 插入测试数据
insert into my_decimal values (99999999.99,99999999.99);
insert into my_decimal values (123456789.99,2015.1314);
insert into my_decimal values (123456.99,2015.1314);
```

![create-table-mydecimal](https://github.com/guobinhit/mysql-tutorial/blob/master/images/column-type/create-table-mydecimal.png)

如上图所示，咱们的结论同样得到了验证。


日期时间型
---

日期时间型数据，顾名思义，就是用来表示日期和时间的数据类型，共有五种类型，分别为：

 - `datetime`：日期时间，其格式为`yyyy-MM-dd HH:mm:ss`，表示的范围是从 1000 年到 9999 年，有零值，即`0000-00-00 0000:00`；
 - `date`：日期，就是`datetime`的`date`部分；
 - `time`：时间，或者说是时间段，为指定的某个时间区间之间，包含正负时间；
 - `timestamp`：时间戳，但并不是真正意义上的时间戳，其是从`1970`年开始计算的，格式和`datetime`一致；
 - `year`：年份，共有两种格式，分别为`year(2)`和`year(4)`.

执行如下 SQL 语句创建日期时间表，进行测试：

```
-- 创建日期时间表
create table my_date(
	d1 datetime,
	d2 date,
	d3 time,
	d4 timestamp,
	d5 year
)charset utf8;
```

当咱们插入数据时，日期时间型中的`time`，可以为负数，甚至可以是很大的负数；`year`，可以使用 2 位数据插入，也可以使用 4 位数据插入；`timestamp`，只要当前所在的记录被更新，该字段就会自动更新为当前时间，且**时间戳类型默认为非空的**。

```
-- 插入测试数据
insert into my_date values ('2017-05-06 13:15:00','2017-05-06','13:15:00','2017-05-06 13:15:00',2017);
insert into my_date values ('2017-05-06 13:15:00','2017-05-06','-113:15:00','2017-05-06 13:15:00',69);
insert into my_date values ('2017-05-06 13:15:00','2017-05-06','-2 13:15:00','2017-05-06 13:15:00',70);
```

![create-table-mydate](https://github.com/guobinhit/mysql-tutorial/blob/master/images/column-type/create-table-mydate.png)

如上图所示，以上 3 条记录已经插入成功，接下来，咱们再来验证更新记录时，时间戳类型的字段`d4`是否会自动更新：

```
-- 更新记录，验证时间戳类型的字段是否会自动更新
update my_date set d1 = '2017-05-06 13:24:00' where d5 = 1970;
```

![update-mydate](https://github.com/guobinhit/mysql-tutorial/blob/master/images/column-type/update-mydate.png)

如上图所示，显然咱们的结论全部得到了验证。


字符串型
---

在 SQL 中，将字符串类型分成了 6 类，分别为：`char`、`varchar`、`text`、`blob`、`enum`和`set`.

**第 1 类：定长字符串**

定长字符串：`char`，即磁盘（二维表）在定义结构的时候就已经确定了最终数据的存储长度。

 - `char(L)`：L 表示 Length，即可以存储的长度，单位为字符，最大长度为 255；
 - `char(4)`：表示在 UTF8 环境下，需要 4*3=12 个字节。

**第 2 类：变长字符串**

变长字符串：`varchar`，即在分配存储空间的时候，按照最大的空间分配，但是实际用了多少，则是根据具体的数据来确定。

 - `varchar(L)`：L 表示 Length，理论长度是 65536，但是会多出 1 到 2 个字节来确定存储的实际长度；
 - `varchar(10)`：例如存储 10 个汉字，在 UTF8 环境下，需要 10*3+1=31 个字节。

实际上，如果存储长度超过 255 个字符，则既不用定长字符串也不用变长字符串，而是用文本字符串`text`.

如何选择定长字符串或者是变长字符串呢？

 - 定长字符串对磁盘空间比较浪费，但是效率高：如果数据基本上确定长度都一样，就使用定长字符串，例如身份证、电话号码等；
 - 变长字符串对磁盘空间比较节省，但是效率低：如果数据不能确定长度（不同的数据有变化），就使用变长字符串，例如地址、姓名等。

**第 3 类：文本字符串**

如果数据量非常大，通常说超过 255 个字符就会使用文本字符串。

文本字符串根据存储的格式进行分类，可以分为：

 - `text`：存储文字；
 - `blob`：存储二进制数据（其实际上都是存储路径），通常不用。

**第 4 类：枚举字符串**

枚举字符串：`enum`，需要事先将所有可能出现的结果都设计好，实际上存储的数据必须是规定好的数据中的一个。

枚举字符串的使用方式：

 - 定义：`enum('元素1','元素2','元素3'...)`，例如`enum('男','女','保密')`；
 - 使用：存储的数据，只能是事先定义好的数据。

执行如下 SQL 语句创建枚举表，进行测试：

```
-- 创建枚举表
create table my_enum(
	gender enum('男','女','保密')
)charset utf8;
```

![create-table-myenum](https://github.com/guobinhit/mysql-tutorial/blob/master/images/column-type/create-table-myenum.png)

再执行如下 SQL 语句，向表`my_enum`中插入测试数据：

```
-- 插入测试数据
insert into my_enum values ('男'),('女'),('保密');
insert into my_enum values ('male');
```
![insert-myenum](https://github.com/guobinhit/mysql-tutorial/blob/master/images/column-type/insert-myenum.png)

通过上面的测试，咱们可以发现使用枚举字符串有一个好处，那就是：**规范数据格式，插入表中的数据只能是事先定义好的某个数据。**

此外，枚举字符串还有一个作用，那就是：**节省存储空间（枚举数据通常都有一个别名），枚举实际上存储的是数值而不是字符串本身。**

在 MySQL 中，系统是有自动转换数据格式的功能的。在这里，咱们可以证明枚举字段存储的是数值，具体方法为：将数据取出来`+0`，如果是字符串最终结果永远为`0`，否则就是其他值。

```
-- 验证枚举字段实际存储的格式
select gender + 0,gender from my_enum;
```

![select-myenum](https://github.com/guobinhit/mysql-tutorial/blob/master/images/column-type/select-myenum.png)

观察上述结果，咱们可以找出枚举元素的实际规律，即按照元素出现的顺序，从`1`开始编号。接下来，咱们再来了解**枚举的原理**：

 - 枚举在进行数据规范（定义）的时候，系统会自动建立一个数字与枚举元素的对应关系（放在日志中）；在进行数据插入的时候，系统自动将字符串转换为对应的数值进行存储；在进行数据提取的时候，系统自动将数值转换成对应的字符串进行显示。

通过阅读以上枚举的原理，咱们可以知道：**使用枚举的效率并不高（低于其他类型的数据），但能规范数据和节省存储空间。**

**第 5 类：集合字符串**

集合字符串：`set`，跟枚举类似，实际存储的是数值而不是字符串。

集合字符串的使用方式：

 - 定义：`set`，元素列表；
 - 使用：可以使用元素列表中的多个元素，用逗号分隔。

执行如下 SQL 语句创建枚举表，进行测试：

```
-- 创建集合表
create table my_set(
	hobby set('音乐','电影','旅行','美食','摄影','运动','宠物')
)charset utf8;
```

![create-table-myset](https://github.com/guobinhit/mysql-tutorial/blob/master/images/column-type/create-table-myset.png)

再执行如下 SQL 语句，向表`my_set`中插入测试数据：
```
-- 插入测试数据
insert into my_set values ('电影,美食,宠物');
insert into my_set values (3);
```

![insert-myset](https://github.com/guobinhit/mysql-tutorial/blob/master/images/column-type/insert-myset.png)

再执行如下 SQL 语句，查看表`my_set`中的数据：

```
-- 查看数据
select hobby + 0,hobby from my_set;
```

![select-myset](https://github.com/guobinhit/mysql-tutorial/blob/master/images/column-type/select-myset.png)

观察上面的结果，相信大部分童鞋也懵啦！对于`3`还好理解，`3=2+1`，对应于集合中数据的编号，也正是`音乐`和`电影`；但是`74`是什么鬼啊？在此，咱们不妨将集合（`'音乐','电影','旅行','美食','摄影','运动','宠物'`）中的元素选中的记为`1`，没有选中的记为`0`，表示成二进制，也就是：

 - `0101001`

再将上面的二进制反过来：

 - `1001010`

不妨算算，上述二进制对应的十进制数，即为`74`.

到这里，相信大家已经恍然大悟啦，原来：**集合字符串中每一个元素都对应一个二进制位，其中被选中的为`1`，未选中的为`0`，最后在反过来，这个二进制数对应的十进制数即为其数据库中实际存储的是数值。**

此外，集合字符串中插入元素的顺序并没有影响，最终系统都会自动去匹配集合的顺序，即：

```
-- 插入测试数据
insert into my_set values ('电影,美食,旅行');
insert into my_set values ('旅行,电影,美食');
```

上述两个 SQL 语句会产生相同的结果：

![insert-select-myset](https://github.com/guobinhit/mysql-tutorial/blob/master/images/column-type/insert-select-myset.png)

如上图所示，显然咱们的结论得到了验证。

最后，集合的原理同枚举类似，因此可以的到相同的结论，即：**使用集合的效率并不高（低于其他类型的数据），但能规范数据和节省存储空间。**




----------
———— ☆☆☆ —— [返回 -> 史上最简单的 MySQL 教程 <- 目录](https://github.com/guobinhit/mysql-tutorial/blob/master/README.md) —— ☆☆☆ ————



