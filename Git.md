# linux mint 安装 Git

**一、linux mint安装 git**

`apt-get install git`

`git version`

`git config --global user.name "yangjiafeng"`

`git config --global user.email "852264915@qq.com"`

***

**二、安装 SSH：**


`apt-get install ssh`

`ps -e | grep sshd`


sshd 表示SSH服务已启动

生成 SSH KEY

`ssh-keygen -t rsa -C "852264915@qq.com"`

进入 `/root/.ssh` 目录，查看 `id_rsa` 和 `id_rsa.pub` 文件：

把公钥`id_rsa.pub`放入github仓库SSH and GPG keys

***

**三、git push无法提交，报错如下：**

`ssh_dispatch_run_fatal: Connection to 52.74.223.119 port 22: Connection timed out`

解决如下：

在.ssh目录下新建config文件，键入一下内容:


`Host github.com`

`User git`

`Hostname ssh.github.com`

`PreferredAuthentications publickey`

`IdentityFile ~/.ssh/id_rsa`

`Port 443`


查看是否可以正常连接：`ssh -T git@github.com`