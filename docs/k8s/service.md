# Service

## 实例

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-nginx
  labels:
    run: my-nginx
spec:
  ports:
  - port: 80
    protocol: TCP
  selector:
    run: my-nginx
```


## 无头服务
```yaml
apiVersion: v1
kind: Service
metadata:
  name: kubia-headless
spec:
  clusterIP: None              #这使得服务成为headless
  ports:
  - port: 80
    targetPort: 8080
  selector:
    app: kubia
```