# PoC

Once simple service that runs in docker container and uses keycloak to auth requests.


### Prerequisites

Install `docker` and `docker-compose`

Please update dns (`/etc/hosts`) resolution to point to:
```text
127.0.0.1	keycloak
```

### How to run?

**Note:** Please first start postgres (`docker-compose up postgres-keycloak`). Postgres requires to create db volume 
files, so this process may take time.
When started first time together with keycloak, database can be not ready to accept connections. And during first
starts **keycloak may fail to start together** at one time with **postgres**.

Assume we want to start all services all together.
- just run all services
```bash
docker-compose up -d
```

- just run all services and force docker-compose to recreate containers (clear cache).
Useful if you have new versions of images available /
or want to use locally-assembled images.
```bash
docker-compose up -d --force-recreate
```

To run UI project, build and run from [here](https://bitbucket.org/aliaksandr_filipau/keycloak-angular-auth)


### Endpoints:

* http://localhost:9080/ - service welcome page
* http://keycloak:8080/ - keycloak


### What under the hood?

3 services:
* nocraft server resides [here](https://github.com/Dmitry-itechart/nocraft-server).
* keycloack taken from [here](https://hub.docker.com/r/jboss/keycloak/)
* we use keycloack with postgres db together, so postgres taken from [here](https://hub.docker.com/_/postgres)

First we need to start keycloak. To make keycloak persisted, we use postgres db as data store. Postges data itself is
 stored in mounted volume.
 
 During initial start we import pre-created 'Demo' realm to keycloack. Pay attention that we assume that `nocraft
 ` server is configured to use this default 'Demo' realm.
 
 This can be configured with nocraft server's environment properties. Pay attention to this configuration part:
 ```json
nocraft:
    environment:
      - keycloak.auth-server-url=http://keycloak:8080/auth
```

Also pay attention that we use internal 'docker-compose' network called 
```json
networks:
  keycloak-network:
```
This way inside this network keycloak will be resolved by it's server name automatically.

When operating from browser - which is on the host system, we will get redirect to `keycloak:8080`. This should be
 resolved to localhost since keycloak exposes `8080` port here and all services by default accessible from `127.0.0.1`
