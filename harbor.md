# harbor#

1. 添加DNS,供内局域网使用：

```
vim /etc/hosts

192.168.1.2 hub.images.com

```

2. 创建harbor目录：

```
mkdir /usr/local/harbor && cd /usr/local/harbor
```

3. 下载harbor:

```
wget https://github.com/goharbor/harbor/releases/download/v1.10.4/harbor-online-installer-v1.10.4.tgz

tar -xzvf harbor-online-installer-v1.10.4.tgz

mv harbor-online-installer-v1.10.4 harbor 

cd harbor
```

4. openssl生成自签名证书：

```
openssl genrsa -out https-server.key 2048

openssl ecparam -genkey -name secp384r1 -out https-server.key

openssl req -new -x509 -sha256 -key https-server.key -out https-server.crt -days 3650

```

5. 编辑habor.yml:

```
vim harbor.yml

hostname: hub.images.com

certificate: /usr/local/harbor/https-server.crt

private_key: /usr/local/harbor/https-server.key

harbor_admin_password: 123456

```

6. 安装：`./install.sh`

7. 访问：`https://hub.images.com`

8. 登录：

用户名：admin

密码：123456


