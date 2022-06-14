## 最坑爹的Mysql安装，做好心理准备

# 安装mysql5.6（centos6）：

不推荐，极容易失败，因为各种原因。

方法一：

检查mysql是否安装

```
 rpm -qa | grep -i mysql   --移除安装的命令：
 yum -y remove mysql-*
```

 1.给CentOS添加rpm源，并且选择较新的源

```
wget dev.mysql.com/get/mysql-community-release-el6-5.noarch.rpm
```

如果有错误：加参数(有错执行，没错省略)

```
wget dev.mysql.com/get/mysql-community-release-el6-5.noarch.rpm --no-check-certificate url
```

2.安装第一步下载的rpm文件

```
yum install mysql-community-release-el6-5.noarch.rpm
```

3.查看mysql57的安装源是否可用，如不可用请自行修改配置文件（/etc/yum.repos.d/mysql-community.repo）使mysql57下面的enable=1

```
yum repolist enabled | grep mysql
```

4.使用yum安装mysql（一路y）

```
yum install mysql-community-server
```

5.启动mysql服务

```
service mysqld start
```

 6.查看mysql是否自启动,并且设置开启自启动

```
chkconfig --list | grep mysqld
chkconfig mysqld on
```

7.修改字符集为UTF-8

```
vim /etc/my.cnf
```

在[mysqld]部分添加：

```
character-set-server=utf8
```

在文件末尾新增[client]段，并在[client]段添加：

```
default-character-set=utf8
```

esc退出编辑  :wq保存退出
修改完成后保存重启服务

```
 service mysqld restart
```

8.修改默认配置(按照提示进行配置  密码呀啥的)

```
mysql_secure_installation
```

9.授权远程登录（这样机器就可以以用户名root密码root远程访问该机器上的MySql.）
方法一：实现远程连接(授权法)
将权限改为ALL PRIVILEGES

```
mysql -uroot -p
mysql> use mysql;
Database changed
mysql> flush privileges;
mysql> select host,user,password from user;
```

方法二：实现远程连接（改表法）

登录后：

```
use mysql;
update user set host = '%' where user = 'root';
fiush privileges
```

这样在远端就可以通过root用户访问Mysql.

# 安装mysql5.6解压版（centos6）

方法二（sxt）：

稳妥的方式

1.百度网盘资源

```
链接：https://pan.baidu.com/s/1AXeZWz2s_L2I6lpeK2CLTA 
提取码：7gbl 

```

2.解压

```
tar -zxvf mysql-5.6.31-linux-glibc2.5-x86_64.tar.gz
```

3.复制到/usr/local/mysql

```
cp -r 原名称  /usr/local/mysql
```

4.进入mysql文件夹

```
cd /usr/local/mysql
```

5.Root用户是最高权限用户，所以一般都是创建用户和用户组，放置最高权限用户进行操作。
添加用户组，命名为mysql

```
groupadd mysql
```

创建用户mysql，并指定所属群组为mysql

```
useradd -r  -g mysql mysql
```

6.赋权，让用户组和用户具有操作权限

注意
下面命令中有. 表示本级目录
一定要保证当前所在文件夹是/usr/local/mysql中

变更mysql用户组有操作当前文件夹的权限

```
chgrp  -R mysql .
```

变更mysql用户具有操作本级目录的权限。

```
chown -R mysql .
```

上面两个命令也可以换成下面一条命令

```
chown -R mysql:mysql ./
```

7.初始化

注意：以下命令需要保证在/usr/local/mysql下

 判断/etc/my.cnf是否存在，如果存在删除

```
ls /etc/my.cnf
```

 如果存在执行下面命令，如果不存在，跳过此步骤

```
rm /etc/my.cnf
```

初始化数据库

```
./scripts/mysql_install_db --user=mysql
```

8.修改配置文件

注意：配置my.cnf和启动文件，根据自己的需要进行修改。如果不需要特殊操作，可以直接复制.
以下命令依然需要保证目前在mysql文件夹下

复制my.cnf文件
命令：

```
cp support-files/my-default.cnf /etc/my.cnf
```

复制启动文件

```
cp support-files/mysql.server      /etc/rc.d/init.d/mysql
```

9.启动、重启、关闭mysql服务

注意：Mysql必须在启动状态下，才可以修改密码（下一步骤才可以做)

