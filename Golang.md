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

**vscode golang开发断点调试launch.json**
```
    "justMyCode":true,
    "configurations": [
        {
            "name": "Launch",
            "type": "go",
            "request": "launch",
            "mode": "debug",
            "remotePath": "",
            "port": 2345,
            "host": "127.0.0.1",
            "program": "/home/yang/work/blog/service/account",
            "env": {
                "GOPATH":"/home/yang/go"
            },
            "args": ["-p","/home/yang/work/blog/deploy/prod/conf.yaml"],
            "showLog": true
        }
    ]
```
**后台运行go程序**

`nohup ./main >/dev/null 2>/dev/null &`

```

#!/bin/bash

pids="`ps -ef |grep blog |grep -v -e grep -e killkeys |awk '{print $2}'`"
if [ -z "$pids" ];then
  #echo empty
  exit 0
fi
echo "kill pid:"$pids
kill -9 $pids &&


cd /opt/blog && git pull origin master && cd /opt/blog/deploy/exec
nohup ./blog-gateway >/dev/null 2>/dev/null &
nohup ./blog-account >/dev/null 2>/dev/null &
nohup ./blog-file >/dev/null 2>/dev/null &
nohup ./blog-orm >/dev/null 2>/dev/null &
nohup ./blog-auther >/dev/null 2>/dev/null &



```
