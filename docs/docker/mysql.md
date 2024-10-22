# docker 安装mysql

```shell
docker run \
  --name mysql \
  -e MYSQL_ROOT_PASSWORD=123456 \
  -e MYSQL_DATABASE= \
  -d mysql:8.4.1
```