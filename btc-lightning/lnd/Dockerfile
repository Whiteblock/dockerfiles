FROM golang:1.13-alpine as builder

LABEL maintainer="Olaoluwa Osuntokun <laolu@lightning.engineering>"

# Force Go to use the cgo based DNS resolver. This is required to ensure DNS
# queries required to connect to linked containers succeed.
ENV GODEBUG netdns=cgo

# Install dependencies and install/build lnd.
RUN apk add --no-cache --update alpine-sdk \
    git \
    make

# Copy in the local repository to build from.
RUN go get -d github.com/lightningnetwork/lnd

RUN cd /go/src/github.com/lightningnetwork/lnd \
&&  make \
&&  make install tags="signrpc walletrpc chainrpc invoicesrpc routerrpc"

# Start a new, final image to reduce size.
FROM alpine as final

# Expose lnd ports (server, rpc).
EXPOSE 9735 10009

# Copy the binaries and entrypoint from the builder image.
COPY --from=builder /go/bin/lncli /bin/
COPY --from=builder /go/bin/lnd /bin/

# Add bash.
RUN apk add --no-cache \
    bash

# Copy the entrypoint script.
ADD 'https://raw.githubusercontent.com/lightningnetwork/lnd/master/docker/lnd/start-lnd.sh' ./start-lnd.sh
RUN chmod +x start-lnd.sh