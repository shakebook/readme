## Jenkins笔记##

1. docker 运行jenkins

```
docker run \
  --name jenkins \
  -u root \
  --rm \
  -d \
  -p 5001:8080 \
  -p 50000:50000 \
  -v /usr/data/jenkins:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -e JENKINS_JAVA_OPTIONS="-Djava.awt.headless=true -Dhudson.security.csrf.GlobalCrumbIssuerConfiguration.DISABLE_CSRF_PROTECTION=true" \
  jenkinsci/blueocean

```

2. 浏览到 http://localhost:5001


3. 解锁，进入容器`docker exec -it containerid /bin/bash`,按页面的提示，找到密码