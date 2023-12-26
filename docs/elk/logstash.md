# logstash

> 文档地址: https://www.elastic.co/guide/en/logstash/8.11/installing-logstash.html

## 架构
数据源-->input-->fliter-->output-->目的地
```
input {     从哪个地方读取，输入数据。
   
}
 
filter {    依据grok模式对数据进行分析结构化
   
}
 
output {    将分析好的数据输出存储到哪些地方
  
}
```


## 配置示例

从标准输入中输入数据 输出到标准输出

```
input {
  stdin {   
  }
}
 
output {
  stdout {   
    codec => rubydebug
  }
}
```

codec => rubydebug 编码格式

## 输入

### stdin 标准输入

```
  stdin {
  
  }
```

在windows下会乱码
指定编码
```
  stdin {
    codec => plain{ charset => "GB2312" }
  }
```

### file 文件输入

```
    file {
        path => ["E:\tms\logs\*.log"]
        type => "system"
        start_position => "beginning"
    }
```

## 过滤


## 输出

### stdout 标准输出
```
  stdout {
    codec => rubydebug
  }
```

### 输出到文件
```
 file {
   path => C:\Users\32834\Desktop\elk\a.txt
   codec => line { format => "%{message}"}
 }
```

### elasticsearch

```
  elasticsearch {
    hosts => ["192.168.16.10:9200"]
    index => "2023-16-25"
    user => "elastic"
	ssl => true
    ssl_certificate_verification => false
    password => "s2VCOHsO=T=RDPYrBR=x"
  }
```