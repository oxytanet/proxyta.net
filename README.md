# Traefik Prod

```
docker network create traefik
export ACME_EMAIL=your@email.address
touch acme.json
chmod 600 acme.json
docker-compose up -d
```

And then, for all your docker-compose projets, on the services you want to
expose, put:

```yaml
    labels:
      - "traefik.enable=true"
      - "traefik.frontend.rule=Host: ${SERVICE}.${DOMAIN_NAME:-local}"
```
