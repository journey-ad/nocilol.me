---
title: 在CentOS上使用 Gogs 搭建自己的 Git 服务器
date: 2016-06-21 07:40:00
categories: lab
tags: [git,Gogs,github,Go]
urlname: using-gogs-to-build-your-own-git-server-on-centos
---
![Gogs](https://img.imjad.cn/images/2016/06/21/QQ20160621153827.png "Gogs")
为了同步代码我们通常使用git来对代码进行管理，最常用的就是Github了。但出于某些原因，有些代码我们并不想公开（当然我们可以使用github的私有仓库，但同样有基于安全意义上的考量），作为一个现实意义上的穷人，我并不能支付得起github的付费方案，因此使用免费开源的git server就成为了我的最佳选择。作为折腾的一部分，在这里把安装的过程和一些需要注意的地方记录一下。

目前比较好的git server有两个，Gitlab和Gogs。Gitlab作为Github的山寨版，功能非常全面，但与此同时也十分臃肿。国人开发的Gogs则十分轻量，据作者说甚至能在一台普通的树莓派上运行，作为个人用的代码托管平台，显然Gogs更适合我。

##安装步骤
1.新建用户
2.下载源码进行编译/下载预编译二进制文件
3.安装
4.调整配置
5.配置nginx反向代理
6.添加服务及开机启动

*注意，这里默认你已经安装并配置好了MySQL和nginx，如果没有，请自行查找如何安装和配置这些依赖。当然你也可以使用SQLite数据库。*

##新建用户
Gogs默认以git用户运行，因此我们需要建立一个git用户
```bash
$ sudo adduser git   #建立git用户
$ su git             #以git用户登录
$ mkdir ~/.ssh       #建立.ssh目录
```

##下载并解压缩
为了节省时间，我选择的是编译好的二进制文件，需要从源码编译的话，请参考一般Go语言项目的编译。
在[这里](https://gogs.io/docs/installation/install_from_binary.html "这里")寻找适用于你系统的二进制包

下载后解压到你喜欢的地方，例如```/usr/share/gogs/```或者```/home/git/gogs/```。目录结构如下：
```bash
$ ls ~/gogs
custom  gogs     log     README.md     scripts
data    LICENSE  public  README_ZH.md  templates
```

##安装
首先建立数据库。```Gogs```目录的```scripts/mysql.sql```文件是数据库初始化文件。
执行```mysql -u root -p < scripts/mysql.sql```（需要输入密码）即可初始化数据库。

然后登录MySQL创建一个新用户gogs，并将数据库gogs 的所有权限都赋予该用户。
```bash
$ mysql -u root -p
mysql> # （输入密码）
mysql> create user 'gogs'@'localhost' identified by '密码';
mysql> grant all privileges on gogs.* to 'gogs'@'localhost';
mysql> flush privileges;
mysql> exit;
```
执行```./gogs web```运行Gogs，然后访问http://服务器IP:3000/ 安装，按照页面提示填写信息。

##调整配置
配置文件位于Gogs目录的```custom/conf/app.ini```，为INI格式的文本文件，关键配置如下。
*详细的配置解释和默认值请参考[配置文件手册](https://gogs.io/docs/advanced/configuration_cheat_sheet.html "配置文件手册")*

- ```RUN_USER```默认为git，指定Gogs以哪个用户运行
- ```ROOT``` 所有仓库的存储根路径
- ```PROTOCOL```用nginx反代的话使用http
- ```DOMAIN```域名，会影响SSH clone地址
- ```ROOT_URL```完整的根路径，会影响页面上链接指向，以及HTTP(s) clone的地址
- ```HTTP_ADDR```监听地址，使用nginx建议127.0.0.1，否则localhost或0.0.0.0也可以
- ```HTTP_PORT```监听端口，默认3000
- ```INSTALL_LOCK```锁定安装页面
- Mailer相关的选项
*注意邮箱stmp地址要加端口号*

##配置nginx反向代理
修改nginx的配置文件，添加如下内容：
```bash
server {
    server_name 域名或IP;
    listen 80; #或者443，如果你使用 HTTPS 的话
    # ssl on; 是否启用加密连接
    # 如果你使用HTTPS，还需要填写ssl_certificate和ssl_certificate_key

    location / { #如果你希望通过子路径访问，此处修改为子路径，注意以/开头和结束
        proxy_pass http://127.0.0.1:3000/;
    }
}
```
执行```sudo service nginx reload```重新载入nginx配置

##添加服务及开机启动
在Gogs目录的```scripts```目录中找到对应你所用发行版的版本，文件名为```gogs```。
执行
```bash
$ sudo cp ~/gogs/scripts/gogs /etc/init.d/
$ sudo chmod +x /etc/init.d/gogs
```
以后就可以使用```sudo service gogs {start|stop|status|restart}```来对Gogs服务进行管理。

但是我们还需要为Gogs添加开机启动，执行：
```bash
$ sudo chkonfig gogs on
```
即可添加开机启动，取消自启可以执行```sudo chkonfig gogs off```

####参考资料
[使用Gogs搭建自己的Git服务器](https://mynook.info/blog/post/host-your-own-git-server-using-gogs "使用 Gogs 搭建自己的 Git 服务器")
[配置文件手册](https://gogs.io/docs/advanced/configuration_cheat_sheet.html "配置文件手册")
[Gogs在github上的项目页](https://github.com/gogits/gogs/ "Gogs在github上的项目页")