# docker-nginx-http3

[![Docker Pulls](https://img.shields.io/docker/pulls/ranadeeppolavarapu/nginx-http3?color=brightgreen)](https://hub.docker.com/r/ranadeeppolavarapu/nginx-http3)
[![Docker Cloud Build Status](https://img.shields.io/docker/cloud/build/ranadeeppolavarapu/nginx-http3)](https://hub.docker.com/r/ranadeeppolavarapu/nginx-http3)
[![Docker Cloud Automated build](https://img.shields.io/docker/cloud/automated/ranadeeppolavarapu/nginx-http3?color=brightgreen)](https://hub.docker.com/r/ranadeeppolavarapu/nginx-http3)
![GitHub](https://img.shields.io/github/license/RanadeepPolavarapu/docker-nginx-http3)
[![Contributor Covenant](https://img.shields.io/badge/Contributor%20Covenant-v1.4%20adopted-ff69b4.svg)](code-of-conduct.md)

Alpine Linux image with nginx with HTTP/3 (QUIC), TLSv1.3, 0-RTT, brotli support. All built on the bleeding edge. Built on the edge, for the edge.

HTTP/3 support provided from the smart people at [CloudFlare](https://cloudflare.com) with the [cloudflare/quiche](https://github.com/cloudflare/quiche) project.

Images for this are available on [Docker Hub](https://hub.docker.com/r/ranadeeppolavarapu/nginx-http3).

`docker pull ranadeeppolavarapu/nginx-http3`

## Contributing

Contributions are welcome. Please feel free to contribute 😊.

## Features

- HTTP/3 (QUIC) via CloudFlare's quiche
- HTTP/2 (with Server Push)
- HTTP/2
- BoringSSL (Google's flavor of OpenSSL)
- TLS 1.3 **with 0-RTT support**
- Brotli compression
- [headers-more-nginx-module](https://github.com/openresty/headers-more-nginx-module)
- Alpine Linux (total size of **51 MB** compressed)

## Future Additions

Possible additions in the future pending IETF spec approvals.

- [Facebook's zstd over the web](https://tools.ietf.org/html/rfc8478)

## HTTP/3 ENABLED!

Using Chrome Canary with the following CLI flags:

```bash
--flag-switches-begin --enable-quic --quic-version=h3-23 --enable-features=EnableTLS13EarlyData --flag-switches-end
```

Run on Mac OS (**darwin**):

```bash
"/Applications/Google Chrome Canary.app Contents/MacOS/Google Chrome Canary" \
  --flag-switches-begin \
  --enable-quic \
  --quic-version=h3-23 \
  --enable-features=EnableTLS13EarlyData \
  --flag-switches-end
```

### HTTP/3 (QUIC) Proof

Since HTTP/3 is experimental, we have to be sensible with it. Therefore, below is HTTP/3 in production on one of my web apps 🙃.

![h3](https://user-images.githubusercontent.com/7084995/67162952-831d5800-f337-11e9-9297-05241a693cc4.png)

## HTTP/2 with Server Push

![alt](https://user-images.githubusercontent.com/7084995/67162942-654ff300-f337-11e9-9dc0-6d7a915d517c.png)

## TLS v1.3

![ssllabs](https://user-images.githubusercontent.com/7084995/67164526-89b4cb00-f349-11e9-87a2-d2dc81610ed4.png)

### 0-RTT Proof

![tls-0-rtt](https://user-images.githubusercontent.com/7084995/67163692-08a50600-f340-11e9-830c-c8a11c824a1f.png)

### Testing 0-RTT

```bash
host=domain.example.com # Replace your domain.
echo -e "GET / HTTP/1.1\r\nHost: $host\r\nConnection: close\r\n\r\n" > request.txt
openssl s_client -connect $host:443 -tls1_3 -sess_out session.pem -ign_eof < request.txt
openssl s_client -connect $host:443 -tls1_3 -sess_in session.pem -early_data request.txt
```
