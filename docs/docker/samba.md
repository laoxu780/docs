# samba

```shell
docker run -d \
--restart=always \
--name samba \
-p 139:139 \
-p 445:445 \
-v /mnt/disk2:/mount \
-v /hdd_1/share:/share \
-v /hdd_1/pp:/pp \
dperson/samba \
-u "xuyh;123456" \
-s "disk2;/mount;yes;no;no;xuyh;xuyh;xuyh;none" \
-s "pp;/pp;yes;no;no;xuyh;xuyh;xuyh;none" \
-s "share;/share;yes;no;no;xuyh;xuyh;xuyh;none"
```


```shell

docker run -d \
--restart=always \
--name samba \
-p 139:139 \
-p 445:445 \
-v /hdd_1/share:/share \
dperson/samba \
-u "xuyh;1015" \
-s "share;/share;yes;no;no;xuyh;xuyh;xuyh;none"
```