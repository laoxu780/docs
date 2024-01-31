# 常用命令


## docker run
* -a stdin：指定标准输出内容类型 STDIN\STDOUT\STDERR
* -d：后台运行容器，返回容器ID
* -i：以交互模式运行容器，通常与-t同时使用
* -P：随机端口映射，容器内部端口随机映射到主机端口
* -p：指定端口映射 格式为： 主机port:容器port
* -t：为容器重新分配一个为输入终端，通常与 -i同时使用
* –name：为容器指定一个名称
* –dns 8.8.8.8：指定容器使用的DNS服务器，默认和宿主一样
* -h “mars”：指定容器的hostname
* -e username “”：设置环境变量
* –env-file：从指定文件读入环境变量
* –cpuset=“0-2”：绑定容器到指定CPU运行
* -m：设置容器使用内存最大值
* –net=“bridge”：指定容器的网络连接类型 支持bridge\host\none\container
* –link=[]：添加连接到另一个容器
* –expose[]：开放一个端口或一组端口
* –volume,-v：绑定一个卷

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


## 开启remote api

```shell
# vim /usr/lib/systemd/system/docker.service
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd -H tcp://0.0.0.0:2375 -H unix://var/run/docker.sock
```

**重启**

```shell
systemctl daemon-reload
systemctl restart docker
```

## 常用

**删除none镜像**

```shell
docker rmi $(docker images -f "dangling=true" -q)
```