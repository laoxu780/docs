# samba

```shell
docker run -d \
--restart=always \
--name samba \
-p 139:139 \
-p 445:445 \
-v /mnt/disk2:/mount \
dperson/samba \
-u "xuyh;123456" \
-s "disk2;/mount;yes;no;no;xuyh;xuyh;xuyh;none"
```