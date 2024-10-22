# mysql安装

## windows

### 1.1 下载压缩包
**下载链接** https://dev.mysql.com/downloads/mysql/



### 1.2

新建环境变量 MYSQL_HOME指向mysql的解压目录

将`%MYSQL_HOME%\bin`添加到PATH变量中


### 新增配置文件

在mysql安装目录下新建文件my.ini

**my.ini**

```
[mysqld]
# 设置3306端口
port=3306
# 设置mysql的安装目录 ---这里输入你安装的文件路径----
basedir=D:\mysql\8.4.0
# 设置mysql数据库的数据的存放目录
datadir=D:\mysql\8.4.0\data
# 允许最大连接数
max_connections=200
# 允许连接失败的次数。
max_connect_errors=10
# 服务端使用的字符集默认为utf8
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
# 默认使用“mysql_native_password”插件认证
#mysql_native_password
default_authentication_plugin=mysql_native_password
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8mb4
[client]
# 设置mysql客户端连接服务端时默认使用的端口
port=3306
default-character-set=utf8mb4
```

### 初始化

`mysqld --initialize --console`

### 初始化密码

`ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY '123456';`

### 安装服务

`mysqld install`


