FROM golang:1.16-alpine3.13 as builder

RUN apk add --no-cache \
    make pkgconfig autoconf automake libtool py-docutils git gcc g++ libevent-dev openssl-dev c-ares-dev ca-certificates

ARG REMCO_VERSION=0.12.1

ADD https://github.com/HeavyHorst/remco/archive/refs/tags/v${REMCO_VERSION}.tar.gz /tmp/

RUN mkdir -p /go/src/github.com/HeavyHorst/remco && \
    cd /go/src/github.com/HeavyHorst/remco && \
    tar --strip-components=1 -zxf /tmp/v${REMCO_VERSION}.tar.gz && \
    make

ARG PGBOUNCER_VERSION=1.15.0

RUN wget http://www.pgbouncer.org/downloads/files/${PGBOUNCER_VERSION}/pgbouncer-${PGBOUNCER_VERSION}.tar.gz -P /tmp/ && \
    cd /tmp && \
    tar xvzf "pgbouncer-$PGBOUNCER_VERSION.tar.gz"

RUN cd "/tmp/pgbouncer-"$PGBOUNCER_VERSION && \
  ./autogen.sh && \
  ./configure --prefix=/tmp/pgbouncer && \
  make && \
  make install

FROM alpine:3.13

LABEL maintainer="Andrei Tretyakov <andrei.tretyakov@gmail.com>"

COPY --from=builder /go/src/github.com/HeavyHorst/remco/bin/remco /usr/local/bin/remco

RUN apk --no-cache add libevent openssl c-ares

COPY --from=builder /tmp/pgbouncer /pgbouncer

COPY docker-entrypoint.sh /

ENTRYPOINT ["/docker-entrypoint.sh"]