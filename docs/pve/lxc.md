# lxc

## 映射目录

在/etc/pve/lxc/101.conf配置文件里增加如下配置


```shell
nano /etc/pve/lxc/101.conf
```

```text
lxc.mount.entry: /mnt/disk1 mnt/disk1 none bind,create=dir 0 0
```