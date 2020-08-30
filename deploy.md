# deploy

```

#!/bin/bash

pids="`ps -ef |grep blog |grep -v -e grep -e killkeys |awk '{print $2}'`"

echo "kill pid:"$pids
kill -9 $pids &&

cd /opt/blog/deploy/exec
git pull origin master &&

nohup ./blog-gateway >/dev/null 2>/dev/null &
nohup ./blog-account >/dev/null 2>/dev/null &
nohup ./blog-file >/dev/null 2>/dev/null &
nohup ./blog-orm >/dev/null 2>/dev/null &
nohup ./blog-auther >/dev/null 2>/dev/null &

```