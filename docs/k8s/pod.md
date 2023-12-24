# pod
> Pod 是k8s 系统中可以创建和管理的最小单元，是资源对象模型中由用户创建或部署的最小资源对象模型，也是在k8s 上运行容器化应用的资源对象，其他的资源对象都是用来支撑或者扩展Pod 对象功能的，比如控制器对象是用来管控Pod 对象的，Service 或者Ingress 资源对象是用来暴露Pod 引用对象的，PersistentVolume 资源对象是用来为Pod提供存储等等，k8s 不会直接处理容器，而是Pod，Pod 是由一个或多个container 组成。

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  namespace: default
  labels:
    name: app
spec:
  containers:
    - name: nginx
      image: nginx:latest
      ports:
        - containerPort: 80
          protocol: TCP
      resources:
        requests:
          memory: "100Mi"
          cpu: 100m
        limits:
          memory: "200Mi"
          cpu: 1000m
      command: [ "nginx" ]
      args:
        - '-g
        - 'daemon off;'
      volumeMounts:
        - name: data
          mountPath: /data
        - name: nfs-logs
          mountPath: /logs
  volumes:
    - name: data
      emptyDir: {}
    - name: nfs-logs
      persistentVolumeClaim:
        claimName: nfs-claim
  nodeSelector:
    disk-type: ssd
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  namespace: default
  name: nfs-claim
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: nfs-storage
  resources:
    requests:
      storage: 5Gi
```

| key                                            | value                                          |
|------------------------------------------------|------------------------------------------------|
| apiVersion                                     | 固定为v1                                          |
| kind                                           | 资源类型 固定Pod                                     |
| metadata                                       | 元数据                                            |
| metadata.name                                  | pod的名称                                         |
| metadata.namespace                             | pod的命名空间                                       |
| metadata.labels                                | pod的标签内容的key value可以自定义                        |
| spec                                           | 定义了 Pod 的行为和构建                                 |
| spec.containers[]                              | pod里的容器 数组                                     |
| spec.containers[].name                         | 容器名称                                           |
| spec.containers[].image                        | 镜像                                             |
| spec.containers[].ports[]                      | 容器对外暴露的端口号声明， 不是必须的，只是一种规范                     |
| spec.containers[].ports[].containerPort        | 端口号                                            |
| spec.containers[].ports[].protocol             | 协议                                             |
| spec.containers[].resources                    | 容器使用的资源限制                                      |
| spec.containers[].resources.requests           | 最小限制                                           |
| spec.containers[].resources.requests.memory    | 最小内存限制 单位E、P、T、G、M、K、Ei、Pi、Ti、Gi、Mi、Ki         |
| spec.containers[].resources.requests.cpu       | 最小cpu限制 例如 0.5表示最小使用半个cpu 也可以使用单位m(毫) 0.5=500m |
| spec.containers[].resources.limits             | 最大资源限制                                         |
| spec.containers[].resources.requests.memory    | 最大使用的内存                                        |
| spec.containers[].resources.requests.cpu       | 最大使用的cpu                                       |
| spec.containers[].command[]                    | 容器运行的命令                                        |
| spec.containers[].args[]                       | 容器运行的命令参数                                      |
| spec.containers[].volumeMounts[]               | 容器挂载的卷                                         |
| spec.containers[].volumeMounts[].name          | 卷名与spec.volumes[].name匹配                       |
| spec.containers[].volumeMounts[].mountPath     | 挂载路径                                           |
| spec.volumes[]                                 | pod卷声明                                         |
| spec.volumes[].name                            | 卷名称                                            |
| spec.volumes[].emptyDir                        | 卷类型为emptyDir 该卷会在pod多个容器中共享 随着pod的删除而删除        |
| spec.volumes[].persistentVolumeClaim           | 绑定pvc                                          |
| spec.volumes[].persistentVolumeClaim.claimName | 绑定的pve的名称                                      |


| spec.nodeSelector                           | 节点选择器 按标签匹配                                    |