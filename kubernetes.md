# kubernetes

**docker启动三台机子**

```
docker run -itd --name box1 --hostname box1 --privileged=true --net cluster --ip 182.182.0.11 centos /sbin/init
docker run -itd --name box2 --hostname box2 --privileged=true --net cluster --ip 182.182.0.12 centos /sbin/init
docker run -itd --name box3 --hostname box3 --privileged=true --net cluster --ip 182.182.0.13 centos /sbin/init

yum install passwd

passwd root

```
**安装ssh**

https://phoenixnap.com/kb/how-to-enable-ssh-centos-7

```

yum -y install openssh-server openssh-clients

systemctl start sshd

systemctl status sshd

systemctl stop sshd

systemctl enable sshd

systemctl disable sshd

vim /etc/ssh/sshd_config

service sshd restart

```

**设置主机名**

如果 DNS 不支持主机名称解析，还需要在每台机器的 /etc/hosts 文件中添加主机名和 IP 的对应关系：

```
cat >> /etc/hosts <<EOF
182.182.0.11 box1
182.182.0.12 box2
182.182.0.13 box3
EOF

```

退出，重新登录 root 账号，可以看到主机名生效。

**添加节点信任关系**

本操作只需要在 box1 节点上进行，设置 root 账户可以无密码登录所有节点：

```

ssh-keygen -t rsa 
ssh-copy-id root@box1
ssh-copy-id root@box2
ssh-copy-id root@box3

```

**安装依赖包**

```

yum install -y epel-release
yum install -y chrony conntrack ipvsadm ipset jq iptables curl sysstat libseccomp wget socat git

```

本文档的 kube-proxy 使用 ipvs 模式，ipvsadm 为 ipvs 的管理工具；
etcd 集群各机器需要时间同步，chrony 用于系统时间同步；

**关闭防火墙**

关闭防火墙，清理防火墙规则，设置默认转发策略：

```
systemctl stop firewalld
systemctl disable firewalld
iptables -F && iptables -X && iptables -F -t nat && iptables -X -t nat
iptables -P FORWARD ACCEPT

```