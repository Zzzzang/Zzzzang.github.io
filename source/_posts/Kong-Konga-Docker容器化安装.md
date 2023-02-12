title: Kong/Konga - Docker容器化安装
toc: true
cover: /gallery/covers/CP77-COVER.jpg
author: Zzzang
tags:
  - Docker
categories:
  - Docker
date: 2023-02-06 01:37:00
---
# 1.0 安装kong + postgresDB
```
docker network create kong-net
```
<!--more-->
```
docker pull postgres:latest
```
```
docker run -d --name kong-database \
               --network=kong-net \
               -p 5432:5432 \
               -e "POSTGRES_USER=kong" \
               -e "POSTGRES_DB=kong" \
               postgres:latest
```

压缩版：`docker run -d --name kong-database --network=kong-net -p 5432:5432 -e "POSTGRES_USER=kong" -e "POSTGRES_DB=kong" postgres:latest`

`docker pull kong:latest`

```
docker run --rm \
     --network=kong-net \
     -e "KONG_DATABASE=postgres" \
     -e "KONG_PG_HOST=kong-database" \
     kong:latest kong migrations bootstrap
```
压缩版：`docker run --rm --network=kong-net -e "KONG_DATABASE=postgres" -e "KONG_PG_HOST=kong-database" kong:latest kong migrations bootstrap`

```
docker run -d --name kong \
     --network=kong-net \
     -e "KONG_DATABASE=postgres" \
     -e "KONG_PG_HOST=kong-database" \
     -e "KONG_PROXY_ACCESS_LOG=/dev/stdout" \
     -e "KONG_ADMIN_ACCESS_LOG=/dev/stdout" \
     -e "KONG_PROXY_ERROR_LOG=/dev/stderr" \
     -e "KONG_ADMIN_ERROR_LOG=/dev/stderr" \
     -e "KONG_ADMIN_LISTEN=0.0.0.0:8001, 0.0.0.0:8444 ssl" \
     -p 8000:8000 \
     -p 8443:8443 \
     -p 8001:8001 \
     -p 8444:8444 \
     kong:latest
```
压缩版：`docker run -d --name kong --network=kong-net -e "KONG_DATABASE=postgres" -e "KONG_PG_HOST=kong-database" -e "KONG_PROXY_ACCESS_LOG=/dev/stdout"  -e "KONG_ADMIN_ACCESS_LOG=/dev/stdout" -e "KONG_PROXY_ERROR_LOG=/dev/stderr" -e "KONG_ADMIN_ERROR_LOG=/dev/stderr" -e "KONG_ADMIN_LISTEN=0.0.0.0:8001, 0.0.0.0:8444 ssl" -p 8000:8000 -p 8443:8443 -p 8001:8001 -p 8444:8444 kong:latest`

`部署成功后，可以访问http://{ip}:8001/检查是否正常运行`


# 2.0 安装监控 Konga
```
docker pull pantsel/konga:latest
```

 - 方案1 konga
```
docker run --rm pantsel/konga:latest -c prepare -a postgres -u postgresql://kong:@172.18.0.1:5432/konga
```

```
docker run -p 1337:1337 \
        --network kong-net \
        --name konga \
        -e "NODE_ENV=production"  \
        -e "DB_ADAPTER=postgres" \
        -e "DB_URI=postgresql://kong:@172.18.0.1:5432/konga" \
        pantsel/konga
```
压缩版:`docker run -p 1337:1337 --network kong-net --name konga -e "NODE_ENV=production"  -e "DB_ADAPTER=postgres" -e "DB_URI=postgresql://kong:@172.18.0.1:5432/konga" pantsel/konga`

 - 方案2 kong-dashboard
```
docker run --rm -p 8080:8080 --network=kong-net pgbi/kong-dashboard  start --kong-url http://kong:8001
```


`http://{konga-ip}:1337/`