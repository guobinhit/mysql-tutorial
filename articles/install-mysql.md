# 详述 MySQL 数据库的安装及配置

首先，下载 MySQL 数据库的安装包：[mysql-5.7.17.msi](http://pan.baidu.com/s/1miyoKbu).

下载完成之后，双击打开此`msi`文件，进入如下界面：

![choosing-a-setup-tyep](https://github.com/guobinhit/mysql-tutorial/blob/master/images/install-mysql/choosing-a-setup-tyep.png)

 - 标注1：安装所有 MySQL 数据库需要的产品；
 - 标注2：仅仅使用 MySQL 数据库的服务器；
 - 标注3：仅仅使用 MySQL 数据库的客户端；
 - 标注4：安装所有包含 MySQL 数据库的产品；
 - 标注5：手动选择需要安装的 MySQL 数据库的产品。

在此，咱们只需要选择 **标注2** 所示的`Server only`即可，然后点击`Next`，进入如下界面：

![installation](https://github.com/guobinhit/mysql-tutorial/blob/master/images/install-mysql/installation.png)

如上图所示，此为安装提示界面，直接点击`Execute`：

![installation-2](https://github.com/guobinhit/mysql-tutorial/blob/master/images/install-mysql/installation-2.png)

 - 标注1：表示当前的状态；
 - 标注2：表示当前的升级进度；
 - 标注3：显示或隐藏升级信息。

如上图所示，此界面展示了当前的状态、进度及升级信息，当升级进度达到`100%`后，如下图所示：

![installation-3](https://github.com/guobinhit/mysql-tutorial/blob/master/images/install-mysql/installation-3.png)

如上图所示，此为 MySQL 数据库升级成功的标志，点击`Next`：

![product-configuration](https://github.com/guobinhit/mysql-tutorial/blob/master/images/install-mysql/product-configuration.png)

如上图所示，提示咱们已经进入了产品配置的阶段，直接点击`Next`即可：

![type-and-networing](https://github.com/guobinhit/mysql-tutorial/blob/master/images/install-mysql/type-and-networing.png)

 - 标注1：配置服务器类型；
 - 标注2：选择`TCP/IP`协议；
 - 标注3：选择端口号为`3306`.

如上图所示，选择默认配置即可，点击`Next`：

![accounts-and-roles](https://github.com/guobinhit/mysql-tutorial/blob/master/images/install-mysql/accounts-and-roles.png)

如上图所示，此界面为设置账号和角色，在这里，咱们只需要设置`root`账号的密码（必须记住，登录 MySQL 数据库时需要验证）即可，密码至少四位，设置完成后，点击`Next`：

![windows-service](https://github.com/guobinhit/mysql-tutorial/blob/master/images/install-mysql/windows-service.png)

 - 标注1：由于咱们使用的是`mysql-5.7.17`版本，因此默认的服务器名称为`MySQL57`；
 - 标注2：默认选择`Standard System Account`即可。

如上图所示，选择默认配置即可，点击`Next`：

![plugins-and-extensions](https://github.com/guobinhit/mysql-tutorial/blob/master/images/install-mysql/plugins-and-extensions.png)

如上图所示，此为配置`Plugins and Extension`，即配置插件和扩展应用的，不用管它，直接点击`Next`即可：

![apply-server-configuration](https://github.com/guobinhit/mysql-tutorial/blob/master/images/install-mysql/apply-server-configuration.png)

如上图所示，此界面为让咱们确认需要应用的服务器配置，确认无误后，点击`Execute`：

![apply-server-configuration-2](https://github.com/guobinhit/mysql-tutorial/blob/master/images/install-mysql/apply-server-configuration-2.png)

如上图所示，正在进行服务器配置，此时，如果电脑弹出提示框，显示“危险”等提示信息，不用管它，这只是安全软件遇到修改配置的时候自动报警而已，直接点击“同意”即可。

![apply-server-configuration-3](https://github.com/guobinhit/mysql-tutorial/blob/master/images/install-mysql/apply-server-configuration-3.png)

如上图所示，服务器配置完成，点击`Finish`：

![product-configuration-next](https://github.com/guobinhit/mysql-tutorial/blob/master/images/install-mysql/product-configuration-next.png)

如上图所示，进一步显示配置完成，直接点击`Next`：

![installation-complete](https://github.com/guobinhit/mysql-tutorial/blob/master/images/install-mysql/installation-complete.png)

如上图所示，到这来，咱就已经将 MySQL 安装完成啦，直接点击`Finish`即可。当然，如果咱们想要查看 MySQL 的详细安装日志，可以点击`Copy Log to Clipboard`将日志复制到文本编辑器进行查看。接下来，打开 MySQL 命令行工具，验证 MySQL 是否安装成功：

![mysql-client](https://github.com/guobinhit/mysql-tutorial/blob/master/images/install-mysql/mysql-client.png)

如上图所示，显然咱们已经安装成功啦！


----------

**温馨提示**：在这里，咱们仅仅安装了 MySQL 数据库的服务器，至于客户端工具，推荐`Navicat for MySQL`和`DbVisualizer`.

----------
———— ☆☆☆ —— [返回 -> 史上最简单的 MySQL 教程 <- 目录](https://github.com/guobinhit/mysql-tutorial/blob/master/README.md) —— ☆☆☆ ————
