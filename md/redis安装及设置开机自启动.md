# redis安装：

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

