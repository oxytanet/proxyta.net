# Traefik Dev

```
docker network create traefik
docker-compose up -d
```

And then, for all your docker-compose projet, on the service you want to
expose, put:

```yaml
    labels:
      - "traefik.backend=${SERVICE}"
      - "traefik.frontend.rule=Host:${SERVICE}.${DOMAIN_NAME:-local}"
      - "traefik.enable=true"
```
