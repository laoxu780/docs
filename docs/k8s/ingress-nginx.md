# ingress-nginx

## helm安装
```shell
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm fetch ingress-nginx/ingress-nginx
tar -xvf ingress-nginx-4.6.0.tgz
```

**修改values.yaml文件**


## 手动安装 

**helm**

* https://kubernetes.io/zh-cn/docs/concepts/services-networking/ingress/
* https://kubernetes.github.io/ingress-nginx/deploy/

下载
`wget -O ingress-nginx.yaml https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.9.6/deploy/static/provider/cloud/deploy.yaml`

编辑
`vim ingress-nginx.yaml`

将externalTrafficPolicy修改为Cluster
```yaml
  externalTrafficPolicy: Cluster
  # externalTrafficPolicy: Local
```

添加`externalIPs: ['192.168.16.81']`与externalTrafficPolicy同级
```yaml
externalIPs: ['192.168.16.81']
```