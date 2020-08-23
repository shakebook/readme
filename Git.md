# Git

**一、linux ubuntu安装 git**

`apt-get install git`

`git version`

`git config --global user.name "yangjiafeng"`

`git config --global user.email "852264915@qq.com"`

**centos源码安装方式**

源码地址：https://mirrors.edge.kernel.org/pub/software/scm/git/

```

wget -O /tmp/git-2.8.0.tar.gz https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.8.0.tar.gz 

cd /tmp && tar -zxf git-2.8.0.tar.gz -C /tmp/

yum install -y curl-devel expat-devel gettext-devel openssl-devel zlib-devel gcc perl-ExtUtils-MakeMaker


./configure --prefix=/usr/local/git


make && make install

yum remove git

vim /etc/profile

GIT_HOME
GIT_HOME=/usr/local/git
export PATH=$PATH:$GIT_HOME/bin

source /etc/profile

git --version

```

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

**四、git连接多个厂库**
远程仓库名字geet
`git remote add geet git@gitee.com:yang-jf/yang_files.git`

查看关联的远程仓库：`git remote -v`

提交到远程仓库：`git push -f geet master`　参数f首次建议加上

git push时提示：更新被拒绝，因为您当前分支的最新提交落后于其对应的远程分支.解决方法：

```
git fetch origin
git merge origin/master
git pull geet master --allow-unrelated-histories

```

