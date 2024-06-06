# docker 安装redis


```shell
docker run \
  --name redis \
  -p 6379:6379\
  -d \
  --restart=always \
  redis:7.2.3
```
