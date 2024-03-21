# ingress

## 参考连接

* https://kubernetes.io/zh-cn/docs/concepts/services-networking/ingress/
* https://kubernetes.github.io/ingress-nginx/deploy/

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: pgadmin
  namespace: default
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /$2
spec:
  ingressClassName: nginx
  rules:
    - host: pgadmin4.xuyh.info
      http:
        paths:
          - pathType: Prefix
            backend:
              service:
                name: pgadmin
                port:
                  number: 80
            path: /
```

## ingress-nginx 路径重写

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: tms1024-admin
  namespace: default
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /$2
spec:
  ingressClassName: nginx
  rules:
    - host: *
      http:
        paths:
          - pathType: ImplementationSpecific
            backend:
              service:
                name: tms1024-admin
                port:
                  number: 80
            path: /api(/|$)(.*)


```