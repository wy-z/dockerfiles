
FROM ubuntu:16.04

ENV REPO https://github.com/bitshares/bitshares-core
ENV REPO_DIR bitshares-core
ENV BUILD_DEP curl jq libtool autoconf cmake git libboost-all-dev libssl-dev

RUN set -ex \
    && apt-get update \
    && apt-get install -y ${BUILD_DEP}
RUN set -ex \
    && curl -L $(curl -sSL https://api.github.com/repos/${REPO}/releases/latest | \
       jq --raw-output '.assets[0] | .browser_download_url') > ${REPO_DIR}.tar.gz \
    && tar zxf  ${REPO_DIR}.tar.gz
RUN set -ex \
    && cd ${REPO_DIR}* \
        && git submodule update --init --recursive \
        && cmake -DCMAKE_BUILD_TYPE=RelWithDebInfo . \
        && make \
    && apt-get remove -y ${BUILD_DEP} \
    && apt-get clean

WORKDIR ${REPO_DIR}

CMD ["./programs/witness_node/witness_node"]
