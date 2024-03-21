# pve


## 虚拟机无法关闭

```shell
ps -ef | grep "/usr/bin/kvm -id 100" | grep -v grep
kill -9 28624
```


## 显卡给lxc使用

```shell
nano /etc/pve/lxc/CTID.conf

lxc.cgroup2.devices.allow: c 226:0 rwm
lxc.cgroup2.devices.allow: c 226:128 rwm
lxc.cgroup2.devices.allow: c 29:0 rwm
lxc.mount.entry: /dev/dri dev/dri none bind,optional,create=dir
lxc.mount.entry: /dev/fb0 dev/fb0 none bind,optional,create=file
lxc.apparmor.profile: unconfined
```