# 文件系统

## Linux系统中查看分区文件系统类型的方法有以下几种：

### 1. 使用lsblk命令查看磁盘信息：lsblk命令可以列出磁盘分区信息，并显示每个分区的文件系统类型。常用选项如下：

linux系统中查看分区文件系统类型的方法

-f：显示每个分区的文件系统类型。

示例：

```shell
lsblk -f            # 列出所有分区信息，并显示文件系统类型
lsblk -f /dev/sda1  # 列出/dev/sda1分区的信息，并显示文件系统类型
```

### 2.使用df命令查看磁盘信息：df命令可以显示磁盘使用情况，包括每个分区的文件系统类型。常用选项如下：
-T：显示文件系统类型。

示例：
```shell
df -T            # 列出所有分区的使用情况，并显示文件系统类型
df -T /dev/sda1  # 列出/dev/sda1分区的使用情况，并显示文件系统类型
```

### 3.使用mount命令查看挂载信息：mount命令可以显示已经挂载的文件系统信息，包括每个分区的文件系统类型。常用选项如下：

-t：显示指定类型的文件系统信息。

示例：

```shell
mount            # 列出所有挂载的文件系统信息，并显示文件系统类型
mount -t ext4    # 列出所有ext4类型的文件系统信息，并显示文件系统类型
mount /dev/sda1  # 挂载/dev/sda1分区，并显示文件系统类型
```

### 挂载samba远程目录

**安装需要的包**

```shell
apt install cifs-utils
```
**指定密码**

`nano /root/.smbcredentials`

```text
username=xuyh
password=1015
```

**修改/etc/fstab文件**
增加一行
```text
//192.168.16.5/share /mnt/share cifs credentials=/root/.smbcredentials,iocharset=utf8 0 0
```