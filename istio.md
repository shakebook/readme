# istio
本教程采用kubernetes1.14和istio1.1.7
安装helm:
```
wget https://storage.googleapis.com/kubernetes-helm/helm-v2.12.1-linux-amd64.tar.gz
tar -zxvf helm-v2.12.1-linux-amd64.tar.gz
cd linux-amd64/
cp helm tiller /usr/local/bin
helm version
tiller -version
helm init
```
#在kubernetes中启动tiller服务
#会拉取镜像失败
查看kubernetes中pod: tiller-deployxxx的镜像
`kubectl describe pod tiller-deploy-548df79d66-j7h6x -n kube-system`
从阿里云下载镜像并修改tag:
```
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/tiller:v2.12.1
docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/tiller:v2.12.1 gcr.io/kubernetes-helm/tiller:v2.12.1 
docker rmi registry.cn-hangzhou.aliyuncs.com/google_containers/tiller:v2.12.1 

```

官方文档：https://istio.io/v1.1/zh/docs/setup/kubernetes/install/kubernetes/
istio下载地址：https://github.com/istio/istio/tags
下载后解压，进入目录istio-1.1.7/install/kubernetes/helm/istio-init：
修改values.yaml: `hub: registry.cn-hangzhou.aliyuncs.com/imooc-istio`
生成模板文件：`helm template . --name imooc-istio-init --namespace istio-system > istio-init.yaml`
创建crd: `kubectl apply -f istio-init.yaml`
查看： 
```
kubectl get pods -n istio-system
kubectl get crd | grep istio
kubectl get crd | grep istio | wc -l
```
kubectl get crd | grep istio | wc -l输出53，crd创建成功
