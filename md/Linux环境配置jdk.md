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

