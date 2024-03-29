# k8s 部署elk

```yaml
 #elasticsearch.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: elasticsearch
  namespace: ops
  labels:
    k8s-app: elasticsearch
spec:
  replicas: 1
  selector:
    matchLabels:
      k8s-app: elasticsearch
  template:
    metadata:
      labels:
        k8s-app: elasticsearch
    spec:
      containers:
      - image: elasticsearch:7.9.2
        name: elasticsearch
        resources:
          limits:
            cpu: 2
            memory: 3Gi
          requests:
            cpu: 0.5
            memory: 500Mi
        env:
          - name: "discovery.type"
            value: "single-node"
          - name: ES_JAVA_OPTS
            value: "-Xms512m -Xmx2g"
        ports:
        - containerPort: 9200
          name: db
          protocol: TCP
        volumeMounts:
        - name: elasticsearch-data
          mountPath: /usr/share/elasticsearch/data
      volumes:
      - name: elasticsearch-data
        persistentVolumeClaim:
          claimName: es-pvc

---

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: es-pvc
  namespace: ops
spec:
  storageClassName: "managed-nfs-storage"
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 10Gi

---

apiVersion: v1
kind: Service
metadata:
  name: elasticsearch
  namespace: ops
spec:
  ports:
  - port: 9200
    protocol: TCP
    targetPort: 9200
  selector:
    k8s-app: elasticsearch

```