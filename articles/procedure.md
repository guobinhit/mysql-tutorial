# 史上最简单的 MySQL 教程（三十三）「存储过程」

## 存储过程

存储过程简称过程，`procedure`，是一种用来处理数据（增删改）的方式。简单点，我们也可以将其理解为没有返回值的函数。

### 创建过程

```
-- 基本语法
create procedure 过程名([参数列表])
begin
	-- 过程体
end
```
如果我们定义的过程的过程体内仅含有一条语句，则可以省略`begin`和`end`。执行如下语句，进行测试：

```
-- 创建过程
create procedure pro()
select * from student;
```

![create-procedure](https://github.com/guobinhit/mysql-tutorial/blob/master/images/procedure/create-procedure.png)

如上图所示，我们创建了一个名为`pro()`的过程，其目的就是为了查询`student`表中的数据。但实际上，过程多用于处理数据，查询并不多用。

### 查看过程

查看过程，基本语法为：

- `show procedure status + [like 'pattern'];`

![show-procedure](https://github.com/guobinhit/mysql-tutorial/blob/master/images/procedure/show-procedure.png)

查看过程的创建语句，基本语法为：

- `show create procedure + 过程名;`

![show-create-procedure](https://github.com/guobinhit/mysql-tutorial/blob/master/images/procedure/show-create-procedure.png)

### 调用过程

由于函数有返回值，因此我们可以用`select`来调用函数。但是存储过程没有返回值，怎么办？实际上，对于存储过程，有一个专门的调用关键字`call`，调用过程的基本语法为：

- `call + 过程名;`

![call-pro](https://github.com/guobinhit/mysql-tutorial/blob/master/images/procedure/call-pro.png)

### 修改过程 & 删除过程

过程只能先删除后新增，不能修改。删除过程的基本语法为：

- `drop procedure + 过程名;`

执行如下语句，进行测试：

```
-- 删除过程
drop procedure pro;
-- 查看过程
show procedure status like 'pro'\G;
```

![drop-procedure](https://github.com/guobinhit/mysql-tutorial/blob/master/images/procedure/drop-procedure.png)

### 过程参数

函数的参数需要指定数据类型，过程比函数更加严格。过程有三种自己的参数类型，分别为：

- `in`，数据只是从过程外部传入给过程内部使用，可以是数值也可以是变量；
- `out`，此参数只能传递变量，且变量指向的数据需要先清空然后才能进入过程内部，该引用供过程内部使用，过程结束后可以将变量的值传递给过程外部使用；
- `inout`，此参数只能传递变量，该变量的值可以给过程内部使用，过程结束后可以变量的值传递给过程外部使用。

因此，过程定义的具体形式应该为：

- `procedure 过程名(in 参数名字 参数类型, out 参数名字 参数类型, inout 参数名字 参数类型)`

下面，我们定义一个简单的过程，并调用过程。代码如下：

```
delimiter $$
create procedure pro2(in var1 int, out var2 int, inout var3 int)
begin
	-- 查看该过程传入的三个变量
	select var1, var2, var3;
end
$$
delimiter ;

-- 调用过程
call pro2(1,2,3);
```

![create-pro2](https://github.com/guobinhit/mysql-tutorial/blob/master/images/procedure/create-pro2.png)

如上图所示，过程`pro2`创建成功。但是，在调用过程的时候出现错误，造成该错误的原因为：过程的`out`和`inout`两个参数只能接受变量，而我们传递了具体的数值，报错也就在情理之中啦！接下来，执行如下语句，进行测试：

```
-- 设置全局变量
set @var1 = 1;
set @var2 = 2;
set @var3 = 3;

-- 调用过程
call pro2(@var1, @var2, @var3);
-- 查看变量
select @var1, @var2, @var3;
```

![set-aite-var](https://github.com/guobinhit/mysql-tutorial/blob/master/images/procedure/set-aite-var.png)

如上图所示，在我们将变量传递给过程的时候，过程正常执行。此外，通过`select`语句我们可以看到传递给`out`类型参数的`@var2`的值在经过过程处理之后，被置为`null`值啦，这也是符合`out`类型参数的先清空后使用原则的。而且，由于`out`和`inout`只能接受变量作为参数，因此在过程内部对`out`和`inout`传入的变量的修改会影响到过程外部。在这里，值得我们注意是：存储过程对变量的操作是滞后的，即**只有在过程结束的时候，才会将过程内部修改的值赋值给外部传入的对应的全局变量**。执行如下语句，进行测试：

```
delimiter $$
create procedure pro3(in var1 int, out var2 int, inout var3 int)
begin
	-- 查看该过程传入的三个变量
	select var1, var2, var3;
	-- 修改局部变量
	set var1 = 10;
	set var2 = 20;
	set var3 = 30;
	-- 查看局部变量
	select var1, var2, var3;
	-- 查看全局变量
	select @var1, @var2, @var3;	
	-- 修改全局变量
	set @var1 = 'a';
	set @var2 = 'b';
	set @var3 = 'c';
	-- 查看全局变量
	select @var1, @var2, @var3;		
end
$$
delimiter ;
```

![create-pro3](https://github.com/guobinhit/mysql-tutorial/blob/master/images/procedure/create-pro3.png)

```
-- 调用过程
call pro3(@var1, @var2, @var3);
-- 在过程结束后，查看全局变量
select @var1, @var2, @var3;
```

![call-pro3](https://github.com/guobinhit/mysql-tutorial/blob/master/images/procedure/call-pro3.png)

如上图所示，存储过程执行成功，且验证了我们结论，即：**在存储过程没有结束的时候，对传入变量的修改并不会影响到对应的全局变量；只有在存储过程结束后，才会将对应的变量值赋值给`out`和`inout`类型的变量，而`in`类型的变量不受影响**。

----------

**温馨提示**：符号`[]`括起来的内容，表示可选项；符号`+`，则表示连接的意思。


----------
———— ☆☆☆ —— [返回 -> 史上最简单的 MySQL 教程 <- 目录](https://github.com/guobinhit/mysql-tutorial/blob/master/README.md) —— ☆☆☆ ————

