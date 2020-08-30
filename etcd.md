# linux install etcd

**1、download**

`wget https://github.com/etcd-io/etcd/releases/download/v3.4.10/etcd-v3.4.10-linux-amd64.tar.gz`

**2、run**

`tar -xzvf etcd-v3.4.10-linux-amd64.tar.gz`

`sudo mkdir -p /usr/local/etcd`

`mv etcd-v3.4.10-linux-amd64/* /usr/local/etcd`

`cd /usr/local/etcd `

`./etcd --listen-client-urls http://0.0.0.0:2379 --advertise-client-urls http://0.0.0.0:2379 --listen-peer-urls http://0.0.0.0:2380 >/dev/null 2>/dev/null & `


