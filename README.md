# PoC

Once simple service that runs in docker container and uses keycloak to auth requests.

### Prerequisites

Install `docker` and `docker-compose`

Please update dns (`/etc/hosts`) resolution to point to:
```text
127.0.0.1	keycloak
```

### How to run?

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

### Endpoints:

* http://localhost:9080/swagger-ui.html - service swagger
* http://keycloak:8080/ - keycloak


### What under the hood?

TODO...

