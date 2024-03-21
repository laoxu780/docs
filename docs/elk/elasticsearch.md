# elasticsearch

> 文档链接: https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html

## 安装

### docker

```shell
# 创建docker网络
docker network create elastic

# 拉取镜像
docker pull docker.elastic.co/elasticsearch/elasticsearch:8.11.4


# 创建临时容器 将配置文件复制到主机
docker run \
 --name elasticsearch \
 --net elastic \
 -p 9200:9200 \
 -m 1GB \
 -it \
 docker.elastic.co/elasticsearch/elasticsearch:8.11.4

docker cp elasticsearch:/usr/share/elasticsearch/config /srv/elk/elasticsearch/
docker cp elasticsearch:/usr/share/elasticsearch/data /srv/elk/elasticsearch/
docker cp elasticsearch:/usr/share/elasticsearch/plugins /srv/elk/elasticsearch/
docker cp elasticsearch:/usr/share/elasticsearch/logs /srv/elk/elasticsearch/

openssl x509 -in /srv/elk/elasticsearch/config/certs/http_ca.crt -sha256 -fingerprint | grep SHA256 | sed 's/://g'

# 删除临时容器
docker stop elasticsearch
docker rm elasticsearch

# 创建elasticsearch
docker run -d \
 --name elasticsearch \
 --net elastic \
 -p 9200:9200 \
 -m 3GB \
 -e ES_JAVA_OPTS="-Xms2g -Xmx2g" \
 -v /srv/elk/elasticsearch/config:/usr/share/elasticsearch/config \
 -v /srv/elk/elasticsearch/data:/usr/share/elasticsearch/data \
 -v /srv/elk/elasticsearch/plugins:/usr/share/elasticsearch/plugins \
 -v /srv/elk/elasticsearch/logs:/usr/share/elasticsearch/logs \
 docker.elastic.co/elasticsearch/elasticsearch:8.11.4
 
# 修改并获取密码
docker exec -it elasticsearch /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic

# 获取kibana token 30分钟有效期
docker exec -it elasticsearch /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana

# 获取ca_trusted_fingerprint 
openssl x509 -in /srv/elk/elasticsearch/config/certs/http_ca.crt -sha256 -fingerprint | grep sha256 | sed 's/://g'

# 获取小写 ca_trusted_fingerprint 
openssl x509 -in /srv/elk/elasticsearch/config/certs/http_ca.crt -sha256 -fingerprint | grep sha256 | sed 's/://g' | tr [:upper:] [:lower:]

# 获取大写 ca_trusted_fingerprint 
openssl x509 -in /srv/elk/elasticsearch/config/certs/http_ca.crt -sha256 -fingerprint | grep sha256 | sed 's/://g' | tr [:lower:] [:upper:]
```

配置文件增加配置 
`xpack.monitoring.collection.enabled: true`
```shell
vim /srv/elk/elasticsearch/config/elasticsearch.yml
```
添加这个配置以后在kibana中才会显示联机状态，否则会显示脱机状态

重启elasticsearch
```shell
docker restart elasticsearch
```
