# 详述 MySQL 导出数据遇到 secure-file-priv 的问题

> **ERROR 1290 (HY000)**: The MySQL server is running with the --secure-file-priv option so it cannot execute this statement.

对于上述错误，相信对于第一次执行 MySQL 数据导出操作的同学大都会遇见。至于为什么会遇到这个错误，原因很简单，那就是：我们不知道 MySQL 默认的导出目录是哪里，甚至都不知道 MySQL 对于数据的导出目录都有所限制。这其实很正常，因为我们不可能知道所有事，只要我们在遇到问题之后，能够找到正确的解决方法并记住就足够啦！学习嘛，就是一个不断积累的过程。

接下来，我们就一起来看看如何解决这个问题：在我们安装完 MySQL 之后，会自动生成一个名为`my.ini`的配置文件，该文件记录了 MySQL 的所有默认配置，如端口号、默认字符集、默认存储引擎等等。而这个配置文件，则位于隐藏文件夹`ProgramData`中，至于如何找到该隐藏文件夹，具体可以参考[详述查看 MySQL 数据文件存储位置的方法](https://github.com/guobinhit/mysql-tutorial/blob/master/mysql-articles/datafile.md)。

此外，我们也可以通过命令查看`secure-file-priv`的当前值：

- `show variables like '%secure%';`

执行上述命令，进行测试：

![6](http://img.blog.csdn.net/20171001200442371)

如上图所示，我设置的`secure-file-priv`值为`D:/CoderLife/testMySQL`，而 MySQL 默认的值为:

- `secure-file-priv="C:/ProgramData/MySQL/MySQL Server 5.7/Uploads"`

我们可以通过打开`my.ini`配置文件进行查看及修改：

![7](http://img.blog.csdn.net/20171001200847759)

如上图所示，我注释了 MySQL 的默认值，并修改成自己指定的路径。实际上，在我们修改此配置文件的内容之后，例如修改了`secure-file-priv`的默认值，要想使其生效，我们必须关闭 MySQL 然后重新启动。在这里，需要我们注意的是：**关闭 MySQL 并不是指简单的用`exit`命令退出 MySQL 或者关闭 MySQL 窗口，而是关闭 MySQL 服务，然后重新启动服务**。至于如何开启和关闭 MySQL 服务，具体可以参考[详述 MySQL 数据库输入密码后闪退的问题及解决方案](https://github.com/guobinhit/mysql-tutorial/blob/master/mysql-articles/resovle-method.md)。


----------
———— ☆☆☆ —— [返回 -> 史上最简单的 MySQL 教程 <- 目录](https://github.com/guobinhit/mysql-tutorial/blob/master/README.md) —— ☆☆☆ ————




