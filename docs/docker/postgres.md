# docker 部署postgres

```shell
docker run -d \
    --name postgres \
    -e POSTGRES_DB=tms \
    -e POSTGRES_PASSWORD='qwe123!@#' \
    -e PGDATA=/var/lib/postgresql/data/pgdata \
    -v pgdata:/var/lib/postgresql/data \
    -p 5432:5432 \
    --restart=always \
    postgres:14.10


## 连接postgres

docker run -it --rm postgres:14.10 psql -h 172.17.0.1 -p 5432 -U postgres
```