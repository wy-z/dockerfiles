
FROM alpine

ENV REPO kelseyhightower/confd
ENV GOPATH /root/go
ENV REPO_DIR ${GOPATH}/src/github.com/kelseyhightower/confd
ENV BUILD_DEP curl git jq make go gcc musl-dev

RUN set -ex \
    && apk add --no-cache ${BUILD_DEP} \
    && mkdir -p $(dirname ${REPO_DIR}) \
    && git clone https://github.com/${REPO} ${REPO_DIR} \
    && cd ${REPO_DIR} \
    && git checkout $(curl -sSL https://api.github.com/repos/${REPO}/releases/latest | jq --raw-output '.tag_name') \
    && make \
    && make install \
    && cd ~ \
    && apk del --purge ${BUILD_DEP} \
    && rm -rf ${GOPATH} \
    && rm -rf /var/cache/apk/*

CMD ["confd"]
