## Jenkins笔记##

1. docker 运行jenkins

```
docker run \
  -u root \
  --rm \
  -d \
  -p 5001:8080 \
  -p 50001:50000 \
  -v /usr/data/jenkins-data:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  jenkinsci/blueocean

```

2. 浏览到 http://localhost:5001


3. 解锁，进入容器`docker exec -it containerid /bin/bash`,按页面的提示，找到密码