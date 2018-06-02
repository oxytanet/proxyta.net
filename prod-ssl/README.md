# Traefik SSL

Put your certificate in:

- `/etc/ssl/certs/traefik-ca.crt`
- `/etc/ssl/certs/traefik.crt`
- `/etc/ssl/private/traefik.key`

```
docker network create traefik
docker-compose up -d
```

And then, for all your docker-compose projets, on the services you want to
expose, put:

```yaml
    labels:
      - "traefik.enable=true"
      - "traefik.frontend.rule=Host: ${SERVICE}.${DOMAIN_NAME:-local}, www.${SERVICE}.${DOMAIN_NAME:-local}"
      - "traefik.docker.network=traefik"
```
