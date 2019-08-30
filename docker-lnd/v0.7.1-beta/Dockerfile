FROM golang:1.12-alpine3.10 as builder
LABEL maintainer="Felix Weis <twitter.com/FelixWeis>"

# Install dep to manage vendor.
# Grab and install the latest version of lnd and all related dependencies.
RUN apk add --no-cache make git binutils

RUN \
    git clone --depth 1 --branch 'v0.7.1-beta' --single-branch \
    https://github.com/lightningnetwork/lnd \
    $GOPATH/src/github.com/lightningnetwork/lnd

# Install dependencies and install/build lnd.
RUN \
    cd $GOPATH/src/github.com/lightningnetwork/lnd && \
    make && \
    make install

RUN strip -s /go/bin/*

FROM alpine:3.10
RUN apk add --no-cache ca-certificates

COPY --from=builder /go/bin/lnd /go/bin/lncli /usr/local/bin/

# Force Go to use the cgo based DNS resolver. This is required to ensure DNS
# queries required to connect to linked containers succeed.
ENV GODEBUG netdns=cgo
# Expose lnd ports (server, rpc).
EXPOSE 9735 10009
WORKDIR /root/.lnd
VOLUME [ "/root/.lnd" ]
