服务端微专业环境安装说明
=================================

## 服务器

本示例使用Ubuntu Server作为部署服务器。你可以选择使用我们已经安装好各个组件的示例镜像，也可以直接下载Ubuntu的原始镜像，按照安装步骤一步步安装。示例镜像基于VirtualBox，你需要根据下载的示例镜像导入虚拟机。

### 示例镜像地址
	http://mirrors.163.com/ubuntu-releases/14.04.3/ubuntu-14.04.3-server-amd64.iso

### 原始镜像地址
	http://mirrors.163.com/ubuntu-releases/14.04.3/ubuntu-14.04.3-server-amd64.iso

## 组件安装及启动

### Java
	% sudo apt-get install openjdk-7-jdk

### Tomcat
	% wget http://mirrors.cnnic.cn/apache/tomcat/tomcat-7/v7.0.64/bin/apache-tomcat-7.0.64.tar.gz
	% tar -xvzf apache-tomcat-7.0.64.tar.gz
	% cd apache-tomcat-7.0.64/bin
	% ./catalina.sh start

### MySQL
	% sudo apt-get install mysql-server-5.6
	% sudo service mysql status # 先检查运行状态，如果未运行，使用start启动，下同
	% sudo service mysql start

### Redis
	% sudo apt-get install redis-server
	% sudo service redis-server status
	% sudo service redis-server start

### Nginx
	% sudo apt-get install nginx
	% sudo service nginx status
	% sudo service nginx start

## 创建数据库
	% mysql -h 127.0.0.1 -u root -pserver # 连接到MySQL
	
	mysql > create database example default character set utf8;
	mysql > create user 'server'@'%' identified by 'example';
	mysql > grant all on example.* to 'server'@'%' identified by 'example';
	mysql > CREATE TABLE `User` (
  			`id` int(11) unsigned NOT NULL AUTO_INCREMENT,
  			`userName` varchar(50) NOT NULL DEFAULT '',
  			`userPassword` varchar(50) NOT NULL DEFAULT '',
  			`userDesc` varchar(100) NOT NULL DEFAULT '',
  			PRIMARY KEY (`id`)
			) ENGINE=InnoDB DEFAULT CHARSET=utf8;
	mysql > INSERT INTO `User` (`userName`, `userPassword`, `userDesc`) VALUES
			 ('test_user', 'test_password', 'Test user for server example'); # 插入一个测试用户，用于后续登录

## 部署示例工程

	% git clone https://git.oschina.net/server-dev/server-example.git	% mvn package	% cp server-example.war apache-tomcat-7.0.64/webapps	
## 访问
	
访问如下地址：http://10.240.155.161/server-example/，输入数据库插入的测试用户名（test_user）及密码（test_password），点击登录后，跳转到用户信息面。