# 临时端口转发 

```shell
## 转发postgres 5432端口
kubectl port-forward pod/postgres-0 --address 0.0.0.0 5432


```