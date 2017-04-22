
FROM alpine

ENV SS_REPO shadowsocks/shadowsocks-libev
ENV SS_DIR shadowsocks-libev
ENV SS_RUNTIME_DEP libev libsodium pcre udns mbedtls
ENV SS_BUILD_DEP jq git file autoconf automake build-base curl gettext-dev libev-dev \
    libsodium-dev libtool libev-dev pcre-dev udns-dev mbedtls-dev linux-headers

RUN set -ex \
    && apk add --update ${SS_RUNTIME_DEP} ${SS_BUILD_DEP} \
    && curl -L $(curl -sSL https://api.github.com/repos/${SS_REPO}/releases/latest | \
       jq --raw-output '.assets[0] | .browser_download_url') > ${SS_DIR}.tar.gz \
    && tar zxf  ${SS_DIR}.tar.gz \
    && cd ${SS_DIR}* \
        && ./configure --disable-documentation \
        && make install \
        && cd .. \
        && rm -rf ${SS_DIR} \
    && apk del --purge ${SS_BUILD_DEP} \
    && rm -rf /var/cache/apk/*

ENTRYPOINT ["ss-server"]