# 主从配置 #

**docker 启动俩个容器**
`docker run --name mysql_master -p 13306:3306 -v /usr/local/mysql/data:/var/lib/mysql -v /usr/local/mysql/conf.d:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.7`

`docker run --name mysql_slave -p 13307:3306 -v /usr/local/mysql/data:/var/lib/mysql -v /usr/local/mysql/conf.d:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=123456  -d mysql:5.7`


**删除镜像和容器后要重启docker:** 

`systemctl restart docker`

**容器内安装vim:**

`apt-get update`

`apt-get install vim`

**新安装的mysql当使用root登录的时候不知道密码，导致登录失败，所以先修改密码，流程如下：**

1.提示：'/var/run/mysqld/mysqld.sock'找到/etc/mysql/mysql.conf.d/mysqld.cnf，[mysqld]下面加 protocol = tcp

2.找到/etc/mysql/my.cnf, cat 查看 发现!includedir /etc/mysql/mysql.conf.d/mysqld.cnf， 我们可以到这个文件夹修改配置

3.在[mysqld] 下面添加：skip-grant-tables，然后重启mysql：

`/etc/init.d/mysql restart` 

启动是：`/etc/init.d/mysql start`

4.`show databases;` `use mysql;` `show tables;` `update user set authentication_string=PASSWORD('123456') where user='root';` 

重启密码为123456，并授权：

`flush privileges;`

5.找到/etc/mysql/mysql.conf.d/mysqld.cnf 注释掉：skip-grant-tables 然后重启mysql

6.可以正常使用root登录了：

`mysql -u root -p`, 输入123456


**master配置**

1.创建repl用户:

`grant replication slave on *.* to 'repl'@'%' identified by '123456';` 

并授权：

`flush privileges;`

2.查看状态：

`show master status;` 

如果显示：Empty set (0.00 sec)，查看是否开启binlog：

`binlog:show variables like 'log_%';`

找到/etc/mysql/mysql.conf.d/mysqld.cnf 在[mysqld]下面加上：log-bin=mysql-bin和 server-id=1（必须唯一），重启mysql

3.成功显示binlog：

`show master status;`

**slave配置**

1.运行容器，进入容器，登录：

`mysql -u root -p`

1.`CHANGE MASTER TO MASTER_HOST='master的ip',MASTER_PORT=3306,MASTER_USER='repl',MASTER_PASSWORD='123456',MASTER_LOG_FILE='mysql-bin.000001',MASTER_LOG_POS=0;`

2.只读不写：`set global read_only=1;`

3.`vim /etc/mysql/mysql.conf/mysqld.cnf: log-bin=mysql-binlog server-id=2`

4.`show slave status\G`
