FROM ubuntu:18.04

ENV PATH=/usr/local/bin:$PATH \
  LANG=C.UTF-8 \
  DEBIAN_FRONTEND=noninteractive

ARG OPENSSL_FIPS_VERSION=2.0.16
ARG OPENSSL_VERSION=1.0.2t

RUN set -ex \
    && apt-get update -y \
    && apt-get -y install curl perl build-essential \
    && mkdir -p /usr/local/src/ \
    && cd /usr/local/src/ \
    && curl -O https://www.openssl.org/source/openssl-fips-${OPENSSL_FIPS_VERSION}.tar.gz \
    && curl -O https://www.openssl.org/source/openssl-${OPENSSL_VERSION}.tar.gz \
    && tar -xvf openssl-fips-${OPENSSL_FIPS_VERSION}.tar.gz \
    && cd openssl-fips-${OPENSSL_FIPS_VERSION} \
    && ./config \
    && make install \
    && cd ../ \
    && rm -f openssl-fips-${OPENSSL_FIPS_VERSION}.tar.gz \
    && rm -rf ./openssl-fips-${OPENSSL_FIPS_VERSION} \
    && tar -xvf openssl-${OPENSSL_VERSION}.tar.gz \
    && cd openssl-${OPENSSL_VERSION} \
    && ./config shared fips no-ssl2 no-ssl3 \
    && make depend \
    && make install \
    && echo "/usr/local/ssl/lib" > /etc/ld.so.conf.d/openssl-${OPENSSL_VERSION}.conf \
    && ldconfig -v \
    && cd ../ \
    && rm -f openssl-${OPENSSL_VERSION}.tar.gz \
    && rm -rf ./openssl-${OPENSSL_VERSION} \
    && mkdir -p /usr/local/ssl/lib/shared \
    && cp /usr/local/ssl/lib/libcrypto.so.1.0.0 /usr/local/ssl/lib/shared/libcrypto.so.1.0.0 \
    && cp /usr/local/ssl/lib/libssl.so.1.0.0 /usr/local/ssl/lib/shared/libssl.so.1.0.0 \
    && ln -sf /usr/local/ssl/lib/shared/libcrypto.so.1.0.0 /usr/local/ssl/lib/shared/libcrypto.so \
    && ln -sf /usr/local/ssl/lib/shared/libssl.so.1.0.0 /usr/local/ssl/lib/shared/libssl.so \
    && rm -f /usr/local/ssl/lib/libcrypto.so /usr/local/ssl/lib/libssl.so \
    && ln -sf /usr/local/ssl/bin/openssl /usr/bin/openssl \
    && apt-get remove -y build-essential \
    && apt-get autoclean \
    && apt-get clean

ENV OPENSSL_FIPS=1 \
    PATH=$PATH:/usr/local/ssl/bin \
    LDFLAGS="-L/usr/local/ssl/lib/ -L//usr/local/ssl/lib64/" \
    LD_LIBRARY_PATH="/usr/local/ssl/lib/:/usr/local/ssl/lib64/" \
    CPPFLAGS="-I/usr/local/ssl/include -I/usr/local/ssl/include/openssl" \
    CFLAGS="-I/usr/local/ssl/include"
