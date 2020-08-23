# Gitlab笔记

**安装**

```
sudo docker run --detach \
  --hostname 47.108.136.149 \
 --publish 80:80 --publish 52:22 \
  --name gitlab \
  --restart always \
  --volume /usr/data/gitlab/config:/etc/gitlab \
  --volume /usr/data/gitlab/logs:/var/log/gitlab \
  --volume /usr/data/gitlab/data:/var/opt/gitlab \
  gitlab/gitlab-ce:latest

```
git clone http://yangjiafeng.com:5002/yang/blog.git
git clone ssh://git@47.108.136.149:52/yang/blog.git
git clone ssh://git@47.108.136.149:yang/blog.git