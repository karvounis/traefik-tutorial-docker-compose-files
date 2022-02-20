# Docker compose file for advanced Traefik configuration

This folder contains the `docker-compose.yml` files for advanced Traefik configuration.

1. [TLS](./docker-compose.01.tls.yml): Creates a new entrypoint and uses TLS for all requests.
1. [Dashboard](./docker-compose.02.dashboard.yml): Enables Traefik's dashboard and api and expose it via a router.
1. [Auth](./docker-compose.03.auth.yml): Shows how to enable and configure authentication (`BasicAuth` and `DigestAuth`) on a route.
1. [Chain](./docker-compose.04.chain.yml): Shows how to configure a chain middleware. In this particular use case, the `whoami_route` first calls the `simple_ratelimit` and then the `digest_auth` middleware.
1. [Metrics](./docker-compose.05.metrics.yml): Enables the metrics endpoint and configure it with a custom router.

## TLS

The following commands have been executed in order to create the certificates under the [certs/](./certs/) folder.

```bash
mkdir -p certs/{ca,traefik}
openssl genrsa -out certs/ca/rootCA.key 4096
openssl req -x509 -new -nodes -key certs/ca/rootCA.key -sha256 -days 3650 -out certs/ca/rootCA.pem -subj "/C=GR/L=Athens/O=Karvounis Tutorials, Inc./CN=Karvounis Root CA/OU=CA department"

openssl genrsa -out certs/traefik/traefik.key 4096
openssl req -new -key certs/traefik/traefik.key -out certs/traefik/traefik.csr -subj "/C=GR/L=Athens/O=Karvounis Tutorials, Inc./CN=*.karvounis.tutorial/OU=Dev.to"
openssl x509 -req -in certs/traefik/traefik.csr -CA certs/ca/rootCA.pem -CAkey certs/ca/rootCA.key -CAcreateserial -out certs/traefik/traefik.crt -days 3650 -sha256
```
