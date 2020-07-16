# 通过Docker安装RabbitMQ

以启动一个rabbitmq节点为例，两条命令可搞定:

/www/rabbitmq目录可自定义，主要用于目录挂载

`mkdir -p /www/rabbitmq`

`docker run -d --hostname rabbit-node1 --name rabbit-node1 -p 5672:5672 -p15672:15672 -v /www/rabbitmq:/var/lib/rabbitmq rabbitmq:management`

***

查看docker容器状态:

`docker ps | grep rabbit`

浏览器打开登录rabbitmq, 入口: 

http://localhost:15672

默认用户名: guest 密码: guest

