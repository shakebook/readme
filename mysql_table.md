## mysql表结构基本知识##

**１、datetime时区误差，设置**

`vim /etc/mysql/mysql.conf.d/mysqld.cnf` 加入: `default-time-zone='+8:00'`,然后重启: `service mysql restart`