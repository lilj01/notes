## vsftpd:

service vsftpd start  启动
service vsftpd stop  关闭
service vsftpd restart  重启



## 后台启动springboot

nohup java -jar mkill.jar >>sto.out &



# nginx  

./sbin/nginx   启动
./sbin/nginx  -s reload  重启

# 

# redis

./src/redis-server redis.conf  启动服务
./src/redis-cli   启动命令行



# linux (centos6)

netstat -tulpn 查看进程号  端口pid等。。
防火墙  /etc/sysconfig/iptables 
重启防火墙 service iptables restart



# 放行端口(centos6)

 vim  /etc/sysconfig/iptables 
 把包含22行复制一行,修改22为8080
 8080:9000 从8080到9000全放行
重启服务

service iptables restart

 restart 重启
 start 启动
 stop 停止





# yum安装命令：

yum install -y wget

yum install -y vim



# 修改yum源（centos6）

1.备份原始源
cp /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.bak

2.创建新源
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo

3.重新缓存
yum clean all
yum makecache



# 修改yum源(阿里云)

CentOS
1、备份
cp /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup

2、下载新的CentOS-Base.repo 到/etc/yum.repos.d/

CentOS 6执行：
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo
或者
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo

CentOS 7执行：
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
或者
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

3、之后运行yum makecache生成缓存

4、其他
非阿里云ECS用户会出现 Couldn't resolve host 'mirrors.cloud.aliyuncs.com' 信息，不影响使用。用户也可自行修改相关配置: eg:

sed -i -e '/mirrors.cloud.aliyuncs.com/d' -e '/mirrors.aliyuncs.com/d' /etc/yum.repos.d/CentOS-Base.repo



# centos7没有netstat命令的解决办法

yum search ifconfig

通过yum search 这个命令我们发现ifconfig这个命令是在net-tools.x86_64这个包里，接下来我们安装这个包就行了

运行  yum install net-tools  就OK了



# Linux环境配置jdk:

## 方法一：上传法（下载到Windows再通过xftp等工具上传至Linux）

注意：要保证安装wget命令。并且把yum源更换了。

下载JDK链接
https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html

赋权限 
chmod +x jdk-8u144-linux-x64.rpm

//安装RPM软件包
rpm -ivh jdk-8u144-linux-x64.rpm

//查看java的版本信息，若出现版本信息则成功
java –version

## 方法二：（wget下载）

1.新建目录 /usr/local/jdk1.8（配置环境变量时是这个，建议一致，不然还得改）
wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u131-b11/d54c1d3a095b4ff2b6607d096fa80163/jdk-8u131-linux-x64.rpm
说明：
wget为：下载方式
--no-check-certificate：用于禁止检查证书
--no-cookies：用于禁用Cookies
--header=header-line：用于定义请求头信息
http...即为jdk1.8下载链接

2.添加执行权限
chmod 755 jdk-8u131-linux-x64.rpm

3.执行rpm命令安装:
rpm -ivh jdk-8u131-linux-x64.rpm

4.查看是否安装成功：
java -version

5.配置环境变量
vim /etc/profile 
    export  JAVA_HOME=/usr/java/jdk1.8.0_131
    export  CLASSPATH=.:$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
    export  PATH=$PATH:$JAVA_HOME/bin

6.生效
source /etc/profile

注意： 2  4 5 6可以替换成 chmod +x  jdk-8u131-linux-x64.rpm 然后执行3

7.测试
touch HelloWorld.java
vim HelloWorld.java(编写HelloWord)
javac HelloWorld.java
java HelloWorld（打印HelloWord成功（代码写错了就回去练手撕代码吧））



# redis通过命令安装：

## 方法一（wget）

1.下载
wget http://download.redis.io/releases/redis-4.0.11.tar.gz

解压

tar -zxvf redis-4.0.11.tar.gz

移动到安装目录下

mv redis-4.0.11 /usr/local/redis/

2.安装命令（因为redis是c语言写的 需要编译）
yum install gcc

3.编译
make MALLOC=libc

4.安装
cd src 
make install

5.修改redis.conf配置文件
说明：主要的几项
修改redis.conf   注释127.0.0.0那行
daemonize 修改为yes
Protectedmode no
关闭保护模式
是否需要密码 requirepass