```
启动mysql服务：
命令：service mysql start
关闭服务：
命令：service mysql stop
重启服务：
命令：service mysql restart
```

10.操作mysql数据库

注意：如果以上的配置都正确执行，可以直接输入mysql进入到mysql编辑模式

```
命令：mysql –u root –p
会提示要求输入密码
如果提示没有mysql命令，需要添加软连接
ln -s /usr/local/mysql/bin/mysql /usr/bin/mysql

进入到mysql命令后，出现[mysql>]
根据自己的需要创建数据库，创建表等CRUD操作


```

11.附：忘记root密码后的修改方式（可跳过）

```
进入/etc/my.cnf 在[mysql]下添加skip-grant-tables 启动安全模式
命令：vi /etc/my.cnf
重启服务：
命令：service mysql restart
登录mysql，输入密码时直接回车
命令:  mysql -u root -p
进入到mysql后，先使用mysql数据库
命令：use mysql
修改密码
命令： update user set password= passworD ("123456") where user='root';
刷新权限
命令： flush privileges;
退出MySql编辑模式
命令：exit
```

12.设置用户具有访问的权限

进入mysql命令行

```
进入mysql命令行
```

执行权限赋予命令

```
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'smallming' WITH GRANT OPTION; 
```

刷新权限

```
flush privileges;
```

退出

```
quit
```

防火墙

```
 vim  /etc/sysconfig/iptables 
 把包含22行复制一行,修改22为8080
 8080:9000 从8080到9000全放行
重启服务

service iptables restart

 restart 重启
 start 启动
 stop 停止
```



# 安装mysql5.7（centos7 _解压版）

百度网盘下载

```
链接：https://pan.baidu.com/s/1d_6iNbIif6eNkTHczGpxww 
提取码：f1g1 
```

1.注意 该步骤为提示，可不执行，第三步执行 （在Centos7中用MariaDB代替了mysql数据库。所以在新安装MySQL前必须做好对系统的清理工作。

```
rpm -qa|grep mariadb
rpm -e mariadb-libs-5.5.60-1.el7_5.x86_64 --nodeps（注意修改名字））第一步只是告知，可略过。（第三步具体执行）
```

2.上传解压包到Linux，并执行解压：
tar -xvf mysql-5.7.23-1.el7.x86_64.rpm-bundle.tar

3.卸载冲突的RPM组件
在我们安装mysql相关组件的时候，如果不将此冲突的组件删除掉，我们是安装不成功的。
查看postfix和mariadb-libs相关的组件：

注意修改名字  不一定和命令中相同。

```
rpm -qa | grep postfix
rpm -qa | grep mariadb
rpm -e --nodeps postfix-2.10.1-6.el7.x86_64
rpm -e --nodeps mariadb-libs-5.5.35-3.el7.x86_64
```

4.安装相应的依赖 ：

```
yum -y install libaio
yum -y install net-tools
yum -y install perl
```

5.经过上面的解压操作，我们得到了很多rpm文件。
但是我们不需要这么多，我们只需要安装以下四个组件就可以

```
rpm -ivh mysql-community-common-5.7.23-1.el7.x86_64.rpm
rpm -ivh mysql-community-libs-5.7.23-1.el7.x86_64.rpm
rpm -ivh mysql-community-client-5.7.23-1.el7.x86_64.rpm
rpm -ivh mysql-community-server-5.7.23-1.el7.x86_64.rpm
```

6.修改MySQL密码
mysql安装完成之后我们是没有设置密码的，但是mysql为我们设置了一个临时的密码，
我们可以查看mysql的日志知道这个临时密码，查看临时密码前确保数据库启动。
（1）启动数据库

```
查看mysql是否启动：service mysqld status
启动mysql：service mysqld start
停止mysql：service mysqld stop
重启mysql：service mysqld restart
```

（2）查看临时密码：

```
grep password /var/log/mysqld.log
```

（注意：localhost后面即密码  即 localhost:密码）
登录mysql（命令：mysql -p），输入临时密码（区分大小写，并且括号等特殊字符也一定要输入）
登录后设置新密码：

```
set password = password("123456");
```

退出当前登录：

```
quit;
```

7.允许远程连接
开启mysql远程访问权限，允许远程连接

```
mysql -u root -p
use mysql;
update user set host = '%' where user = 'root';
flush privileges;
```

8.放行3306端口防火墙

