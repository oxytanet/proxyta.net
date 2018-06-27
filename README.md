# proxyta.net

A simple reverse proxy setup, for both dev & prod, with [docker-compose](https://docs.docker.com/compose/) &
[traefik](https://traefik.io/)

You can read more here: [https://saurel.me/proxyta.net](https://saurel.me/proxyta.net)

## Launch

1. Create a docker network: `docker network create proxytanet`
2. Run `docker-compose up -d` in the folder corresponding to your environment:
    - [dev](dev/)
    - [prod with letsencrypt](prod-le/) (look at the README first)
    - [prod with your certificates](prod-ssl/) (look at the README first)

## Use in your other services

In the projects using docker-compose you want to reverse-proxy, add those labels and the network to the service(s) that
have to be exposed:

```
services:
  your_app:
    [...]
    labels:
      traefik.enable: "true"
      traefik.frontend.rule: "Host: your_app.${DOMAIN_NAME:-local}, www.your_app.${DOMAIN_NAME:-local}"
      traefik.docker.network: "proxytanet"
    networks:
      - proxytanet
      - default
```

You also need to declare the `proxytanet` docker network as external in the same file:

```
networks:
  proxytanet:
    external: true
```

:warning: Don't forget to setup the DNS for `service.${DOMAIN_NAME:-local}` and its `www.` version :warning:

For this, in dev, you can just add it to your `/etc/hosts`

## Go to Dev from Prod and vice versa

Just launch `docker-compose down` in the old folder and follow the README in the new. You don't even need to touch your
other services.
