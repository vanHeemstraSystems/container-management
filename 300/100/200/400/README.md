# 400 - Installing Scope using Docker Compose

To install Scope on your local Docker machine using Docker Compose, use the following docker-compose.yml file:

```
version: '2'
services:
  scope:
    image: weaveworks/scope:1.13.2
    network_mode: "host"
    pid: "host"
    privileged: true
    labels:
      - "works.weave.role=system"
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock:rw"
    command:
      - "--probe.docker=true"
```
containers/app/docker-compose.dev.yml

In addition have following Dockerfile.dev

```
FROM
...
```
containers/app/scope/Dockerfile.dev

Run docker compose as follows:

```
...
```

Scope needs to be installed onto every machine that you want to monitor. Once launched, Scope doesn’t require any other configuration and it also doesn’t depend on Weave Net.
