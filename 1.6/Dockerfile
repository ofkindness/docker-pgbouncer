FROM buildpack-deps:jessie

MAINTAINER Andrei Tretyakov <andrei.tretyakov@gmail.com>

ENV PGBOUNCER_VERSION 1.6.1

RUN cd /usr/src \
  && curl -SLO "https://pgbouncer.github.io/downloads/files/$PGBOUNCER_VERSION/pgbouncer-$PGBOUNCER_VERSION.tar.gz" \
  && tar xvzf "pgbouncer-$PGBOUNCER_VERSION.tar.gz" \
  && cd "pgbouncer-$PGBOUNCER_VERSION/" \
  && ./autogen.sh \
  && ./configure --prefix=/usr/local \
  && make \
  && make install \
  && rm  -rf /usr/src/pgbouncer-* \
  && mkdir -p /etc/pgbouncer /var/log/pgbouncer /var/run/pgbouncer \
  && groupadd -r postgres && useradd -r -g postgres postgres \
  && chown -R postgres:postgres /etc/pgbouncer /var/log/pgbouncer /var/run/pgbouncer

ENV CONFD_VERSION 0.10.0

RUN wget --no-check-certificate --output-document=/usr/local/bin/confd "https://github.com/kelseyhightower/confd/releases/download/v$CONFD_VERSION/confd-$CONFD_VERSION-linux-amd64" \
  && chmod +x /usr/local/bin/confd \
  && chown -R postgres:postgres /usr/local/bin/confd

RUN mkdir -p /etc/confd/{conf.d,templates}

COPY config/pgbouncer.toml /etc/confd/conf.d/pgbouncer.toml
COPY config/users.toml /etc/confd/conf.d/users.toml
COPY config/pgbouncer.ini.tmpl /etc/confd/templates/pgbouncer.ini.tmpl
COPY config/users.txt.tmpl /etc/confd/templates/users.txt.tmpl

RUN apt-get update \
  && apt-get install -y supervisor \
  && rm -rf /var/lib/apt/lists/*

RUN mkdir -p /var/log/supervisor

COPY config/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

CMD ["/usr/bin/supervisord"]

EXPOSE 6543