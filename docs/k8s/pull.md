# 拉取镜像支持http

```
      [plugins."io.containerd.grpc.v1.cri".registry.mirrors]
      
      [plugins."io.containerd.grpc.v1.cri".registry.mirrors."192.168.88.174:5000"]
        endpoint = ["http://192.168.88.174:5000"]
```

## 仓库认证

```shell
kubectl create secret docker-registry registry-auth \
--docker-username=admin \
--docker-password=Harbor12345 \
--docker-email=328340506@qq.com \
--docker-server=registry.xuyh.info


containers 同级增加:

      imagePullSecrets:
        - name: registry-auth

```