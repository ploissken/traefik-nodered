# README
Para criar novos containers docker com instâncias encapsuladas do node-red,
crie uma nova pasta em /var/www/ chamada CONTAINER_NAME. Edite o template
abaixo com o CONTAINER_NAME e SUBDOMAIN (previamente configurado no digital
ocean). Incremente TRAEFIK_PORT e inicie-o com:
```
docker-compose up -d
```

## node-red
mais informações sobre a imagem do docker:
https://hub.docker.com/r/nodered/node-red

### docker-compose template
```
version: "3"
# networks internal to docker
networks:
  traefik:
    external: true
  internal:
    external: false

services:
  CONTAINER_NAME:
    image: nodered/node-red:latest-minimal
    networks:
      - traefik
      - internal
    ports:
      - "1880:TRAEFIK_PORT"
    labels:
      - "traefik.backend=nodered"
      - "traefik.docker.network=traefik"
      - "traefik.enable=true"
      - "traefik.port=TRAEFIK_PORT"
      - "traefik.frontend.rule=Host:SUBDOMAIN.controlartcloud.com.br"
    environment:
      HOST: 0.0.0.0
      TZ: America/Sao_Paulo #specific configuration of nodered container
    volumes:
      - ./:/data # persist files to root folder
    user: "root"
```
