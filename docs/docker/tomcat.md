# docker 运行tomcat

## 编辑Dockerfile文件

```dockerfile
FROM tomcat:10.1.19-jre21

# 添加war包
ADD app.war /usr/local/tomcat/webapps/ROOT.war

# 将时区设置为东八区
RUN cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo 'Asia/Shanghai' > /etc/timezone

# 修改tomcat端口
# RUN sed -i 's|"8080"|"9092"|' /usr/local/conf/server.xml

# 指定数据卷
VOLUME ["/srv/tms/data"]

ENTRYPOINT ["catalina.sh", "run"]

```

## 构建镜像

```shell
docker build -t app:1.0.0 .
```

## 运行容器

```shell
docker run -d \
  -e "SPRING_PROFILES_ACTIVE=test" \
  -e "JAVA_OPTS=-Dfile.encoding=UTF-8 --add-exports java.desktop/sun.font=ALL-UNNAMED --add-opens java.desktop/sun.font=ALL-UNNAMED" \
  -v /srv/tms/data:/srv/tms/data \
  --name tms-label-designer \
  -p 9201:8080 \
  app:1.0.0
```
