## Description

[Traefik](https://github.com/traefik/traefik) - the Cloud Native Application Proxy.

[Documentation](https://doc.traefik.io/traefik/)

### Expose docker container example

In `docker-compose.yml` of application add:

```
services:
  servicename:
    ...
    labels:
      - "traefik.enable=true"
      - "traefik.http.services.appname.loadbalancer.server.port=80"
      - "traefik.http.routers.appname-https.entrypoints=websecure"
      - "traefik.http.routers.appname-https.rule=Host(`appurl.com`) || Host(`www.appurl.com`)"
      - "traefik.http.routers.appname-https.tls=true"
      - "traefik.http.routers.appname-https.tls.certresolver=lets-encrypt"
      - "traefik.http.routers.appname-http.entrypoints=web"
      - "traefik.http.routers.appname-http.rule=Host(`appurl.com`) || Host(`www.appurl.com`)"
      - "traefik.http.routers.appname-http.middlewares=http2https"
```
