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