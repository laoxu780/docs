# 自签名证书

## 生成 CA 证书

```shell
## 生成 CA 证书私钥
openssl genrsa -out ca.key 4096

## 生成 CA 证书
openssl req -x509 -new -nodes -sha512 -days 3650 \
 -subj "/C=CN/ST=Beijing/L=Beijing/O=example/OU=Personal/CN=sz56t.local" \
 -key ca.key \
 -out ca.crt

```

## 生成 Server 证书

```shell
## 生成 Server 私钥
openssl genrsa -out harbor.local.key 4096

## 生成 Server 证书签名请求（CSR）
openssl req -sha512 -new \
    -subj "/C=CN/ST=Beijing/L=Beijing/O=example/OU=Personal/CN=harbor.local" \
    -key harbor.local.key \
    -out harbor.local.csr
```

## 使用以下命令生成 x509 v3 扩展文件：

```shell
cat > v3.ext <<-EOF
authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
extendedKeyUsage = serverAuth
subjectAltName = IP:11.8.36.21
EOF
```
域名方式

```shell
cat > v3.ext <<-EOF
authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
extendedKeyUsage = serverAuth
subjectAltName = @alt_names

[alt_names]
DNS.1=harbor.local
EOF

```


## 使用 CA 证书签发 Server 证书

```shell
openssl x509 -req -sha512 -days 3650 \
    -extfile v3.ext \
    -CA ca.crt -CAkey ca.key -CAcreateserial \
    -in harbor.local.csr \
    -out harbor.local.crt
```

## 转换 server.crt 为 server.cert

```shell
openssl x509 -inform PEM -in harbor.local.crt -out harbor.local.cert
```

```
/root/certs
ca.crt  ca.key  ca.srl  harbor.local.cert  harbor.local.crt  harbor.local.csr  harbor.local.key  v3.ext
```