# ssl证书自动申请

* https://soulteary.com/2018/08/30/use-docker-certbot-to-obtain-ssl-certificates.html
* https://www.iminling.com/2023/07/30/186.html
* https://github.com/certbot/certbot



~/.secrets/certbot/cloudflare.ini

```shell
rm -rf /etc/letsencrypt

mkdir ~/.secrets

vim ~/.secrets/cloudflare.ini
# dns_cloudflare_email = 328340506@qq.com 
# dns_cloudflare_api_token = 9a6ZUnov_4039aoWOAjtvNzr9BB0hplbTuU34Swa

docker run -it --rm --name certbot \
  -v /etc/letsencrypt:/etc/letsencrypt \
  -v ~/.secrets/cloudflare.ini:/.secrets/cloudflare.ini \
  certbot/dns-cloudflare:v2.4.0 \
  --server https://acme-v02.api.letsencrypt.org/directory  \
  certonly --dns-cloudflare --dns-cloudflare-credentials /.secrets/cloudflare.ini --dns-cloudflare-propagation-seconds 20 --email 328340506@qq.com --agree-tos --non-interactive -d 174.xuyh.info
  
  
  ## 尝试获取
  certonly --dns-cloudflare --agree-tos --non-interactive --dns-cloudflare-credentials /.secrets/cloudflare.ini --email 328340506@qq.com --dns-cloudflare-propagation-seconds 20 -d 174.xuyh.info --dry-run
  
  ## 获取
  certonly --dns-cloudflare --agree-tos --non-interactive --dns-cloudflare-credentials /.secrets/cloudflare.ini --email 328340506@qq.com --dns-cloudflare-propagation-seconds 20 -d 174.xuyh.info
  
  ## 重新获取
  renew --dns-cloudflare --no-self-upgrade --agree-tos --non-interactive --dns-cloudflare-credentials /.secrets/cloudflare.ini --dns-cloudflare-propagation-seconds 20
  
  
```


```shell
docker run -it --rm --name certbot \
  -v /etc/letsencrypt:/etc/letsencrypt \
  -v ~/.secrets/cloudflare.ini:/.secrets/cloudflare.ini \
  certbot/dns-cloudflare:v2.4.0 \
  renew --dns-cloudflare --no-self-upgrade --agree-tos --non-interactive --dns-cloudflare-credentials /.secrets/cloudflare.ini --dns-cloudflare-propagation-seconds 20

```