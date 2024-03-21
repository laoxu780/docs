# k8s 部署elk 8



## 单机部署

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: elasticsearch-config-nfs-pv
spec:
  capacity:
    storage: 2G
  volumeMode: Filesystem
  accessModes:
    - ReadWriteMany
  persistentVolumeReclaimPolicy: Recycle
  storageClassName: elasticsearch-config-nfs-scn
  claimRef:
    name: elasticsearch-config-nfs-pvc
    namespace: default
  nfs:
    server: 192.168.16.10
    path: /elk/elasticsearch/config
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: elasticsearch-config-nfs-pvc
spec:
  accessModes:
    - ReadWriteMany
  storageClassName: elasticsearch-config-nfs-scn
  resources:
    requests:
      storage: 1G
---
apiVersion: v1
kind: PersistentVolume
metadata:
  name: elasticsearch-data-nfs-pv
spec:
  capacity:
    storage: 2G
  volumeMode: Filesystem
  accessModes:
    - ReadWriteMany
  persistentVolumeReclaimPolicy: Recycle
  storageClassName: elasticsearch-data-nfs-scn
  claimRef:
    name: elasticsearch-data-nfs-pvc
    namespace: default
  nfs:
    server: 192.168.16.10
    path: /elk/elasticsearch/data
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: elasticsearch-data-nfs-pvc
spec:
  accessModes:
    - ReadWriteMany
  storageClassName: elasticsearch-data-nfs-scn
  resources:
    requests:
      storage: 1G 
---
apiVersion: v1
kind: PersistentVolume
metadata:
  name: elasticsearch-plugins-nfs-pv
spec:
  capacity:
    storage: 2G
  volumeMode: Filesystem
  accessModes:
    - ReadWriteMany
  persistentVolumeReclaimPolicy: Recycle
  storageClassName: elasticsearch-plugins-nfs-scn
  claimRef:
    name: elasticsearch-plugins-nfs-pvc
    namespace: default
  nfs:
    server: 192.168.16.10
    path: /elk/elasticsearch/plugins
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: elasticsearch-plugins-nfs-pvc
spec:
  accessModes:
    - ReadWriteMany
  storageClassName: elasticsearch-plugins-nfs-scn
  resources:
    requests:
      storage: 1G
---
apiVersion: v1
kind: PersistentVolume
metadata:
  name: elasticsearch-logs-nfs-pv
spec:
  capacity:
    storage: 2G
  volumeMode: Filesystem
  accessModes:
    - ReadWriteMany
  persistentVolumeReclaimPolicy: Recycle
  storageClassName: elasticsearch-logs-nfs-scn
  claimRef:
    name: elasticsearch-logs-nfs-pvc
    namespace: default
  nfs:
    server: 192.168.16.10
    path: /elk/elasticsearch/logs
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: elasticsearch-logs-nfs-pvc
spec:
  accessModes:
    - ReadWriteMany
  storageClassName: elasticsearch-logs-nfs-scn
  resources:
    requests:
      storage: 1G
---
apiVersion: batch/v1
kind: Job
metadata:
  name: elk-init
spec:
  template:
    spec:
      securityContext:
        runAsUser: 0
      containers:
        - name: elk-init
          image: registry.xuyh.info/elasticsearch/elasticsearch@sha256:dabfecf24c417af77a9daed847c695117ecb3d2c185bf9b0017ae49d2bb1abb6
          volumeMounts:
            - name: config
              mountPath: /opt/elasticsearch/config
            - name: data
              mountPath: /opt/elasticsearch/data
            - name: plugins
              mountPath: /opt/elasticsearch/plugins
            - name: logs
              mountPath: /opt/elasticsearch/logs
          args:
            - bash
            - -c
            - |2
                if [ xmm13789917141 == x ]; then
                  echo "Set the ELASTIC_PASSWORD environment variable in the .env file";
                  exit 1;
                elif [ xmm13789917141 == x ]; then
                  echo "Set the KIBANA_PASSWORD environment variable in the .env file";
                  exit 1;
                fi;
                if [ ! -f config/certs/ca.zip ]; then
                  echo "Creating CA";
                  bin/elasticsearch-certutil ca --silent --pem -out config/certs/ca.zip;
                  unzip config/certs/ca.zip -d config/certs;
                fi;
                if [ ! -f config/certs/certs.zip ]; then
                  echo "Creating certs";
                  echo -ne \
                  "instances:\n"\
                  "  - name: elasticsearch\n"\
                  "    dns:\n"\
                  "      - elasticsearch-svc\n"\
                  > config/certs/instances.yml;
                  bin/elasticsearch-certutil cert --silent --pem -out config/certs/certs.zip --in config/certs/instances.yml --ca-cert config/certs/ca/ca.crt --ca-key config/certs/ca/ca.key;
                  unzip config/certs/certs.zip -d config/certs;
                fi;
                echo "Setting file permissions"
                chown -R root:root config/certs;
                find . -type d -exec chmod 750 \{\} \;;
                find . -type f -exec chmod 640 \{\} \;;
                echo "Waiting for Elasticsearch availability";
                until curl -s --cacert config/certs/ca/ca.crt https://elasticsearch-svc:9200 | grep -q "missing authentication credentials"; do sleep 30; done;
                echo "Setting kibana_system password";
                until curl -s -X POST --cacert config/certs/ca/ca.crt -u "elastic:mm13789917141" -H "Content-Type: application/json" https://elasticsearch-svc:9200/_security/user/kibana_system/_password -d "{\"password\":\"mm13789917141\"}" | grep -q "^{}"; do sleep 10; done;
                echo "All done!";
                cp -r /usr/share/elasticsearch/config /opt/elasticsearch/config
                cp -r /usr/share/elasticsearch/data /opt/elasticsearch/data
                cp -r /usr/share/elasticsearch/plugins /opt/elasticsearch/plugins
                cp -r /usr/share/elasticsearch/logs /opt/elasticsearch/logs
      volumes:
        - name: config
          persistentVolumeClaim:
            claimName: elasticsearch-config-nfs-pvc
        - name: data
          persistentVolumeClaim:
            claimName: elasticsearch-data-nfs-pvc
        - name: plugins
          persistentVolumeClaim:
            claimName: elasticsearch-plugins-nfs-pvc
        - name: logs
          persistentVolumeClaim:
            claimName: elasticsearch-logs-nfs-pvc
      restartPolicy: Never
  backoffLimit: 3
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: elasticsearch
  labels:
    app: elasticsearch
spec:
  replicas: 1
  selector:
    matchLabels:
      app: elasticsearch
  template:
    metadata:
      labels:
        app: elasticsearch
    spec:
      containers:
        - name: elasticsearch
          image: registry.xuyh.info/elasticsearch/elasticsearch@sha256:dabfecf24c417af77a9daed847c695117ecb3d2c185bf9b0017ae49d2bb1abb6
          ports:
            - containerPort: 9200
      volumes:
        - name: config
          persistentVolumeClaim:
            claimName: elasticsearch-config-nfs-pvc
        - name: data
          persistentVolumeClaim:
            claimName: elasticsearch-data-nfs-pvc
        - name: plugins
          persistentVolumeClaim:
            claimName: elasticsearch-plugins-nfs-pvc
        - name: logs
          persistentVolumeClaim:
            claimName: elasticsearch-logs-nfs-pvc
---
apiVersion: v1
kind: Service
metadata:
  name: elasticsearch-svc
  labels:
    tms: elasticsearch-svc
spec:
  ports:
    - port: 9200
      protocol: TCP
  selector:
    app: elasticsearch
```

## 集群部署


## elasticsearch

```yaml


```

## kibana

## logstash

