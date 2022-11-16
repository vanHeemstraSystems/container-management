# 400 - Installing Scope using Docker Compose

To install Scope on your local Docker machine using Docker Compose, use the following docker-compose.yml file:

```
version: "3.7"

secrets:
  database_password:
    file: ./secrets/.database_password
  proxy_password:
    file: ./secrets/.proxy_password
    
# See https://stackoverflow.com/questions/29261811/use-docker-compose-env-variable-in-dockerbuild-file
services:

  scope:
    secrets:
      - proxy_password
    build:
      context: ./scope
      dockerfile: Dockerfile.dev
      args: # from env_file
        UNIQUE_NAMESPACE: ${UNIQUE_NAMESPACE}
        IMAGE_REPOSITORY: ${IMAGE_REPOSITORY}
        PROXY_USER: ${PROXY_USER}
        PROXY_PASSWORD: ${PROXY_PASSWORD}
        PROXY_FQDN: ${PROXY_FQDN}
        PROXY_PORT: ${PROXY_PORT}
    env_file:
      - .env
    container_name: ${UNIQUE_NAMESPACE}-scope-dev
    security_opt:
      - no-new-privileges:true
    privileged: true
    network_mode: "host"
    pid: "host"    
    labels:
      - "works.weave.role=system"
    ports:
      - "4039:4040"
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock:rw"
    command:
      - "--probe.docker=true"
```
containers/app/docker-compose.dev.yml

In addition have following Dockerfile.dev

```
ARG IMAGE_REPOSITORY
# pull official base image, see https://hub.docker.com/r/zhicwu/prom/
FROM ${IMAGE_REPOSITORY}/weaveworks/scope:1.13.2

# See https://stackoverflow.com/questions/29261811/use-docker-compose-env-variable-in-dockerbuild-file
ARG PROXY_USER
ARG PROXY_PASSWORD
ARG PROXY_FQDN
ARG PROXY_PORT

ENV HTTP_PROXY="http://${PROXY_USER}:${PROXY_PASSWORD}@${PROXY_FQDN}:${PROXY_PORT}"
ENV HTTPS_PROXY="http://${PROXY_USER}:${PROXY_PASSWORD}@${PROXY_FQDN}:${PROXY_PORT}"

# expose port
EXPOSE 4039
```
containers/app/scope/Dockerfile.dev

Run docker compose as follows:

```
$ cd containers/app
$ docker-compose --file docker-compose.dev.yml --project-name container-management-dev up --build -d
```

Scope needs to be installed onto every machine that you want to monitor. Once launched, Scope doesn’t require any other configuration and it also doesn’t depend on Weave Net.
