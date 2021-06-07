FROM golang:alpine3.13

LABEL maintainer="Andrei Tretyakov <andrei.tretyakov@gmail.com>"

ENV CONFD_VERSION 0.16.0

RUN mkdir -p /usr/src/ 

RUN apk add --no-cache curl make git

RUN cd /usr/src \
    && mkdir -p $GOPATH/src/github.com/kelseyhightower \
    && curl -SLO "https://github.com/kelseyhightower/confd/archive/v$CONFD_VERSION.tar.gz" \
    && tar xvzf "v$CONFD_VERSION.tar.gz" -C "$GOPATH/src/github.com/kelseyhightower" \
    && cd "$GOPATH/src/github.com/kelseyhightower/confd-$CONFD_VERSION" \
    && make

ENTRYPOINT ["/docker-entrypoint.sh"]