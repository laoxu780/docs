# kibana安装

```shell
# 拉取镜像
docker pull docker.elastic.co/kibana/kibana:8.11.4

## 运行临时容器 将配置文件复制到主机
docker run -it \
	-d \
	--restart=always \
	--log-driver json-file \
	--log-opt max-size=100m \
	--log-opt max-file=2 \
	--name kibana \
	-p 5601:5601 \
	--net elastic \
	docker.elastic.co/kibana/kibana:8.11.4
	
	
docker cp kibana:/usr/share/kibana/config /srv/elk/kibana/
docker cp kibana:/usr/share/kibana/data /srv/elk/kibana/
docker cp kibana:/usr/share/kibana/plugins /srv/elk/kibana/
docker cp kibana:/usr/share/kibana/logs /srv/elk/kibana/

# 删除临时容器
docker stop kibana
docker rm kibana
```


**修改配置文件**
vim vim /root/elk/kibana/config/kibana.yml
* 增加：i18n.locale: "zh-CN"
* 修改：elasticsearch.hosts: ['https://172.20.0.2:9200']，将IP改成elasticsearch的docker ip，注意一定要用https
* 修改：`xpack.fleet.outputs: [{id: fleet-default-output, name: default, is_default: true, is_default_monitoring: true, type: elasticsearch, hosts: ['https://172.20.0.2:9200'], ca_trusted_fingerprint: xxxxxxxxxx}]`
* 将IP改成elasticsearch的docker ip，注意一定要用https
* 
```shell


```

**运行容器**
```shell
docker run -it \
	-d \
	--restart=always \
	--log-driver json-file \
	--log-opt max-size=100m \
	--log-opt max-file=2 \
	--name kibana \
	-p 5601:5601 \
	--net elastic \
	-v /srv/elk/kibana/config:/usr/share/kibana/config \
	-v /srv/elk/kibana/data:/usr/share/kibana/data \
	-v /srv/elk/kibana/plugins:/usr/share/kibana/plugins \
	-v /srv/elk/kibana/logs:/usr/share/kibana/logs \
	docker.elastic.co/kibana/kibana:8.11.3
```

