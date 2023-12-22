# StatefulSet k8s部署有状态服务

## yaml文件结构

```yaml
apiVersion: v1
kind: Service
metadata:
  name: postgres
  labels:
    app: postgres
spec:
  ports:
    - port: 5432
      name: pport
  clusterIP: None
  selector:
    app: postgres
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: postgres
spec:
  serviceName: postgres
  replicas: 1
  selector:
    matchLabels:
      app: postgres
  template:
    metadata:
      labels:
        app: postgres
    spec:
      terminationGracePeriodSeconds: 30
      securityContext:
        runAsUser: 1000
        fsGroup: 1000
      containers:
        - name: postgres
          image: postgres:15.4
          ports:
            - containerPort: 5432
              protocol: TCP
          volumeMounts:
            - name: postgres-data
              mountPath: /var/lib/postgresql/data
          env:
            - name: POSTGRES_DB
              value: tms-tool
            - name: POSTGRES_PASSWORD
              value: "123456"
            - name: PGDATA
              value: /var/lib/postgresql/data/pgdata
      nodeSelector:
        postgres: "true"
  volumeClaimTemplates:
    - metadata:
        name: postgres-data
      spec:
        accessModes: [ "ReadWriteOncePod" ]
        storageClassName: nfs-client
        resources:
          requests:
            storage: 10Gi
```