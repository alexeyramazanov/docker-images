# DEPRECATED

Use traefik instead.

## Description

Nginx proxy with automated Let's Encrypt.
Uses official nginx image instead of nginx-proxy bundle.

Based on:
- [docker-letsencrypt-nginx-proxy-companion](https://github.com/nginx-proxy/docker-letsencrypt-nginx-proxy-companion)
- [nginx-proxy](https://github.com/nginx-proxy/nginx-proxy)
- [docker-gen](https://github.com/jwilder/docker-gen)

Additional details:
- https://github.com/nginx-proxy/docker-letsencrypt-nginx-proxy-companion/blob/master/docs/Advanced-usage.md
- https://github.com/nginx-proxy/nginx-proxy#separate-containers

## Requirements

Download latest [nginx.tmpl](https://github.com/jwilder/nginx-proxy/blob/master/nginx.tmpl) and place it in `docker-gen` folder.

## Optional

Create new docker network:

```bash
docker network create main
```

If you don't need/want separate network - don't forget to remove `networks` block from `docker-compose.yml` 

## Usage

```bash
docker-compose up -d
```

## Proxying other containers example

`docker-compose.yml`:

```yaml
version: '3'

networks:
  default:
    external:
      name: main

services:
  website:
    image: nginx:mainline-alpine
    container_name: website
    volumes:
      - ./www:/usr/share/nginx/html:ro
    environment:
      VIRTUAL_HOST: website.com
      LETSENCRYPT_HOST: website.com,www.website.com
    restart: always
```
