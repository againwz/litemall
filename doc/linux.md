部署Node.js项目（CentOS）
文档提供方：上海驻云信息科技有限公司    更新时间：2018-01-15 16:44:00
   
Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境，用来方便地搭建快速的易于扩展的网络应用。Node.js 使用了一个事件驱动、非阻塞式 I/O 的模型，使其轻量又高效，非常适合运行在分布式设备的数据密集型的实时应用。Node.js 的包管理器 npm，是全球最大的开源库生态系统。典型的应用场景包括：

实时应用：如在线聊天，实时通知推送等等（如socket.io）
分布式应用：通过高效的并行I/O使用已有的数据
工具类应用：海量的工具，小到前端压缩部署（如grunt），大到桌面图形界面应用程序
游戏类应用：游戏领域对实时和并发有很高的要求（如网易的pomelo框架）
利用稳定接口提升Web渲染能力
前后端编程语言环境统一：前端开发人员可以非常快速地切入到服务器端的开发（如著名的纯Javascript全栈式MEAN架构）
适用对象

本文档介绍如何在阿里云CentOS系统的云服务器ECS实例上，安装Nodejs并部署项目。

准备工作

部署之前，请做如下准备工作：

购买ECS实例
您的实例运行的镜像是CentOS7.2
您的实例可以连接公网
本地已经安装用于连接 Linux 实例的工具，如 PuTTY。
基本流程

使用云服务器ECS安装Nodejs并部署项目的操作步骤如下：

购买ECS实例，并连接实例。
选择以下任一种方法部署Node.js环境：
使用二进制文件。
使用NVM安装多版本。
部署测试项目。
操作步骤

步骤 1：创建ECS实例

创建ECS实例。选择操作系统为公共镜像CentOS7.2。使用root用户登录Linux实例。

步骤2：部署Node.js环境

使用以下任一种方法部署Node.js环境。

使用二进制文件安装

该部署过程使用的安装包是已编译好的二进制文件，解压之后，在bin文件夹中就已存在node和npm，无需手工编译。

安装步骤：

wget命令下载Node.js安装包。该安装包是编译好的文件，解压之后，在bin文件夹中就已存在node和npm，无需重复编译。

wget https://nodejs.org/dist/v6.9.5/node-v6.9.5-linux-x64.tar.xz
解压文件。

tar xvf node-v6.9.5-linux-x64.tar.xz
创建软链接，使node和npm命令全局有效。通过创建软链接的方法，使得在任意目录下都可以直接使用node和npm命令：

ln -s /root/node-v6.9.5-linux-x64/bin/node /usr/local/bin/node
ln -s /root/node-v6.9.5-linux-x64/bin/npm /usr/local/bin/npm
查看node、npm版本。

node -v
npm -v
至此，Node.js环境已安装完毕。软件默认安装在/root/node-v6.9.5-linux-x64/目录下。如果需要将该软件安装到其他目录（如：/opt/node/）下，请进行如下操作：

