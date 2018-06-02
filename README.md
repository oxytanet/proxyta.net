# proxyta.net

A simple reverse proxy setup, for both dev & prod, with [docker-compose](https://docs.docker.com/compose/) &
[traefik](https://traefik.io/)

## Launch

1. Create a docker network: `docker network create proxytanet`
2. Run `docker-compose up -d` in the folder corresponding to your environment:
    - [dev](dev/)
    - [prod with letsencrypt](prod-le/) (look at the README first)
    - [prod with you certificates](prod-ssl/) (look at the README first)

## Use in your other services

In the projects using docker-compose you want to reverse-proxy, add those labels to the service that have to be exposed:

```
services:
  your_app:
    [...]
    labels:
      traefik.enable: "true"
      traefik.frontend.rule: "Host: your_app.${DOMAIN_NAME:-local}, www.your_app.${DOMAIN_NAME:-local}"
      traefik.docker.network: "proxytanet"
```

You also need to declare the docker network `proxytanet` as external in the same file:

```
networks:
  proxytanet:
    external: true
```

:warning: Don't forget to setup the DNS for `service.${DOMAIN_NAME:-local}` and its `www.` version :warning:

For this, in dev, you can just add it to your `/etc/hosts`

## Go to Dev from Prod and vice versa

Just launch `docker-compose down` in the old folder and follow the README in the new
