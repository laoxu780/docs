# elasticsearch

## 安装

### docker
```shell
docker pull elasticsearch:7.17.16

docker network create es-net

docker run -d \
-e "ES_JAVA_POTS=-Xmx512m -Xmx2048m" \
-e "discovery.type=single-node" \
-v es-data:/usr/share/elasticsearch/data \
-v es-plugins:/usr/share/elasticsearch/plugins \
--privileged \
--network es-net \
-p 9200:9200 \
-p 9300:9300 \
elasticsearch:7.12.1





docker run --rm docker.elastic.co/elasticsearch/elasticsearch:7.17.16
```