# ubuntu 使用nfs

## 安装


**安装客户端**
```shell
sudo apt update  # 更新系统软件包
sudo apt install nfs-common  # 安装NFS客户端
```


## 挂载

```shell
mount remotehost:/remote/path /local/path
```

例如:
```shell
sudo mkdir -p /mnt/nfs  # 创建本地挂载目录
sudo mount 192.168.0.100:/home/share /mnt/nfs  # 挂载远程共享目录到本地目录
```


## 自动挂载

通过上述方法挂载远程文件系统需要手动执行mount命令来完成，如果我们希望在系统启动时自动挂载远程共享目录，可以在/etc/fstab文件中添加挂载配置：
```shell
remotehost:/remote/path /local/path nfs defaults 0 0
```

192.168.16.2:/mnt/hdd_1/media /mnt/media nfs defaults 0 0