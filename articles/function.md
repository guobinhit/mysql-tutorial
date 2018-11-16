# 史上最简单的 MySQL 教程（三十二）「函数」

# 函数

**函数**，就是将一段代码封装到一个结构中，在需要执行该段代码的时候，直接调用该结构（函数）执行即可。此操作，实现了代码的复用。在 MySQL 中，函数有两种，分别为：系统函数和自定义函数。

## 系统函数

顾名思义，系统函数就是系统定义好的函数，在需要的时候，我们直接调用即可。

任何函数都有返回值（对于空函数，我们就认为其返回值为`空`），而且在 MySQL 中任何有返回值的操作都是通过`select`来操作的，因此 MySQL 的函数调用就是通过`select`来实现的。

在 MySQL 中，字符是字符串操作中最常见的基本单位。此外，如果对中文的截取是按字节进行的话，很容易造成乱码的问题。

下面，我们介绍一些常见的、对字符进行操作的系统函数：首先，执行如下语句，定义一些变量，

```
set @cn = '你好世界';
set @en = 'hello world';
set @one = 'charies';
set @two = 'gavin';
set @three = 'Gavin';

select @cn, @en, @one, @two, @three;
```

![set-aite-cn](https://github.com/guobinhit/mysql-tutorial/blob/master/images/function/set-aite-cn.png)

然后，调用系统函数进行测试：

- `substring`，截取字符串，单位为字符；

![select-substring](https://github.com/guobinhit/mysql-tutorial/blob/master/images/function/select-substring.png)

如上图所示，在 MySQL 中字符串的位置是从`1`开始，`0`含有特殊的意义。


- `char_length`，获取字符长度；
- `length`，获取字节长度；

![select-char-length](https://github.com/guobinhit/mysql-tutorial/blob/master/images/function/select-char-length.png)

- `instr`，判断字符串中某个子串是否存在，存在则返回具体的位置，不存在则返回`0`；

![select-instr](https://github.com/guobinhit/mysql-tutorial/blob/master/images/function/select-instr.png)

- `lpad`，左填充，将字符串按照某个指定的填充方式，填充到指定（字符）长度；

![select-lpad](https://github.com/guobinhit/mysql-tutorial/blob/master/images/function/select-lpad.png)

如上图所示，在对`@en`进行填充的时候，填充结果为`hellohellhello world`，其中第二个`hello`并没有填充全，这是因为系统函数`lpad`的第二个参数限定的了变量`@en`的具体长度，如示例中我们设置其为`20`，而原`@en`的长度为`11`，因此只能向`@en`中在填充`9`个字符。

- `insert`，找到目标位置，将指定长度的字符串替换为目标字符串；

![select-insert](https://github.com/guobinhit/mysql-tutorial/blob/master/images/function/select-insert.png)

如上图所示，`insert`函数并没有修改变量自身的值，只是对变量的值进行加工而已。

- `strcmp`，比较字符串的大小；

![select-strcmp](https://github.com/guobinhit/mysql-tutorial/blob/master/images/function/select-strcmp.png)

如上图所示，在用`strcmp`函数对字符串进行比较的时候不区分大小写（默认校对集），并用`0`表示两个字符串相等；用`-1`表示第一个参数的字符串小于第二个参数的字符串；用`1`表示第一个参数的字符串大于第二个参数的字符串。

## 自定义函数

对于任意一个函数，其都包含如下要素：

- 函数名；
- 参数类别（可以为空）；
- 返回值；
- 函数体（作用域）。

根据上面这些函数要素，我们就来尝试创建自定义函数。

### 创建函数

```
-- 基本语法
create function 函数名([参数列表]) returns 数据类型
begin
	-- 函数体
	-- 返回值，类型为 returns 指定的数据类型
end
```
 如果我们定义的函数的函数体内仅含有返回值，则可以省略`begin`和`end`。此外，自定义函数和系统函数的调用方式相同。执行如下语句，进行测试：

```
-- 自定义函数
create function showLove() returns int
return 521;
-- 调用自定义函数
select showLove();
```
![create-show-love](https://github.com/guobinhit/mysql-tutorial/blob/master/images/function/create-show-love.png)

### 查看函数

查看函数，基本语法为：

- `show function status + [like 'pattern'];`

![show-function-status](https://github.com/guobinhit/mysql-tutorial/blob/master/images/function/show-function-status.png)

如上图所示，我们可以看到`showLove`函数是属于`test`数据库的，这引出了函数的一个性质，即**函数是属于具体数据库的，在一个数据库定义的函数不能在其定义的数据库外使用，但是可以查看**。

查看函数的创建语句，基本语法为：

- `show create function + 函数名;`

![show-create-function](https://github.com/guobinhit/mysql-tutorial/blob/master/images/function/show-create-function.png)

### 修改函数 & 删除函数

函数只能先删除后新增，不能修改。删除函数的基本语法为：

- `drop function + 函数名;`

执行如下语句，进行测试：

```
-- 删除函数
drop function showLove;
-- 查看函数
show function status like 'showLove'\G;
```

![drop-function](https://github.com/guobinhit/mysql-tutorial/blob/master/images/function/drop-function.png)

### 函数参数

对于函数的参数，一共有两种，分别为形参和实参，其中，形参可以理解为定义函数时使用的参数，且形参必须指定数据类型；实参可以理解为在调用函数时传入的值或变量。因此，函数定义的具体形式应该为：

- `function 函数名(形参名字 形参类型) returns 返回数据类型`

下面，我们定义一个函数，完成一个简单的需求，即求`1`到指定数值的和。代码如下：

```
delimiter $$
create function addAll(num int) returns int
begin
	-- 定义条件变量
	set @i = 1;
	set @res = 0;    -- 保存求和结果
	-- 循环求和
	while @i <= num do
		-- 任何变量想要修改值都必须使用 set 关键字，且 MySQL 中没有 += 或者 ++ 运算符
		set @res = @res + @i;
		-- 修改循环变量
		set @i = @i + 1;
	end while;
	-- 返回求和结果
	return @res;
end
$$
delimiter ;
```

![create-addall2](https://github.com/guobinhit/mysql-tutorial/blob/master/images/function/create-addall2.png)

如上图所示，函数已经定义成功。接下来，执行如下语句，进行测试：

```
-- 调用函数求和
select addAll(100);

-- 查询自定义变量 @res 和 @i
select @res, @i;
```
![select-addall](https://github.com/guobinhit/mysql-tutorial/blob/master/images/function/select-addall.png)

如上图所示，求和函数`addAll`已经正确执行。此外，我们发现在函数内部使用`@`符号定义的变量`@res`和`@i`在函数外部也是可以查看使用的，这说明：**使用`@`符号定义的变量为全局变量**。

### 变量作用域

在 MySQL 中，变量的作用域有两种，分别为全局和局部，其中，全局变量可以在任何地方使用；局部变量只能在函数内部使用。

- 全局变量：使用`set`关键字定义，用`@`符号标识；
- 局部变量：使用`declare`关键字声明，且所用的局部变量必须在函数体开始之前进行声明。

接下来，我们利用局部变量定义一个函数，完成一个简单的需求，即求`1`到指定数值的和，要求`10`的倍数不加。代码如下：

```
delimiter $$
create function addAll2(num int) returns int
begin
	-- 声明变量，包含循环变量和结果变量
	declare i int default 1;    -- 定义局部变量可以含有属性
	declare res int default 0;
	-- 循环求和
	mywhile:while i <= num do
		-- 条件判断
		if i % 10 = 0 then
			-- 修改循环变量
			set i = i + 1;
			-- 重新循环
			iterate mywhile;
		end if;
		-- 修改结果变量
		set res = res + i;
		-- 修改循环变量
		set i = i + 1;
	end while;
	-- 返回求和结果
	return res;
end
$$
delimiter ;
```

![create-addall2](https://github.com/guobinhit/mysql-tutorial/blob/master/images/function/create-addall2.png)

如上图所示，函数已经定义成功。接下来，执行如下语句，进行测试：

```
-- 调用函数求和
select addAll(100), addAll2(100);
```
![select-addall2](https://github.com/guobinhit/mysql-tutorial/blob/master/images/function/select-addall2.png)

如上图所示，函数已经正确执行。

----------

**温馨提示**：符号`[]`括起来的内容，表示可选项；符号`+`，则表示连接的意思。


----------
———— ☆☆☆ —— [返回 -> 史上最简单的 MySQL 教程 <- 目录](https://github.com/guobinhit/mysql-tutorial/blob/master/README.md) —— ☆☆☆ ————
