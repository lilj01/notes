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

