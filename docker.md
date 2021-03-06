# docker

**install docker**

安装文档： `https://docs.docker.com/engine/install/centos/`

国内镜像：

`sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo`

`sudo apt-get update`

```
sudo apt-get install

apt-transport-https ca-certificates

curl gnupg-agent software-properties-common

```

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

```

```
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable

apt-cache madison docker-ce

sudo docker run hello-world

```

**ohter install method**

```
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io
```

1.Docker 要求 Ubuntu 系统的内核版本高于 3.10

**2.通过 uname -r 命令查看你当前的内核版本:**

`uname -r`

**3.获取最新版本的 Docker 安装包:** 

`wget -qO- https://get.docker.com/ | sh ` 或 uduntu下安装：

`sudo apt-get update`

**4.安装docker:** 

`sudo apt-get install -y docker.io`

启动docker: 

`sudo service docker start`

重启docker:

`systemctl restart docker`

**5.拉去镜像：**

`docker pull mysql:5.7`

**6.查看镜像列表：**

`docker images`

**7.查看已启动的容器：**

`docker ps`

**8.查看所有容器列表：** 

`docker ps -a`

**9.删除本地镜像：**

`docker rmi 镜像id`

**10.强制删除本地镜像：**

`docker rmi -f 镜像id`

**11.删除已启动的容器：**

`docker rm -f 容器id`

**12.删除已停止的容器：**

`docker rm 容器id`

**13.停止容器运行：**

`docker stop 容器id`

**14.启动容器：**

`docker start 容器id`

**15.进入容器：**

`docker exec -it 容器id /bin/bash`

**重启所有容器：**
`docker restart $(docker ps -q)`

**docker制作镜像**

有2中方式：1、源码拷贝 2、可执行程序拷贝，推荐使用2，第一种需要go mod,go mod download速度会很慢

把程序`go build -o` 输出到Dockerfile相同目录中,Dockerfile定义如下：

```
FROM centos:7

ADD . /

RUN chmod 777 /orm

ENV PARAMS=""

ENTRYPOINT ["sh","-c","/orm $PARAMS"]

```

解释：

```

FROM centos:7 # 基础镜像

ADD . /  # 从Dockerfile所在目录中把可执行程序拷贝到docker daemon 根目录/

RUN chmod 777 /orm #添加文件权限


ENV PARAMS="" #从命令行中获取参数

ENTRYPOINT ["sh","-c","/orm $PARAMS"] # 执行命令

```

**docker虚拟机**

在做开发的时候，我们有时候需要搭建集群，传统的方式通过Hyper-V、VMware、VirtualBox、XenServer、Kvm等方式，

docker横空出世后，这些工具也将成为历史。下面我门使用docker如何创建有固定ip的虚拟机。

你可以制作你喜欢的系统镜像，这里我以centos为例子，

1. 登录hub.docker.com: `docker login hub.docker.com` 这里也可以是你的私有仓库

拉取镜像: `docker pull centos`

这里默认是centos:latest,如果hub.docker.com没有你需要的版本，你可以自己制作一个制定版本的镜像。

2. 创建自定义网络类型，并指定网段：

查看已有的网络类型： `docker network ls`

删除已有网络类型： `docker network rm 网络类型名称`

创建网络，并指定网段：`docker network create --subnet=182.182.0.0/24 cluster`

网段：`182.182.0.0/24` 网络类型名字：cluster

根据centos镜像启动一个容器：

`docker run -itd --name box1 --hostname box1 --net cluster --ip 182.182.0.11 centos`

```
解释：

-itd: 以交互命令方式启动，可以使用 -it /bin/bash方式进入容器，-d后台运行

--name:容器的名称

--hostname: 容器里面系统主机的名称

--net: 网络类型名称

--ip: 容器里面系统指定静态ip

centos: 镜像tag

```

进入容器： `docker exec -it box1 /bin/bash`

查看网络是否正常：`yum install vim` 如果不能下载，看看是否网段是否错误

查看hosts: `vim /etc/hosts`

docker ps -q

停止所有运行的容器：`docker stop $(docker ps -q)`

删除所有容器：`docker rm $(docker ps -aq)`

删除所有镜像: docker rmi $(docker images -q) -f

docker使用技巧--解决中文乱码的问题 :

1.进入docker，执行 vim /etc/profile 打开profile文件

2.将“export LANG="C.UTF-8”命令添加在profile文件最后,保存退出

3.执行 source /etc/profile ，即可正常显示中文

**docker挂载配置文件**

`docker run -itd --name blog-account -p 9001:9001 -v /opt/work/conf.yaml:/opt/work/conf.yaml -e PARAMS="-c /opt/work/conf.yaml" yangjiafeng.com:6664/blog/account`

**docker配置sock5代理翻墙**

创建docker服务插件目录:

`sudo mkdir -p /etc/systemd/system/docker.service.d`

创建一个名为http-proxy.conf的文件:

`sudo touch /etc/systemd/system/docker.service.d/http-proxy.conf `

编辑http-proxy.conf的文件:

`sudo vim /etc/systemd/system/docker.service.d/http-proxy.conf`

写入内容(将代理ip和代理端口修改成你自己的):
```
[Service]
Environment="HTTP_PROXY=socks5://127.0.0.1:7070/"
```

重新加载服务程序的配置文件:

`sudo systemctl daemon-reload`

重启docker:

`sudo systemctl restart docker`

验证是否配置成功:

`systemctl show --property=Environment docker`