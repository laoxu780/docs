# macvlan


## 创建网络

```shell
docker network create -d macvlan \
    --subnet=192.168.16.0/24 \
    --gateway=192.168.16.1 \
    --subnet=2001:db8:abc8::/64 \
    --gateway=2001:db8:abc8::10 \
     -o parent=eno1 \
     macvlan1
```


## 容器指定ip

```shell
docker run --rm -d -it --network macvlan1 --ip 192.168.16.10 nginx:1.23.2
```