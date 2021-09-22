

命令：

```
docker run -p 3306:3306 --name mysql -v /usr/mydata/mysql/log:/var/log/mysql -v /usr/mydata/mysql/data:/var/lib/mysql -v /usr/mydata/mysql/conf:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=123456 -d mysql:latest
```



分析：

```

    docker run -d mysql:latest             以后台的方式运行 mysql 版本的镜像，生成一个容器。
    --name mysql                           容器名为 mysql
    -e MYSQL_ROOT_PASSWORD=123456          设置登陆密码为 123456，登陆用户为 root
    -p 3306:3306                           将容器内部 3306 端口映射到 主机的 3306 端口，即通过 主机的 3306 可以访问容器的 3306 端口
    -v /usr/mydata/mysql/log:/var/log/mysql    将容器的 日志文件夹 挂载到 主机的相应位置
    -v /usr/mydata/mysql/data:/var/lib/mysql   将容器的 数据文件夹 挂载到 主机的相应位置
    -v /usr/mydata/mysql/conf:/etc/mysql/conf.d   将容器的 自定义配置文件夹 挂载到主机的相应位置
    
```





查看容是否器启动

```
docker ps -a
```