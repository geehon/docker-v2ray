# V2ray Client In Docker

[![996.icu](https://img.shields.io/badge/link-996.icu-red.svg)](https://996.icu)
[![LICENSE](https://img.shields.io/badge/license-Anti%20996-blue.svg)](https://github.com/996icu/996.ICU/blob/master/LICENSE)

## install docker

first you need install docker.

[official handbook](https://docs.docker.com/engine/install/ubuntu/)

## modify config file

set `"vnext"` as you v2ray server

```json
"outbounds": [
    {
      "tag": "proxy",
      "protocol": "vmess",
      "settings": {
        "vnext": [
          {
            "address": "www.example.com",
            "port": 46587,
            "users": [
              {
                "id": "ac82e9ba-f610-4ec9-a990-c990cc2ffc69",
                "alterId": 1,
                "email": "t@t.tt",
                "security": "auto"
              }
            ]
          }
        ]
      },
      "streamSettings": {
        "network": "tcp"
      },
      "mux": {
        "enabled": true,
        "concurrency": 8
      }
    },
    {
      "tag": "direct",
      "protocol": "freedom",
      "settings": {}
    },
    {
      "tag": "block",
      "protocol": "blackhole",
      "settings": {
        "response": {
          "type": "http"
        }
      }
    }
  ],
```

## start client

in project root folder `docker-v2ry` run:

```shell
docker run -d --name v2ray --network host -v "$PWD"/v2ray/:/etc/v2ray/ v2fly/v2fly-core
```

## settings in you pc network

`settings`->`NetWork`->`Network Proxy` then set Network Proxy as Manual.Set input field named Socks Host: 127.0.0.1 and port:10808

## set proxy as automatic

create docker network

```shell
docker network create my_net
```

in project root folder run

```shell
sudo docker run -d \
--name mynginx \
--network my_net --network-alias nginx \
-v "$PWD"/www:/usr/share/nginx/html \
-v "$PWD"/nginx/conf.d:/etc/nginx/conf.d \
-p 10809:80 \
nginx
```

## PC settings

set network proxy as automatic and the configureUrl as `http://127.0.0.1:10809/v2ray/pac.txt`

now enjoy freedom😉

## Option: Use docker-compose to start service

first you need install docker-compose

then in project root floder run

```shell
docker-compose up -d
```

> Important! you should restart you v2ray service once change your `config.json` file.

run command:

```shell
docker-compose restart v2ray
```
