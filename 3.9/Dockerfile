# Copyright Broadcom, Inc. All Rights Reserved.
# SPDX-License-Identifier: APACHE-2.0

#FROM docker.io/bitnami/minideb:bookworm
FROM debian:sid-slim

ENV HOME="/" \
    OS_FLAVOUR="debian-12" \
    OS_NAME="linux" \
    PATH="/opt/bitnami/common/bin:/opt/bitnami/java/bin:/opt/bitnami/zookeeper/bin:$PATH"

COPY prebuildfs /
SHELL ["/bin/bash", "-o", "errexit", "-o", "nounset", "-o", "pipefail", "-c"]
# Install required system packages and dependencies
RUN install_packages ca-certificates curl netcat-openbsd procps xmlstarlet zlib1g

RUN install_packages openjdk-17-jre-headless

RUN mkdir -p /tmp/bitnami/pkg/cache/

RUN cd /tmp/bitnami/pkg/cache/; \
    COMPONENT=wait-for-port-linux-amd64; \
    mkdir -p /opt/bitnami/common/bin; \
    curl -sSL -O "https://github.com/bitnami/wait-for-port/releases/download/v1.0.10/${COMPONENT}.tar.gz"; \
    tar -zxf "${COMPONENT}.tar.gz" -C /opt/bitnami/common/bin --no-same-owner ; \
    mv /opt/bitnami/common/bin/wait-for-port-linux-amd64 /opt/bitnami/common/bin/wait-for-port; \
    rm -rf ${COMPONENT}.tar.gz;

RUN cd /tmp/bitnami/pkg/cache/; \
    COMPONENT="apache-zookeeper-3.9.4-bin"; \
    curl -sSL -O "https://dlcdn.apache.org/zookeeper/zookeeper-3.9.4/${COMPONENT}.tar.gz"; \
    mkdir -p /opt/bitnami/zookeeper; \
    tar -zxf "${COMPONENT}.tar.gz" -C /opt/bitnami/zookeeper --strip-components=1 --no-same-owner ; \
    rm -rf ${COMPONENT}.tar.gz;

RUN apt-get update && apt-get upgrade -y && \
    apt-get clean && rm -rf /var/lib/apt/lists /var/cache/apt/archives
RUN chmod g+rwX /opt/bitnami
RUN find / -perm /6000 -type f -exec chmod a-s {} \; || true
RUN ln -s /opt/bitnami/scripts/zookeeper/entrypoint.sh /entrypoint.sh
RUN ln -s /opt/bitnami/scripts/zookeeper/run.sh /run.sh

COPY rootfs /
RUN /opt/bitnami/scripts/zookeeper/postunpack.sh
ENV APP_VERSION="3.9.4" \
    BITNAMI_APP_NAME="zookeeper" \
    IMAGE_REVISION="8"

EXPOSE 2181 2888 3888 8080

USER 1001
ENTRYPOINT [ "/opt/bitnami/scripts/zookeeper/entrypoint.sh" ]
CMD [ "/opt/bitnami/scripts/zookeeper/run.sh" ]
