# pv 持久化卷

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: postgres-pv
  labels:
    type: local
spec:
  storageClassName: postgres-scn
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOncePod
  nfs:
    server: 192.168.16.10
    path: /elk/elasticsearch/certs
```

## pvc

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: postgres-pvc
spec:
  storageClassName: postgres-scn
  accessModes:
    - ReadWriteOncePod
  resources:
    requests:
      storage: 10Gi
```