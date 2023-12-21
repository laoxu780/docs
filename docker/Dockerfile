FROM nginx:1.24.0

LABEL maintainer="xuyh <328340506@qq.com>"

RUN cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo 'Asia/Shanghai' >/etc/timezone

COPY /build          /usr/share/nginx/html
COPY /nginx.conf    /etc/nginx/nginx.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]