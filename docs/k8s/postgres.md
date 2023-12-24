# k8s安装postgres


## 部署postgres
**postgres.yaml**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: postgres
  labels:
    app: postgres
spec:
  type: NodePort
  ports:
    - port: 5432
      nodePort: 30432
      name: pport
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
      volumes:
        - name: postgres-data
          persistentVolumeClaim:
            claimName: postgres-pvc  
        
        
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

## 部署pgadmin4

**pgadmin4-pod.yaml**
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: pgadmin4
  namespace: default
spec:
  ingressClassName: nginx
  rules:
    - host: localhost
      http:
        paths:
          - pathType: Prefix
            backend:
              service:
                name: pgadmin4
                port:
                  number: 80
            path: /
---
apiVersion: v1
kind: Service
metadata:
  name: pgadmin4
  labels:
    app: pgadmin4
spec:
  ports:
    - port: 80
      name: web
      targetPort: 80
  selector:
    app: pgadmin4
---
apiVersion: v1
kind: Pod
metadata:
  name: pgadmin4
  namespace: default
  labels:
    app: pgadmin4
spec:
  containers:
    - name: pgadmin4
      image: dpage/pgadmin4
      ports:
        - containerPort: 80
          protocol: TCP
      env:
        - name: PGADMIN_DEFAULT_EMAIL
          value: 328340506@qq.com
        - name: PGADMIN_DEFAULT_PASSWORD
          value: qwe123!@#
```