6. 启动服务  

   ./src/rerdis-server redis.conf

7. 启动客户端命令
   ./src/redis-cli
   如果有密码  auth 密码



方法二（下载到Windows再通过xftp等工具上传至Linux）
https://redis.io/download
解压
tar -zxvf red...
移动、更名
mv redis.. /usr/local/redis/
make
编译
make install
修改redis.conf   注释127.0.0.0那行
daemonize 修改为yes
Protectedmode no
关闭保护模式
是否需要密码 requirepass

启动服务
./src/redis-server  redis.conf

启动客户端命令
./src/redis-cli
如果有密码  auth 密码
关闭
启动客户端之后输入shutdown  quit



# Linux下Redis开机自启（Centos6）

1、设置redis.conf中daemonize为yes,确保守护进程开启。

2、编写开机自启动脚本

```
vim /etc/init.d/redis
```

脚本内容如下：

```
# chkconfig:   2345 90 10

# description:  Redis is a persistent key-value database 

PATH=/usr/local/bin:/sbin:/usr/bin:/bin   
REDISPORT=6379  
# 自己的redis-server路径(需要自己更改)
EXEC=/usr/local/redis/src/redis-server   
REDIS_CLI=/usr/local/redis/src/redis-cli   

PIDFILE=/var/run/redis.pid
# 自己的redis.conf 路径(需要自己更改)
CONF="/usr/local/redis/redis.conf"  
AUTH="1234"  

case "$1" in   
        start)   
                if [ -f $PIDFILE ]   
                then   
                        echo "$PIDFILE exists, process is already running or crashed."  
                else  
                        echo "Starting Redis server..."  
                        $EXEC $CONF   
                fi   
                if [ "$?"="0" ]   
                then   
                        echo "Redis is running..."  
                fi   
                ;;   
        stop)   
                if [ ! -f $PIDFILE ]   
                then   
                        echo "$PIDFILE exists, process is not running."  
                else  
                        PID=$(cat $PIDFILE)   
                        echo "Stopping..."  
                       $REDIS_CLI -p $REDISPORT  SHUTDOWN    
                        sleep 2  
                       while [ -x $PIDFILE ]   
                       do  
                                echo "Waiting for Redis to shutdown..."  
                               sleep 1  
                        done   
                        echo "Redis stopped"  
                fi   
                ;;   
        restart|force-reload)   
                ${0} stop   
                ${0} start   
                ;;   
        *)   
               echo "Usage: /etc/init.d/redis {start|stop|restart|force-reload}" >&2  
                exit 1  
esac
```

4、设置权限

```
cd /etc/init.d/

chmod 755 redis
```

5、启动测试

```
/etc/init.d/redis start
```

启动成功会提示如下信息：

Starting Redis server… 
Redis is running… 

6、设置开机自启动

```
chkconfig redis on
```

7.重启

```
reboot 
```



# centos 7环境下下载安装并配置redis4*开机启动

1.下载（首先进入目录  cd /usr/local/src ）

```
wget http://download.redis.io/releases/redis-4.0.2.tar.gz
```

2．进入/usr/local/src目录下，解压redis安装文件

```
tar -zxvf redis-4.0.2.tar.gz
```

3.进入解压后的文件目录，之后直接编译即可

```
cd /usr/local/src/redis-4.0.2
make
make install
```

4.创建存储redis文件目录

```
mkdir -p /usr/local/redis
```

5.复制redis-server redis-cli到新建立的文件夹

```
cp /usr/local/src/redis-4.0.2/src/redis-server /usr/local/redis/

cp /usr/local/src/redis-4.0.2/src/redis-cli /usr/local/redis/
```

6.复制redis的配置文件

```
cp /usr/local/src/redis-4.0.2/redis.conf /usr/local/redis/
```

7.7．编辑配置文件

```
vim /usr/local/redis/redis.conf
```

　注意：

​    　①　在bind 127.0.0.1前加“#”将其注释掉

　　②　默认为保护模式，把 protected-mode yes 改为 protected-mode no

　　③　默认为不守护进程模式，把daemonize no 改为daemonize yes

