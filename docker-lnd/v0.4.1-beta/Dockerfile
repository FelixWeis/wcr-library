FROM golang:1.10

LABEL maintainer="Felix Weis <twitter.com/felixweis>"

# Expose lnd ports (server, rpc).
EXPOSE 9735 10009

# Force Go to use the cgo based DNS resolver. This is required to ensure DNS
# queries required to connect to linked containers succeed.
ENV GODEBUG netdns=cgo

# Install dep to manage vendor.
# Grab and install the latest version of lnd and all related dependencies.
RUN git clone -b 'v0.4.1-beta' --single-branch --depth 1 https://github.com/lightningnetwork/lnd $GOPATH/src/github.com/lightningnetwork/lnd

# Make lnd folder default.
WORKDIR $GOPATH/src/github.com/lightningnetwork/lnd

# Install dependencies and install/build lnd.
RUN make && make install 
RUN strip /go/bin/*

FROM debian:9-slim
COPY --from=0 /go/bin/* /usr/local/bin/

