# docker 部署翻译服务


## 启动

```shell
docker run \
  --name translate \
  -p 5000:5000 \
  -d libretranslate/libretranslate --load-only zh,en,ru
```

## 停止
```shell
docker rm -f translate
```