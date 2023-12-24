# 部署redis

## 制作pv

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: redis-pv
  labels:
    type: local
spec:
  storageClassName: redis-scn
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOncePod
  hostPath:
    path: "/data/redis/data"
```


## pvc

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: redis-pvc
spec:
  storageClassName: redis-scn
  accessModes:
    - ReadWriteOncePod
  resources:
    requests:
      storage: 10Gi
```

## StatefulSet

```yaml

apiVersion: v1
kind: Service
metadata:
  name: redis
  labels:
    app: redis
spec:
  ports:
    - port: 6379
      name: pport
  selector:
    app: redis
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: redis
spec:
  serviceName: redis
  replicas: 1
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
        - name: redis
          image: redis:7.2.3
          ports:
            - containerPort: 6379
              protocol: TCP
          volumeMounts:
            - name: redis-data
              mountPath: /rdata
      nodeSelector:
        redis: "true"
      volumes:
        - name: redis-data
          persistentVolumeClaim:
            claimName: redis-pvc
```