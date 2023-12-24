# 常用命令

```shell
# 拉取镜像
docker pull nginx:latest


# 登录
docker login -u admin -p Harbor12345 registry.xuyh.info

# 登出
docker logout registry.xuyh.info

# 容器设置为自启动
docker update --restart=always nginx

# 容器取消自启动
docker update --restart=no nginx
```