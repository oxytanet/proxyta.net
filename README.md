# proxyta.net

A simple reverse proxy setup, for both dev & prod, with [docker-compose](https://docs.docker.com/compose/) &
[traefik](https://traefik.io/)

You can read more here: [https://saurel.me/proxyta.net](https://saurel.me/proxyta.net)

## Launch

1. Create a docker network: `docker network create --ipv6 --subnet=fd00:dead:beef::/48 web`
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
      traefik.http.routers.your_app.rule: "Host(`your_app.${CHATONS_DOMAIN:-localhost}`) || Host(`www.your_app.${CHATONS_DOMAIN:-localhost}`)"
      traefik.http.routers.your_app.entrypoints: "websecure"
    networks:
      - web
      - default
```

You also need to declare the `web` docker network as external in the same file:

```
networks:
  web:
    external: true
```

:warning: In production, don't forget to setup the DNS for `service.${DOMAIN_NAME}` and its `www.` version :warning:

## Go to Dev from Prod and vice versa

Just launch `docker-compose down` in the old folder and follow the README in the new. You don't even need to touch your
other services.