　　④　将 requirepass foobared前的“#”去掉，密码改为你想要设置的密码（如果不想设置密码此项可忽略）

　　以上修改完成后，保存退出。

8.编辑redis开机启动redis脚本

```
vim /etc/init.d/redis
```

   加入如下内容：

   注意：如果第7部中启用了密码，此处需要添加该项  AUTH="123456"  即第10行  否则注释即可

```
#!/bin/bash
# chkconfig: 2345 10 90
# description: Start and Stop redis
#PATH=/usr/local/bin:/sbin:/usr/bin:/bin
REDISPORT=6379
EXEC=/usr/local/redis/redis-server
REDIS_CLI=/usr/local/redis/redis-cli
PIDFILE=/var/run/redis_6379.pid
CONF="/usr/local/redis/redis.conf"
AUTH="123456" 

case "$1" in
    start)
        if [ -f $PIDFILE ]
        then
                echo "$PIDFILE exists, process is already running or crashed"
        else
                echo "Starting Redis server..."
                $EXEC $CONF
        fi
        if [ "$?"="0" ]
        then
              echo "Redis is running..."
        fi
        ;;
    stop)
        if [ ! -f $PIDFILE ]
        then
                echo "$PIDFILE does not exist, process is not running"
        else
                PID=$(cat $PIDFILE)
                echo "Stopping ..."
                $REDIS_CLI -p $REDISPORT SHUTDOWN
                while [ -x ${PIDFILE} ]
               do
                    echo "Waiting for Redis to shutdown ..."
                    sleep 1
                done
                echo "Redis stopped"
        fi
        ;;
   restart|force-reload)
        ${0} stop
        ${0} start
        ;;
  *)
    echo "Usage: /etc/init.d/redis {start|stop|restart|force-reload}" >&2
        exit 1
esac
```

9.添加开机启动服务

```
vim /etc/rc.local
```

在末尾加入以下内容

```
service redis start
```

10.设置权限

```
chmod 755 /etc/init.d/redis
```

11.注册系统服务

```
chkconfig --add redis
```

注意：

如果提示“service redis does not support chkconfig”的信息，说明第8部中脚本内容有问题，可以参考：

```
https://www.cnblogs.com/niocai/archive/2012/07/12/2587780.html
```

12.测试redis服务（启动   停止  服务）

```
service redis start
service redis stop
```

13.创建redis命令软连接

```
ln -s /usr/local/redis/redis-cli /usr/bin/redis
```

14.输入redis即可启动客户端

注意：如果有密码则需要 auth 密码

```
redis
```



# wget下载tomcat8

1.下载
wget https://archive.apache.org/dist/tomcat/tomcat-8/v8.0.23/bin/apache-tomcat-8.0.23.tar.gz
2.解压
tar -zxvf apache-tomcat-8.0.23.tar.gz
3.移动 
mv apache-tomcat-8.0.23.tar.gz /usr/local/tomcat/
4.启动
cd /usr/local/tomcat
./bin/start.sh
5.查看启动日志
tailf logs/catalina.out


centos7防火墙：
firewall-cmd --zone=public --add-port=3306/tcp --permanent
firewall-cmd --reload



## 最坑爹的Mysql安装，做好心理准备

# 安装mysql5.6（centos6）：

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

# 安装mysql5.6（centos6）

方法二（北京尚学堂）：

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



# 安装mysql（centos7 _解压版）

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



# linux常用命令

- 创建文件命令

  ```shell
  touch demo.txt
  ```

- 编辑文件命令

  ```shell
  vi demo.txt
  ```

- 创建文件夹命令

  ```shell
  mkdir school
  ```

- 删除文件或者文件夹命令

  ```shell
  rm -rf demo.txt
  ```

  ```shell
  rm -rf shcool
  ```

- 查看运行中的进程列表

  ```shell
  ps aux
  ```

- 关闭进程

  ```shell
  kill -9 进程编号
  ```

- 关闭SELinux（安全机制，影响很多东西，关了省事儿）

  - 编辑文件

    ```shell
    vi /etc/selinux/config
    ```

  - 设置 SELINUX=disabled，重启系统reboot