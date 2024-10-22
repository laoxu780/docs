FROM nginx:1.23.2

LABEL maintainer="xuyh <328340506@qq.com>"

RUN cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo 'Asia/Shanghai' >/etc/timezone

COPY /dist          /usr/share/nginx/html
COPY /nginx.conf    /etc/nginx/nginx.conf

EXPOSE 3000

CMD ["nginx", "-g", "daemon off;"]
