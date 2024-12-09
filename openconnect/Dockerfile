FROM alpine:3.20

ARG OPENCONNECT_VERSION=9.12
ARG OPENCONNECT_URL="https://www.infradead.org/openconnect/download/openconnect-${OPENCONNECT_VERSION}.tar.gz"
ARG OPENCONNECT_VPNC_SCRIPT_URL="https://gitlab.com/openconnect/vpnc-scripts/-/raw/master/vpnc-script"
ARG OPENCONNECT_RUNTIME_DEPS="libxml2"

# runtime dependencies
RUN set -eux; \
	apk add --no-cache \
		${OPENCONNECT_RUNTIME_DEPS} \
	;

# install vpnc-script
RUN set -eux; \
    wget -O /usr/bin/vpnc "$OPENCONNECT_VPNC_SCRIPT_URL"; \
    chmod +x /usr/bin/vpnc;

# compile and install openconnect
RUN set -eux; \
	\
	cd /tmp; \
	apk add --no-cache --virtual .build-deps \
        gcc \
        make \
        musl-dev \
        linux-headers \
        openssl-dev \
        libxml2-dev \
	; \
	wget -O openconnect.tar.gz "$OPENCONNECT_URL"; \
	mkdir -p /usr/src/openconnect; \
	tar -xzf openconnect.tar.gz -C /usr/src/openconnect --strip-components=1; \
	rm openconnect.tar.gz; \
    cd /usr/src/openconnect; \
	\
	./configure \
      --with-vpnc-script=/usr/bin/vpnc \
      --disable-shared \
      --disable-nls; \
	nproc="$(getconf _NPROCESSORS_ONLN)"; \
    make -j "$nproc"; \
    make install; \
    \
    mkdir -p /usr/local/etc/openconnect; \
    cp openconnect /usr/local/bin/openconnect; \
    rm -rf /usr/src/openconnect; \
    \
    apk del --no-network .build-deps;

# verify openconnect works
RUN set -eux; \
    openconnect -V;

CMD ["openconnect", "--config", "/usr/local/etc/openconnect/openconnect.conf"]
