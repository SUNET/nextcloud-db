FROM ubuntu:focal
# Distro should match the one in the proxysql package name
ARG DISTRO=ubuntu20

# Proxysql version, based on https://github.com/sysown/proxysql/releases
ARG VERSION=2.7.1

RUN apt-get update && \
    apt-get install -y wget mariadb-client && \
    wget https://github.com/sysown/proxysql/releases/download/v${VERSION}/proxysql_${VERSION}-${DISTRO}_amd64.deb -O /opt/proxysql_${VERSION}-${DISTRO}_amd64.deb && \
    dpkg -i /opt/proxysql_${VERSION}-${DISTRO}_amd64.deb && \
    rm -f /opt/proxysql_${VERSION}-${DISTRO}_amd64.deb && \
    rm -rf /var/lib/apt/lists/*

VOLUME /var/lib/proxysql
EXPOSE 6032 6033 6080

COPY entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
