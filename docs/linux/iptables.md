# linux防火墙 iptables


**修改链默认策略**

```shell
# 查看
iptables -L INPUT --line-numbers | grep "Chain INPUT"

# 修改
iptables -P INPUT <ACTION>

# 默认允许转发
iptables -P FORWARD ACCEPT
```