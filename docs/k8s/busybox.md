# busybox


busybox-deployment.yaml
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: busybox
spec:
  selector:
    matchLabels:
      app: busybox
  replicas: 1
  template:
    metadata:
      labels:
        app: busybox
    spec:
      containers:
      - name: busybox
        image: busybox:1.32
        imagePullPolicy: IfNotPresent
        args:
        - /bin/sh
        - -c
        - sleep 3600
```

**应用配置文件**

`kubectl apply -f busybox-deployment.yaml`

**进入容器**

`kubectl exec -it busybox-pod -- sh`

**运行零时busybox容器**
```shell
kubectl run -i --tty --image busybox:1.28 dns-test --restart=Never --rm
```