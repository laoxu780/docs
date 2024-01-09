# java 命令行参数

## java

### 常用参数

| 选项                            | 作用          | 样例 |
|-------------------------------|-------------|----|
| -server<br/>-client           | 指定以那种模式运行   |    |
| -classpath、 -cp               | 指定classpath |    |
| -jar filename                 | 执行jar包      |    |
| -Xms                          | 最小堆内存限制     |    |
| -Xmx                          | 最大堆内存限制     |    |
| -Xss                          | 线程堆栈大小      |    |
| -Dfile.encoding=utf-8         | 指定字符编码      |    |
| -Duser.timezone=Asia/Shanghai | 指定时区        |    |


## 解压jar

```shell
jar -fx ***.jar
```