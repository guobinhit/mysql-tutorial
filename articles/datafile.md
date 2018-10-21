# 详述查看 MySQL 数据文件存储位置的方法

我们可能会有一个疑惑，那就是：**当我们在本地（自己的电脑）安装完 MySQL 之后，也创建了很多的数据库啊、表啊，也存储了很多的数据啊，但是这些内容都存储到哪里了呢**？特别是，当我们需要直接操作这些数据文件的时候，翻遍了整个电脑，却找不到 MySQL 的数据文件到底在哪里，这就有些坑爹啦！

在这里，教给大家一个非常简单的能够立即定位到 MySQL 数据文件的存储位置方法，即在 MySQL 客户端键入如下命令：

- `show global variables like "%datadir%";`

![show-global-variables](https://github.com/guobinhit/mysql-tutorial/blob/master/images/datafile/show-global-variables.png)

如上图所示，MySQL 的数据文件就存放在`Data`目录。至于接下来想查看具体的数据库还是表，这就看大家的心情啦！

不过大家可能还会有一个疑问，那就是我们都查到 MySQL 的数据文件在`C`盘的`ProgramData`目录下了，但是翻遍了整个`C`盘却仍然没有找到`ProgramData`文件夹，这是为什么？答案就是：**在默认情况下，`ProgramData`文件夹是隐藏的**。

至于如何显示隐藏的文件夹，相信大家都已经玩的很溜啊！`/一脸坏笑`。


----------
———— ☆☆☆ —— [返回 -> 史上最简单的 MySQL 教程 <- 目录](https://github.com/guobinhit/mysql-tutorial/blob/master/README.md) —— ☆☆☆ ————
