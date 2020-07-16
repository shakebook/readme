## iptables 配置 ##

**1.查看系统是否安装防火墙**

`root@localhost:/usr# which iptables`

`/sbin/iptables`

`root@localhost:/usr# whereis iptables`

`iptables: /sbin/iptables /etc/iptables /usr/share/iptables /usr/share/man/man8/iptables.8.gz`

如果是这样的信息，那么表明iptables就是安装了的。
如果没有安装，那么使用`sudo apt-get install iptables` 安装。

**2.查看防火墙的配置信息**

`iptables -L -n`

`mkdir /etc/iptables` #先新建目录，本身无此目录

`vim /etc/iptables/rules.v4`

/etc/iptables/rules.v4 中的内容是:

```
*filter
:INPUT DROP [0:0]
:FORWARD ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]
:syn-flood - [0:0]
-A INPUT -i lo -j ACCEPT
-A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 22 -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 80 -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 3306 -j ACCEPT
-A INPUT -p icmp -m limit --limit 100/sec --limit-burst 100 -j ACCEPT
-A INPUT -p icmp -m limit --limit 1/s --limit-burst 10 -j ACCEPT
-A INPUT -p tcp -m tcp --tcp-flags FIN,SYN,RST,ACK SYN -j syn-flood
-A INPUT -j REJECT --reject-with icmp-host-prohibited
-A syn-flood -p tcp -m limit --limit 3/sec --limit-burst 6 -j RETURN
-A syn-flood -j REJECT --reject-with icmp-port-unreachable
COMMIT
```

**4.使防火墙生效**

`iptables-restore < /etc/iptables/rules.v4`

**5.创建文件，添加以下内容，使防火墙开机启动**

`vim /etc/network/if-pre-up.d/iptables`

```
#!/bin/bash
iptables-restore < /etc/iptables/rules.v4

```

**6.添加执行权限**

`chmod +x /etc/network/if-pre-up.d/iptables`

**7.查看规则是否生效**

`iptables -L -n`

Ubuntu中没有直接停止关闭iptables的命令，像`service iptables stop`这类命令，是centos才有的。关闭的话，可以暂时开放所有端口作为替代方案:

`iptables -P INPUT ACCEPT`
`iptables -P OUTPUT ACCEPT`  