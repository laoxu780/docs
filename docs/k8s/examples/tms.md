# 部署tms

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: filebeat-config
data:
  filebeat.yml: |
    filebeat.inputs:
      - type: log
        enabled: true
        paths:
          - /app/logs/*.log

        multiline.type: pattern
        multiline.pattern: '^\d{4}'
        multiline.negate: true
        multiline.match: after
    processors:
      - drop_fields:
          fields: ["log","host","input","agent","ecs"]
          ignore_missing: true
    output.logstash:
      hosts: ["192.168.16.7:5041"]
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: tms
  labels:
    app: tms
spec:
  replicas: 3
  selector:
    matchLabels:
      app: tms
  template:
    metadata:
      labels:
        app: tms
    spec:
      terminationGracePeriodSeconds: 30
      securityContext:
        runAsUser: 0
        fsGroup: 0
      containers:
      - name: tms
        image: registry.xuyh.info/k8s/tms:1.0.0
        ports:
        - containerPort: 8080
        volumeMounts:
          - name: app-logs
            mountPath: /app/logs
      - name: filebeat
        image: docker.elastic.co/beats/filebeat:8.12.1
        volumeMounts: 
        - name: app-logs
          mountPath: /app/logs
        - name: filebeat-config
          mountPath: /usr/share/filebeat/filebeat.yml
          subPath: filebeat.yml
      imagePullSecrets:
        - name: registry-auth
      volumes:
        - name: app-logs
          emptyDir: {}
        - name: filebeat-config
          configMap:
            name: filebeat-config
---
apiVersion: v1
kind: Service
metadata:
  name: tms
  labels:
    tms: tms
spec:
  ports:
  - port: 8080
    protocol: TCP
  selector:
    app: tms
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: tms
  namespace: default
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /$2
spec:
  ingressClassName: nginx
  rules:
    - host: k8s-master
      http:
        paths:
          - pathType: ImplementationSpecific
            backend:
              service:
                name: tms
                port:
                  number: 8080
            path: /api(/|$)(.*)
```