```
firewall-cmd --zone=public --add-port=3306/tcp --permanent
firewall-cmd --reload
```

第8步之后安装已完成，以下作为扩展

9、创建数据库，并指定UTF-8编码
CREATE DATABASE 数据库名 CHARACTER SET utf8 COLLATE utf8_general_ci;

10、命令行导入数据库
mysql -h localhost -u root -p 数据库名< /home/fps001.sql

11、命令行导出数据库
mysqldump -h localhost -u root -p 数据库名> /home/fps001.sql

在centos7：查看端口 pid等信息
netstat -apn 

查看某端口下具体信息

netstat -apn | grep 3306  



### centos7本地安装MySQL数据库8.0.11版本

说明：下载8.0.15雷同。修改相关名字即可。

附上链接（8.0.15）：

```
链接：https://pan.baidu.com/s/1cjTbAIzRZtOAyxsFiqN2PQ 
提取码：u6tf 
```

- MySQL本地安装文件上传到Linux

- 百度网盘链接（8.0.11）：

- ```
  链接：https://pan.baidu.com/s/1inn9yRVhlgnf7gafOuoXsg 
  提取码：jd3s 
  ```

- 执行解压缩

  注意：8.0.11版本由于我提供的已经解压，可略过此步骤  8.0.15需要。但是需要改下压缩包名字

  ```shell
  tar xvf mysql-8.0.11-1.el7.x86_64.rpm-bundle.tar
  ```

- 安装依赖的程序包

  ```shell
  yum -y install libaio -y
  yum -y install net-tools -y
  yum -y install perl -y
  ```

- 卸载冲突的RPM组件

  ```shell
  rpm -qa | grep postfix
  rpm -qa | grep mariadb
  rpm -e --nodeps postfix-2.10.1-6.el7.x86_64
  rpm -e --nodeps mariadb-libs-5.5.35-3.el7.x86_64
  ```

- 安装MySQL程序包

  注意：安装8.0.15需要改名字

  ```shell
  rpm -ivh mysql-community-common-8.0.11-1.el7.x86_64.rpm 
  rpm -ivh mysql-community-libs-8.0.11-1.el7.x86_64.rpm 
  rpm -ivh mysql-community-client-8.0.11-1.el7.x86_64.rpm 
  rpm -ivh mysql-community-server-8.0.11-1.el7.x86_64.rpm 
  ```

- 修改MySQL目录权限

  ```shell
  chmod -R 777 /var/lib/mysql/
  ```

- 初始化MySQL

  ```shell
  mysqld --initialize
  chmod -R 777 /var/lib/mysql/*
  ```

- 启动MySQL

  ```shell
  service mysqld start
  相关其他命令：
  查看mysql是否启动：service mysqld status
  启动mysql：service mysqld start
  停止mysql：service mysqld stop
  重启mysql：service mysqld restart
  ```

- 查看初始密码

  ```shell
  grep 'temporary password' /var/log/mysqld.log
  ```

- 登陆数据库之后，修改默认密码

  ```mysql
  mysql -uroot -p（登录：输入初始密码）
  alter user user() identified by "123456"; 
  ```

- 允许远程使用root帐户

  ```mysql
  use mysql;
  UPDATE user SET host = '%' WHERE user ='root';
  FLUSH PRIVILEGES;
  ```

- 允许远程访问MySQL数据库（/etc/my.cnf）

  ```ini
  vim /etc/my.cnf
  character_set_server = utf8
  bind-address = 0.0.0.0
  ```

- 开启防火墙3360端口

  ```shell
  firewall-cmd --zone=public --add-port=3306/tcp --permanent
  firewall-cmd --reload
  ```

如果最后执行不能远程连接：

```
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '123456'; 
```

# linux设置mysql开机启动

注意：针对于mysql5.6

1、 将服务文件拷贝到init.d下，并重命名为mysql

```
cp /usr/local/mysql/support-files/mysql.server /etc/init.d/mysql  
```

2、赋予可执行权限

```
chmod +x /etc/init.d/mysql    
```

3、添加服务

```
chkconfig --add mysql
```

4、显示服务列表

```
chkconfig --list
```

如果看到mysql的服务，并且3,4,5都是on的话则成功

如果是off，则键入chkconfig --level 345 mysql on

5、重启电脑

```
reboot
```

6、如果看到有监听说明服务启动了

```
netstat -na | grep 3306
```

