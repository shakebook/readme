# kubectl命令

**在 kube-system 命名空间中列出集群中的 Deployments **
`kubectl get deployment --namespace=kube-system`

**获取 DNS Deployment 的名称**
`kubectl get deployment -l k8s-app=kube-dns --namespace=kube-system`


**缩放副本数**
`kubectl scale deployment [deployment_name] --replicas=3`