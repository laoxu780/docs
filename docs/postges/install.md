# postgres 安装

## ubuntu 22.04安装postgres

### 更改系统时区

```shell
-- 列出所有可用的时区
timedatectl list-timezones
-- 设置的时区
timedatectl set-timezone Asia/Shanghai
```

### 安装

1. 更新包列表：

```shell
apt update
```

2. 安装PostgreSQL：

```shell
apt install postgresql postgresql-contrib
```

3. 启动PostgreSQL服务：

```shell
systemctl start postgresql
```

4. 确保PostgreSQL随系统启动：

```shell
systemctl enable postgresql
```

5. 切换到PostgreSQL用户（默认为postgres）：

```shell
su postgres
```


6. 创建一个新的角色（可选）：

```shell
createuser --interactive
```


7. 创建一个新的数据库（可选）：

```shell
createdb <your-database-name>
```

| 选项     | 描述     |
| -------- | -------- |
| -E | 指定数据库的编码 |


8. 登录到PostgreSQL命令行界面：
9. 
```shell
psql
```
