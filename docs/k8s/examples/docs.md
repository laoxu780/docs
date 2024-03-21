# docs部署

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: docs
  labels:
    app: docs
spec:
  replicas: 1
  selector:
    matchLabels:
      app: docs
  template:
    metadata:
      labels:
        app: docs
    spec:
      containers:
      - name: docs
        image: registry.xuyh.info/k8s/docs:1.0.0
        ports:
        - containerPort: 80
        command: [ "nginx" ]
        args:
          - '-g'
          - 'daemon off;'
      imagePullSecrets:
        - name: registry-auth
---
apiVersion: v1
kind: Service
metadata:
  name: docs
  labels:
    tms: docs
spec:
  ports:
    - port: 80
      protocol: TCP
  selector:
    app: docs
---

apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: docs
  namespace: default
spec:
  ingressClassName: nginx
  rules:
    - host: docs.xuyh.info
      http:
        paths:
          - backend:
              service:
                name: docs
                port:
                  number: 80
            path: /
            pathType: Prefix
```