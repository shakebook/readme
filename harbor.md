# harbor#

请提前安装docker-compose, 详见docker-compose教程

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

9. docker 登录私有仓库hub.images.com

docker login hub.images.com

userName :admin , password: 123456

10. 推送镜像到私有仓库

```
docker tag dev-orm hub.images.com/yang-files/orm

docker push hub.images.com/yang-files/orm:dev

```

解释：

dev-orm:制作好的本地镜像tag（通过`docker build -t dev-orm .`）

yang-files: harbor中的项目名称

orm : 项目中的镜像tag

11. 运行在harbor私有仓库的镜像

`bocker run --name harbor-orm -p 9004:9004 -d hub.images.com/yang-files/orm`




