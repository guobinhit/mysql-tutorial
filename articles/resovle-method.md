# 详述 MySQL 数据库输入密码后闪退的问题及解决方案

1 案例说明
------

　　最近一直在用 MySQL 数据库演示基础功能，但是这两天忽然出现了一个问题，那就是：**在启动 MySQL 服务端并输入密码后，出现闪退现象。**

　　之后，在网上搜了搜，发现出现这种问题很常见，大多数原因可能是在咱们使用安全软件的时候，无意中关闭了 MySQL 服务。此外，如果 MySQL 服务已经启动了，但还是出现了闪退的现象，那就可能是 MySQL 的配置出现了问题。



2 解决方案
------

　　在出现上述问题的时候，咱们首先要查看 MySQL 服务是否启动，如果 MySQL 服务没有启动，那么大多数情况，当咱们手动启动 MySQL 服务之后，上述问题就可以得到解决；如果 MySQL 服务已经启动，但还是出现了闪退现象，这就需要咱们修改相关配置啦！

接下来，按照如下步骤（依次点击），进入`服务`管理页面：

 1. 我的电脑；
 2. 管理；
 3. 服务和应用程序；
 4. 服务。

![computer-management](https://github.com/guobinhit/mysql-tutorial/blob/master/images/resovle-method/computer-management.png)

如上图所示，在这里，咱们就可以查看 MySQL 的服务的状态啦！


**第 1 种情况：MySQL 服务没有启动**

咳咳，这个容易，点击右键或者点击下图红色箭头所示的按钮启动 MySQL 服务即可。

![start-server](https://github.com/guobinhit/mysql-tutorial/blob/master/images/resovle-method/start-server.png)

点击`启动`后，就让咱们重新启动 MySQL 服务端，输入密码，进行测试：

![test-mysql-client](https://github.com/guobinhit/mysql-tutorial/blob/master/images/resovle-method/test-mysql-client.png)

如上图所示，显然咱们的问题已经得到了解决。

**第 2 种情况：MySQL 服务已经启动**

在 MySQL 服务启动的情况下，出现闪退的问题，这就可能是 MySQL 的配置出现了问题。更进一步，可能是`C:\ProgramFiles\MySQL\MySQL Server X.X`的`my.ini`文件（安装路径下）有问题。对于此问题：

 - **方法 1**：首先进入`cmd`切入`MySQL`的安装目录，再切入`bin`目录，输入`mysqld-nt--skip-grant-tables`命令。然后，此`cmd`窗口先不要关闭， 打开另一个窗口登陆`MySQL`或直接点开`MySQL CommandLine Client`输入正确的密码，即可。
 - **方法 2**：找出运行`win+R`，在这个`DOS`窗体中，`cd`到`MySQL Server X.X`的`bin`目录下，然后输入`mysql -u root -p`命令，再输入密码，即可。
 - **方法 3**：如果登录又提示`mysql: unknownvariable 'character-set-server=gbk'`，解决方法：修改`MySQL`安装目录下的`my.ini`文件，将里面的`default-character-set=gbk`注释或删除掉，在服务里重启`MySQL`服务，即可。

到这里，相信如果再遇到 MySQL 数据库输入密码后闪退问题时，咱们都可以轻而易举的解决啦！

----------
———— ☆☆☆ —— [返回 -> 史上最简单的 MySQL 教程 <- 目录](https://github.com/guobinhit/mysql-tutorial/blob/master/README.md) —— ☆☆☆ ————
