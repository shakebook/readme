## mysql基础 ##

**docker安装myslq**

拉取镜像：`docker pull mysql:latest`

查看本地镜像：`docker images`

运行mysql容器：

```

docker run -itd --name mysql_master -p 13306:3306 -v /usr/local/mysql/data:/var/lib/mysql -v /usr/local/mysql/conf.d:/etc/mysql/conf.d -e \ MYSQL_ROOT_PASSWORD=123456 mysql

```

分别宿主机上挂在数据和配置文件：`-v /usr/local/mysql/data:/var/lib/mysql -v /usr/local/mysql/conf.d:/etc/mysql/conf.d`

配置数据库时区和utf8等：

配置utf8,支持中文：

在宿主机/usr/local/mysql/conf.d目录下新建my.cnf文件，粘贴以下内容：

```

[mysqld]
character-set-server=utf8 
[client]
default-character-set=utf8 
[mysql]
default-character-set=utf8

```

配置数据库时区:

查看时区：`show variables like '%time_zone%';`

如果时区不是东八区，要修改配置：

编辑宿主机`vim /usr/local/mysql/conf.d/my.cnf`，在`mysqld`下边的配置中添加一行：

`default-time_zone = '+8:00'`

注意：修改配置文件后，要重新启动容器：`docker restart 容器id/容器名`

进入mysql容器里：`docker exec -it mysql-test /bin/bash`

登录mysql：`mysql -u root -p`

密码：123456

**mysql基本管理**

显示所有数据库：`show databases;`

选择名称为mysql的数据库：`use mysql;`

创建名称为testName的数据库：`create database testName`

删除名称为testName的数据库：`drop database testName`

**mysql数据备份**

导出数据：`mysqldump -uroot -p --all-databases > /var/lib/sqlfile.sql`

导入数据：`mysql -u root -p && source /var/lib/sqlfile.sql`

如果是目标是远程，使用scp即可。


**修改数据库名称**

```
#!/bin/bash

mysql -uroot -p 123456 -e 'create database if not exists blog'
list_table=$(mysql -uroot -p 123456 -Nse "select table_name from information_schema.TABLES where TABLE_SCHEMA='yang_files'")

for table in $list_table
do
    mysql -uroot -p 123456 -e "rename table yang_files.$table to blog.$table"
done

```

数据库名称由yang_files改为blog