mkdir -p /opt/node/
mv /app/node-v6.9.5-linux-x64/* /opt/node/
rm -f /usr/local/bin/node
rm -f /usr/local/bin/npm
ln -s /opt/node/bin/node /usr/local/bin/node
ln -s /opt/node/bin/npm /usr/local/bin/npm
使用NVM安装多版本

NVM（Node version manager）是Node.js的版本管理软件，使用户可以轻松在Node.js各个版本间进行切换。适用于长期做 node 开发的人员或有快速更新node版本、快速切换node版本这一需求的用户。

安装步骤：

直接使用git将源码克隆到本地的~/.nvm目录下，并检查最新版本。

yum install git
git clone https://github.com/cnpm/nvm.git ~/.nvm && cd ~/.nvm && git checkout `git describe --abbrev=0 --tags`
激活NVM。

echo ". ~/.nvm/nvm.sh" >> /etc/profile
source /etc/profile
列出Node.js的所有版本。

nvm list-remote
安装多个Node.js版本。

nvm install v6.9.5
nvm install v7.4.0
运行 nvm ls 查看已安装Node.js版本，当前使用的版本为v6.9.5。返回结果如下所示。

[root@iZXXXXZ .nvm]# nvm ls
      v6.9.5
->       v7.4.0
      system
stable -> 7.4 (-> v7.4.0) (default)
unstable -> 6.9 (-> v6.9.5) (default)
运行 nvm use v7.4.0 切换Node.js版本至v7.4.0。返回结果如下所示。

[root@iZXXXXZ .nvm]# nvm use v7.4.0
Now using node v7.4.0
NVM的更多操作请参考帮助文档：

nvm help
步骤3：部署测试项目

新建项目文件example.js。
cd ~
touch example.js
使用vim编辑器打开项目文件example.js。

yum install vim
vim example.js
输入 i，进入编辑模式，将以下项目文件内容粘贴到文件中。使用Esc按钮，退出编辑模式，输入:wq，回车，保存文件内容并退出。
项目文件内容：

const http = require('http');
const hostname = '0.0.0.0';
const port = 3000;
const server = http.createServer((req, res) => {
res.statusCode = 200;
res.setHeader('Content-Type', 'text/plain');
res.end('Hello World\n');
});
server.listen(port, hostname, () => {
console.log(`Server running at http://${hostname}:${port}/`);
});
注意：
项目文件内容中的3000为端口号，可以自行定义。
运行项目。

node ~/example.js
注意：
可以使用命令 node ~/example.js & 将项目置于后台运行。
使用命令查看项目端口是否存在。

netstat -tpln
登录ECS管理控制台，并在安全组中 添加安全组规则 放行端口（如本示例中为TCP 3000端口）。

（可选）如果您的实例中开启了防火墙，必须添加端口的入站规则（如本示例中为TCP 3000端口）。

在本地机器的浏览器中输入 http://实例公网IP地址:端口号 访问项目。
通过浏览器访问项目



配置JDK

在终端，通过：

yum list java*
1
查看阿里云有的java包

可以看到有很多，并且有java1.8.0，所以我们愉快的安装就可以了

通过如下命令安装jdk：

yum install java-1.8.0-openjdk*
1
安装完成之后，我们可以通过

java -version
1
查看java安装是否成功 
这里写图片描述

通过这种方式不需要你自己配置java环境变量，也是一个比较方便的方式。

配置mysql

我当时也不知道为什么脑抽就选择了安装mysql5.7，因为最新版本的很多改动比较大，除了错误网上很多都搜索不到，是通过查看mysql的参考文档解决的。

其实这也挺好的，你走在别人前头的感觉还是挺不错的。

首先，这里并没有通过yum方式安装，因为aliyun的mysql版本比较低，所以可以直接到mysql网站下载：

这里是最新版本下载方式，在服务器终端输入如下命令下载：

wget http://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.17-1.el7.x86_64.rpm-bundle.tar
1
然后将其添加到yum库中进行安装即可。安装方式同上。

以下介绍自己遇到的坑点：

首先，我安装过程中，好像没让我输入过密码，但是安装完成后，我通过mysql -u root或者mysql都无法登陆，所以只能通过强制更改密码方式： 
参考这篇文章修改密码。 
http://www.centoscn.com/mysql/2014/0603/3081.html 
第一个槽点：修改密码指令mysql数据库中没有了password字段，改为了authentication_string字段，所以修改密码需要将命令中的password替换为authentication_string

修改完成后，确实可以登录进入mysql了，但是坑爹的是什么指令都执行不了，提示请用alter user方式更改密码后再执行命令。。。更改密码，发现不能改，提示密码不符合要求，好嘛，查了一下，5.7版本要求大小写字母，数字，特殊符号一个都不能少，改了密码之后终于可以正常使用了！

具体更改密码指令参见参考文档： 
http://dev.mysql.com/doc/refman/5.7/en/preface.html

这里我们就可以把之前穿好的sql文件导入到服务器的mysql中了，首先在msyql中建立相同名字的数据库。例如：users

然后使用该数据库执行

source users.sql
1
就可以成功导入了。

***************************************


1、下载 jdk1.8，我下载的jdk-8u121-linux-x64.tar.gz，使用FileZilla上传至/home/ftp，在/usr/lib/jvm创建目录

sudo mkdir /usr/lib/jvm
1
解压/home/ftp到刚才创建目录/usr/lib/jvm

sudo tar -zxvf jdk-8u121-linux-x64.tar.gz -C /usr/lib/jvm
1
2、配置java环境变量

vi ~/.bashrc
1
移至光标到文件末尾,添加以下内容

#set oracle jdk environment
export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_121  ##这里要换成自己解压的jvm目录
export JRE_HOME=${JAVA_HOME}/jre  
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib  
export PATH=${JAVA_HOME}/bin:$PATH  
1
2
3
4
5
使变量生效

source ~/.bashrc
1
3、命令端输入java -version

java version "1.8.0_121"
Java(TM) SE Runtime Environment (build 1.8.0_121-b13)
Java HotSpot(TM) 64-Bit Server VM (build 25.121-b13, mixed mode)


*********************
卸载yum安装
[root@iZ2zeimh1usrjdbas8qoncZ app]# rpm -qa|grep mysql
[root@iZ2zeimh1usrjdbas8qoncZ app]# rpm -qa|grep mysql
[root@iZ2zeimh1usrjdbas8qoncZ app]# rpm -qa|grep java
python-javapackages-3.4.1-11.el7.noarch
tzdata-java-2018e-3.el7.noarch
javapackages-tools-3.4.1-11.el7.noarch
java-1.8.0-openjdk-headless-1.8.0.171-8.b10.el7_5.x86_64
java-1.8.0-openjdk-1.8.0.171-8.b10.el7_5.x86_64
[root@iZ2zeimh1usrjdbas8qoncZ app]# mysql rpm -e --nodeps java-1.8.0-openjdk-1.8.0.171-8.b10.el7_5.x86_64
-bash: mysql: command not found
[root@iZ2zeimh1usrjdbas8qoncZ app]# mysql rpm -e --nodeps java
-bash: mysql: command not found
[root@iZ2zeimh1usrjdbas8qoncZ app]# rpm -e --nodeps java-1.8.0-openjdk-1.8.0.171-8.b10.el7_5.x86_64
[root@iZ2zeimh1usrjdbas8qoncZ app]# rpm -e --nodeps java-1.8.0-openjdk-headless-1.8.0.171-8.b10.el7_5.x86_64
[root@iZ2zeimh1usrjdbas8qoncZ app]# java
-bash: /usr/bin/java: No such file or directory


***************************************
:ffi;qK=X3>Y

Yasin@mysql
Yasin@ccy
卸载MariaDB

CentOS7默认安装MariaDB而不是MySQL，而且yum服务器上也移除了MySQL相关的软件包。因为MariaDB和MySQL可能会冲突，故先卸载MariaDB。

1、安装新版mysql之前，我们需要将系统自带的mariadb-lib卸载

[root@linuxidc home]# rpm -qa | grep -i mariadb
 mariadb-libs-5.5.52-1.el7.x86_64
[root@linuxidc home]# rpm -e --nodeps mariadb-libs-5.5.52-1.el7.x86_64
2、到mysql的官网下载最新版mysql的rpm集合包：mysql-5.7.18-1.el6.x86_64.rpm-bundle.tar

3、上传mysql-5.7.18-1.el6.x86_64.rpm-bundle.tar到linux服务器，并解压tar包

[root@linuxidc home]# mkdir mysql
[root@linuxidc home]# tar -xf mysql-5.7.18-1.el6.x86_64.rpm-bundle.tar -C mysql
[root@linuxidc home]# cd mysql
[root@linuxidc mysql]# ll
total 459492
-rw-r--r-- 1 7155 31415  23618836 Mar 20 17:40 mysql-community-client-5.7.18-1.el6.x86_64.rpm
-rw-r--r-- 1 7155 31415    335496 Mar 20 17:40 mysql-community-common-5.7.18-1.el6.x86_64.rpm
-rw-r--r-- 1 7155 31415   3747352 Mar 20 17:40 mysql-community-devel-5.7.18-1.el6.x86_64.rpm
-rw-r--r-- 1 7155 31415  39086508 Mar 20 17:40 mysql-community-embedded-5.7.18-1.el6.x86_64.rpm
-rw-r--r-- 1 7155 31415 135869292 Mar 20 17:40 mysql-community-embedded-devel-5.7.18-1.el6.x86_64.rpm
-rw-r--r-- 1 7155 31415   2177064 Mar 20 17:40 mysql-community-libs-5.7.18-1.el6.x86_64.rpm
-rw-r--r-- 1 7155 31415   1723180 Mar 20 17:40 mysql-community-libs-compat-5.7.18-1.el6.x86_64.rpm
-rw-r--r-- 1 7155 31415 159060212 Mar 20 17:41 mysql-community-server-5.7.18-1.el6.x86_64.rpm
-rw-r--r-- 1 7155 31415 104881084 Mar 20 17:41 mysql-community-test-5.7.18-1.el6.x86_64.rpm
4、使用rpm -ivh命令进行安装

[root@linuxidc mysql]# rpm -ivh mysql-community-common-5.7.18-1.el6.x86_64.rpm
warning: mysql-community-common-5.7.18-1.el6.x86_64.rpm: Header V3 DSA/SHA1 Signature, key ID 5072e1f5: NOKEY
Preparing...                          ################################# [100%]
Updating / installing...
   1:mysql-community-common-5.7.18-1.e################################# [100%]
[root@linuxidc mysql]# rpm -ivh mysql-community-libs-5.7.18-1.el6.x86_64.rpm
warning: mysql-community-libs-5.7.18-1.el6.x86_64.rpm: Header V3 DSA/SHA1 Signature, key ID 5072e1f5: NOKEY
Preparing...                          ################################# [100%]
Updating / installing...
   1:mysql-community-libs-5.7.18-1.el6################################# [100%]
[root@linuxidc mysql]# rpm -ivh mysql-community-client-5.7.18-1.el6.x86_64.rpm
warning: mysql-community-client-5.7.18-1.el6.x86_64.rpm: Header V3 DSA/SHA1 Signature, key ID 5072e1f5: NOKEY
Preparing...                          ################################# [100%]
Updating / installing...
   1:mysql-community-client-5.7.18-1.e################################# [100%]
[root@linuxidc mysql]# rpm -ivh mysql-community-server-5.7.18-1.el6.x86_64.rpm
warning: mysql-community-server-5.7.18-1.el6.x86_64.rpm: Header V3 DSA/SHA1 Signature, key ID 5072e1f5: NOKEY
Preparing...                          ################################# [100%]
Updating / installing...
   1:mysql-community-server-5.7.18-1.e################################# [100%]
[root@linuxidc mysql]# 
上面几个包有依赖关系，执行有先后。

使用rpm安装方式安装mysql，安装的路径如下：

a 数据库目录
/var/lib/mysql/
b 配置文件
/usr/share/mysql(mysql.server命令及配置文件)
c 相关命令
/usr/bin(mysqladmin mysqldump等命令)
d 启动脚本
/etc/rc.d/init.d/(启动脚本文件mysql的目录)

e /etc/my.conf

5、 数据库初始化

为了保证数据库目录为与文件的所有者为 mysql 登陆用户，如果你的linux系统是以 root 身份运行 mysql 服务，需要执行下面的命令初始化

[root@linuxidc mysql]# mysqld --initialize --user=mysql
如果是以 mysql 身份登录运行，则可以去掉 --user 选项。

另外 --initialize 选项默认以“安全”模式来初始化，则会为 root 用户生成一个密码并将该密码标记为过期，登陆后你需要设置一个新的密码，

而使用 --initialize-insecure 命令则不使用安全模式，则不会为 root 用户生成一个密码。

这里演示使用的 --initialize 初始化的，会生成一个 root 账户密码，密码在log文件里，红色区域的就是自动生成的密码

[root@linuxidc mysql]# cat /var/log/mysqld.log
2017-06-05T14:30:52.709474Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
2017-06-05T14:30:55.590590Z 0 [Warning] InnoDB: New log files created, LSN=45790
2017-06-05T14:30:56.000269Z 0 [Warning] InnoDB: Creating foreign key constraint system tables.
2017-06-05T14:30:56.109868Z 0 [Warning] No existing UUID has been found, so we assume that this is the first time that this server has been started. Generating a new UUID: 960c533e-49fb-11e7-91f2-00163e089fd2.
2017-06-05T14:30:56.116186Z 0 [Warning] Gtid table is not ready to be used. Table 'mysql.gtid_executed' cannot be opened.
2017-06-05T14:30:56.116777Z 1 [Note] A temporary password is generated for root@localhost: :Wu?2QQutQwj
 现在启动mysql数据库systemctl start mysqld.service（Centos7特有的启动方式）

[root@linuxidc mysql]# systemctl start mysqld.service
可以使用下面两个命令对mysql进行停止，启动和重启：

启动：

使用 service 启动：service mysqld start
使用 mysqld 脚本启动：/etc/inint.d/mysqld start
使用 safe_mysqld 启动：safe_mysqld&
停止：

使用 service 启动：service mysqld stop
使用 mysqld 脚本启动：/etc/inint.d/mysqld stop
mysqladmin shutdown 
重启：

使用 service 启动：service mysqld restart
使用 mysqld 脚本启动：/etc/inint.d/mysqld restart
连接数据库

[root@linuxidc mysql]# mysql -u root -p 
Enter password:
密码输入：  :Wu?2QQutQwj

修改密码：

set password = password('你的密码');
设置远程访问

grant all privileges on *.* to 'root' @'%' identified by '123456'; 
flush privileges;
设置mysql开机启动

加入到系统服务：
chkconfig --add mysql
自动启动：
chkconfig mysql on
查询列表：
chkconfig

说明：都没关闭（off）时是没有自动启动。


linux mysql添加用户，删除用户，以及用户权限
一些主要的命令：

登录：

mysql -u username -p

显示全部的数据库：

show databases;

使用某一个数据库：

use databasename;

显示一个数据库的全部表：

show tables;

退出：

quit;

删除数据库和数据表

mysql>drop database 数据库名;

mysql>drop table 数据表名;
用户相关：

查看全部的用户：

SELECT DISTINCT CONCAT('User: ''',user,'''@''',host,''';') AS query FROM mysql.user;

新建用户：

CREATE USER 'dog'@'localhost' IDENTIFIED BY '123456'; 


为用户授权：

格式：
grant 权限 on 数据库.* to username@登录主机 identified by "password";
演示样例：
grant all privileges on testDB.* to test@localhost identified by '1234';
然后须要运行刷新权限的命令：
flush privileges;

为用户授予部分权限：

grant select,update on testDB.* to test@localhost identified by '1234';

授予一个用户全部数据库的某些权限：

grant select,delete,update,create,drop on *.* to test@"%" identified by "1234";

删除用户：

Delete FROM user Where User='test' and Host='localhost';
然后刷新权限；

删除账户及权限：>drop user username@'%';

　　　　　　　　>drop user username@ localhost; 

改动指定用户password

使用root登录：
mysql -u root -p
运行命令：
update mysql.user set password=password('新密码') where User="test" and Host="localhost";
刷新权限：
flush privileges;


******************

Linux系统下我的/etc/sysconfig/路径下无iptables文件
2017年09月25日 11:46:11
阅读数：3850
虚拟机新装了一个CentOs7，然后做防火墙配置的时候找不到iptables文件，解决方法如下：

因为默认使用的是firewall作为防火墙，把他停掉装个iptable

systemctl stop firewalld 
systemctl mask firewalld

yum install -y iptables 
yum install iptables-services

然后就有iptables文件，就可以作配置

开启服务 
systemctl start iptables.service

systemctl restart iptables.service // 重启防火墙使配置生效 
systemctl enable iptables.service // 设置防火墙开机启动

其他命令： 
检查是否安装了iptables 
service iptables status 
安装iptables 
yum install -y iptables 
升级iptables 
yum update iptables 
安装iptables-services 
yum install iptables-services

systemctl disable iptables #禁止iptables服务 
systemctl stop iptables #暂停服务 
systemctl enable iptables #解除禁止iptables 
systemctl start iptables #开启服务



*************************
 mysql 授权
 
 mysql> grant select,insert,update,delete on ccy.* to ccy@"%" identified by "Yasin@ccy";
Query OK, 0 rows affected, 1 warning (0.00 sec)

mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)

mysql> ^C
mysql>  grant select,insert,update,delete on litemall.* to test@"%" identified by "Yasin@test";
Query OK, 0 rows affected, 1 warning (0.00 sec)

mysql>  revoke all on *.* from 'test'@"%";
Query OK, 0 rows affected (0.00 sec)

mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)

mysql>  grant select,insert,update,delete on litemall.* to test@"%" identified by "Yasin@test";
Query OK, 0 rows affected, 1 warning (0.00 sec)

mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)



*****************

/etc/java/jdk1.8.0_171
[root@iZ2zeimh1usrjdbas8qoncZ jdk1.8.0_171]# vi  /etc/profile

JAVA_HOME=/etc/java/jdk1.8.0_171
PATH=$JAVA_HOME/bin:$PATH
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export JAVA_HOME
export PATH
export CLASSPATH


[root@iZ2zeimh1usrjdbas8qoncZ jdk1.8.0_171]# source /etc/profile


