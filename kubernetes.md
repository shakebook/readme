# kubernetes

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

**kubernetes 拉取habor私有镜像**

创建名为k8s-harbor-image-pull-secrets的secret:

```
kubectl create secret docker-registry k8s-harbor-image-pull-secrets \
--docker-server=https://yangjiafeng.com:6664 \
--docker-username=admin \
--docker-password='123456'

kubectl patch serviceaccount default -p '{"imagePullSecrets": [{"name": "k8s-harbor-image-pull-secrets"}]}'

```

在kubernetes部署中使用secret：

```

apiVersion: apps/v1
kind: Deployment
metadata:
  name: api-deployment
  labels:
    app: api
spec:
  selector:
    matchLabels:
      app: api
  replicas: 2
  template:
    metadata:
      labels:
        app: api
    spec:
      imagePullSecrets:
        - name: k8s-harbor-image-pull-secrets
      containers:
        - name: api
          image: yangjiafeng.com:6664/test/api-service
          ports:
            - name: api-service
              containerPort: 8080


```

 清理k8s集群
 yum remove -y kubelet kubeadm kubectl

kubeadm reset -f
modprobe -r ipip
lsmod
rm -rf ~/.kube/
rm -rf /etc/kubernetes/
rm -rf /etc/systemd/system/kubelet.service.d
rm -rf /etc/systemd/system/kubelet.service
rm -rf /usr/bin/kube*
rm -rf /etc/cni
rm -rf /opt/cni
rm -rf /var/lib/etcd
rm -rf /var/etcd

kubeadm reset