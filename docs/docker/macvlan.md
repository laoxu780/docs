# macvlan


创建网络
```shell
docker network create -d macvlan --subnet=192.168.50.0/24 --gateway=192.168.50.1 --ipv6 --subnet=240e:XXXX:XXXX:XXXX::/60 --gateway=240e:XXXX:XXXX:XXXX::1 -o parent=eth0 macnet

docker network create -d macvlan --subnet=192.168.16.0/24 --gateway=192.168.16.1 -o parent=eno1 macvlan1



docker network create -d macvlan \
    --subnet=192.168.16.0/24 \
    --gateway=192.168.16.1 \
    --subnet=2001:db8:abc8::/64 \
    --gateway=2001:db8:abc8::10 \
     -o parent=eno1 \
     macvlan1

```

