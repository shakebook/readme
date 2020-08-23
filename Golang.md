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

**vscode golang开发断点调试**
```
    "configurations": [
        
        {
            "name": "Launch",
            "type": "go",
            "request": "launch",
            "mode": "debug",
            "remotePath": "",
            "port": 2345,
            "host": "127.0.0.1",
            "program": "${workspaceRoot}",
            "env": {
                "GOPATH":"/home/yang/workspace"
            },
            "args": [],
            "showLog": true
        }
    ]
```
**后台运行go程序**

`nohup ./main >/dev/null 2>/dev/null &`
