#### 1.docker 启动mysql 8.0.18

```
docker run -p 8806:3306 --name CL_mysql -e MYSQL_ROOT_PASSWORD=123456 -d mysql:8.0.18
```



#### 2.连接报错解决

但是在本地用Navicat登录的时候，发现报错了。报错信息

连接Docker启动的mysql出现：**ERROR 2059 (HY000): Authentication plugin 'caching_sha2_password' cannot be loaded**

解决方案：

##### 1.进入mysql容器

```shell
docker exec -it CL_mysql /bin/bash
```

##### 2.进入mysql

```shell
mysql -uroot -P 3306 -p
```

##### 3.修改密码

```sql
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '123456';

flush privileges;
```

修改好了之后，再用Navicat登录就可以了。

