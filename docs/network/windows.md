# windows 静态路由

## 添加静态路由

```shell
route add 192.168.89.0 mask 255.255.255.0 192.168.88.174
route add 192.168.89.0/24 192.168.88.174
```


## 删除静态路由

```shell
route delete 192.168.89.0 MASK 255.255.255.0 192.168.88.174
route delete 192.168.89.0/24 192.168.88.174
```