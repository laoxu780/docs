# 命令

## 打标签
```shell
kubectl label node k8s-node1 postgres=true
```

## 进入临时容器
```shell
# busybox
kubectl run -i --tty --image busybox:1.28 dns-test --restart=Never --rm

# postgres
kubectl run psql-test --image registry.xuyh.info/kubernetes/postgres:14.10 --rm --tty -i --restart='Never' --env="PGPASSWORD=123456" \
--command -- psql --host postgres-0.postgres -U postgres -p 5432
```