# Linux mint系统 Golang开发环境搭建

**安装**

`tar -C /usr/local -xzf go1.14.2.linux-amd64.tar.gz`

`vim ~/.bashrc`

`export PATH=$PATH:/usr/local/go/bin`

`source ~/.bashrc`

**配置代理**

`export GOPROXY=https://mirrors.aliyun.com/goproxy,direct`

`export GOSUMDB=sum.golang.google.cn`

**初始化项目**

`go mod init project`

**获取第三方包**

`go get github.com/go-redis/redis/v7`

