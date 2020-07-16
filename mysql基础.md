## mysql基础 ##

**docker安装myslq**

拉取镜像：`docker pull mysql:latest`

查看本地镜像：`docker images`

运行mysql容器：

`docker run -itd --name mysql-test -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql`

进入mysql容器里：`docker exec -it mysql-test /bin/bash`

登录mysql：`mysql -u root -p`

密码：123456

**mysql基本管理**

显示所有数据库：`show databases;`

选择名称为mysql的数据库：`use mysql;`

创建名称为testName的数据库：`create database testName`

删除名称为testName的数据库：`drop database testName`