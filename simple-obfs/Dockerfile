
FROM alpine

ENV REPO shadowsocks/simple-obfs
ENV REPO_DIR simple-obfs

RUN set -ex \
    && apk add --no-cache libcrypto1.0 \
                          libev \
                          libsodium \
                          mbedtls \
                          pcre \
                          udns \
    && apk add --no-cache \
               --virtual TMP autoconf \
                             automake \
                             build-base \
                             jq \
                             git \
                             curl \
                             gettext-dev \
                             libev-dev \
                             libsodium-dev \
                             libtool \
                             linux-headers \
                             mbedtls-dev \
                             openssl-dev \
                             pcre-dev \
                             tar \
                             udns-dev \
    && git clone https://github.com/${REPO} \
    && cd ${REPO_DIR} \
    && git checkout $(curl -sSL https://api.github.com/repos/${REPO}/releases/latest | jq --raw-output '.tag_name') \
    && git submodule update --init --recursive \
    && ./autogen.sh \
    && ./configure --disable-documentation \
    && make install \
    && cd .. \
    && rm -rf $REPO_DIR \
    && apk del --purge TMP \
    && rm -rf /var/cache/apk/*

CMD ["obfs-server"]
