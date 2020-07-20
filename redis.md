# redis笔记#

**使用docker安装redis**

拉取境像：`docker pull docker.io/redis`

运行容器：

``
docker run --name dev-redis -p 6379:6379 -v /usr/data/redis:/data -d redis:latest redis-server --appendonly yes
```

参数解释：

`-p 6379:6379` : 将容器的6379端口映射到主机的6379端口

`-v /usr/data/redis:/data` : 将主机中/usr/data/redis目录挂载到容器的/data

-d ： 后台运行

`redis-server --appendonly yes` : 在容器执行redis-server启动命令，并打开redis持久化配置

Redis设置密码 : `config set requirepass 123456`

查询密码：`config get requirepass`

密码验证：`auth 123456`