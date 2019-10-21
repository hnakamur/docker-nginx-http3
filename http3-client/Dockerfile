##################################################
# http3-client
##################################################

FROM alpine:latest AS builder

# Point to Alpine Linux edge branch.
RUN echo "http://dl-cdn.alpinelinux.org/alpine/edge/main" > /etc/apk/repositories \
  && echo "http://dl-cdn.alpinelinux.org/alpine/edge/community" >> /etc/apk/repositories \
  && echo "http://dl-cdn.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories

RUN apk add --no-cache --virtual .build-deps \
  git \
  rust \
  cargo \
  cmake \
  make \
  g++ \
  perl \
  go \
  && mkdir -p /usr/src \
  && cd /usr/src \
  && git clone --recursive https://github.com/cloudflare/quiche

RUN cd /usr/src/quiche \
  && cargo build --release --example http3-client


FROM alpine:latest 

COPY --from=builder /usr/src/quiche/target/release//examples/http3-client /usr/bin/

RUN apk add --no-cache libgcc

ENTRYPOINT ["/usr/bin/http3-client"]
CMD ["--no-verify", "https://localhost"]
