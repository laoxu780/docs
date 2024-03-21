# docker-compose


## 安装
```shell
curl -L https://github.com/docker/compose/releases/download/v2.24.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
```

**启动服务**

```shell
docker-compose up -d
```

**-f指定配置文件**

```shell
docker-compose up -f docer-test.yml
```


**停止或删除服务**

```shell
# stop直接停止services
docker-compose stop

# down则会停止并删除创建的service，volume和network。
docker-compose down
```

**进入服务**

```shell
docker-compose exec mysql bash
```

**查看服务日志**

```shell
docker-compose logs
```


## up

* -d 在后台运行服务容器。
* --no-color 不使用颜色来区分不同的服务的控制台输出。
* --no-deps 不启动服务所链接的容器。
* --force-recreate 强制重新创建容器，不能与 --no-recreate 同时使用。
* --no-recreate 如果容器已经存在了，则不重新创建，不能与 --force-recreate 同时使用。
* --no-build 不自动构建缺失的服务镜像。
* -t, --timeout TIMEOUT 停止容器时候的超时（默认为 10 秒）。

## down


**停止并删除工程中所有服务的容器、网络**

```shell
docker-compose stop
```

**停止并删除工程中所有服务的容器、网络、镜像**
```shell
docker-compose down --rmi all
```

**停止并删除工程中所有服务的容器、网络、数据卷**
```shell
docker-compose down -v
```
