FROM debian:bookworm-slim

ARG DEBIAN_FRONTEND=noninteractive

RUN mkdir /docker-entrypoint-initdb.d
RUN mkdir /backups


RUN apt-get update; \
  apt-get upgrade -y; \
  apt-get install -y \
  gosu \
  mariadb-backup \
  mariadb-client \
  mariadb-server \
  openssl \
  pigz \
  pv \
  rsync \
  socat \
  sysstat \
  tzdata \
  ; \
  rm -rf /var/lib/apt/lists/*; \
  # purge and re-create /var/lib/mysql with appropriate ownership
  rm -rf /var/lib/mysql; \
  mkdir -p /var/lib/mysql /var/run/mysqld; \
  chown -R mysql:mysql /var/lib/mysql /var/run/mysqld; \
  # ensure that /var/run/mysqld (used for socket and lock files) is writable regardless of the UID our mysqld instance ends up having at runtime
  chmod 777 /var/run/mysqld; \
  # comment out a few problematic configuration values
  find /etc/mysql/ -name '*.cnf' -print0 \
  | xargs -0 grep -lZE '^(bind-address|log|user\s)' \
  | xargs -rt -0 sed -Ei 's/^(bind-address|log|user\s)/#&/'; \
  # don't reverse lookup hostnames, they are usually another container
  echo '[mysqld]\nskip-host-cache\nskip-name-resolve' > /etc/mysql/conf.d/docker.cnf

VOLUME /var/lib/mysql
VOLUME /backups

COPY docker-entrypoint.sh /usr/local/bin/
ENTRYPOINT ["docker-entrypoint.sh"]

EXPOSE 3306
CMD ["mysqld"]
