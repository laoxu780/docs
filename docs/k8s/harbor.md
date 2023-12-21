# harbor搭建镜像仓库

## 安装
> 下载地址 https://github.com/goharbor/harbor/releases

```shell
$ wget https://github.com/goharbor/harbor/releases/download/v2.7.4/harbor-offline-installer-v2.7.4.tgz

## 安装docker-compose工具
curl -L "https://get.daocloud.io/docker/compose/releases/download/v1.25.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
 
## 解压缩
tar -zxvf harbor-offline-installer-v2.7.4.tgz
 
## 拷贝模板文件
cp harbor.yml.tmpl harbor.yml

## 编辑harbor配置文件
vim harbor.yml
```

```
  5 hostname: 101.43.4.210           //仓库地址改为自己的IP
  6 
  8 http:
 
 10   port: 80                    //对外暴露端口
 
 11 
 12 # https related config
 13 #https:                         //https我们这里不用全都注释掉
 14   # https port for harbor, default is 443
 15 #  port: 443
 16   # The path of cert and key files for nginx
 17 #  certificate: /your/certificate/path
 18 #  private_key: /your/private/key/path
 
 
 
 47 data_volume: /data           //仓库数据存储目录，根据自己需求修改
                                 //仓库大多情况下都是独立的一台或多台主从服务器
```

安装部署
```shell
sh install.sh
```

访问harbor页面
```
http://192.168.10.85:80 
//默认登陆
admin
Harbor12345
```


参考链接:
https://blog.csdn.net/weixin_48704094/article/details/130147424