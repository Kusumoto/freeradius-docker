FROM ubuntu:zesty

LABEL Author="Weerayut Hongsa <kusumoto.com@gmail.com>"

ARG FREERADIUS_VERSION="3.0.15"

RUN \
    DEBIAN_FRONTEND=noninteractive apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
        libtalloc-dev \
        libkqueue-dev \
        libldap2-dev \
        libpq-dev \
        ca-certificates \
        libssl-dev \
        openssl \
        g++ \
        curl \
    && cd /tmp \
    && curl -fSLk ftp://ftp.freeradius.org/pub/freeradius/freeradius-server-${FREERADIUS_VERSION}.tar.gz -o freeradius-server-${FREERADIUS_VERSION}.tar.gz \
    && tar xzf freeradius-server-${FREERADIUS_VERSION}.tar.gz \
    && cd freeradius-server-${FREERADIUS_VERSION} \
    && ./configure \
    && make \
    && make install \
    && rm -rf /tmp/* \
    && useradd -ms /bin/bash radius \
    && chown -R radius:radius /usr/local/etc/raddb \
    && DEBIAN_FRONTEND=noninteractive apt-get remove --purge -y g++ gcc \
    && DEBIAN_FRONTEND=noninteractive apt-get autoremove -y \
    && DEBIAN_FRONTEND=noninteractive apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*

EXPOSE 1812/udp 1813/udp

ADD radiusd.conf /usr/local/etc/raddb

CMD [ "radiusd", "-X" ]
    

