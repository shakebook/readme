# docker

**install docker**

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