FROM alpine:latest

ARG PROXY_VERSION=0.8.13

RUN set -ex \
    && apk add --no-cache --virtual .3proxy-builddeps \
      ca-certificates \
      wget \
      musl-dev \
      make \
      gcc \
    && wget -O 3proxy.tar.gz "https://github.com/z3APA3A/3proxy/archive/$PROXY_VERSION.tar.gz" \
    && mkdir -p /usr/src/3proxy \
    && tar -xzf 3proxy.tar.gz -C /usr/src/3proxy --strip-components=1 \
    && rm 3proxy.tar.gz \
    && cd /usr/src/3proxy \
    && make -f Makefile.Linux PLUGINS='' \
    && mv /usr/src/3proxy/src/3proxy /usr/local/bin/3proxy \
    && cd / \
    && rm -r /usr/src/3proxy \
    && apk del .3proxy-builddeps

RUN apk add --no-cache \
    openvpn \
    runit

COPY filesystem/. /

CMD ["/usr/local/bin/runit_init"]
