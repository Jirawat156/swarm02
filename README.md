# url (gitea)
- https://jirawatjpv1.xops.ipv9.xyz

# wakatime swarm02
- https://wakatime.com/@spcn24/projects/dwmpsobqdi

### Create stack deploy code compose ใน xOps.ipv9
                  version: '3.3' 
                  services:
  web: 
    image: jirawatjp/gitea:v1
    networks: 
    - webproxy 
    logging:
      driver: json-file
    container_name: jirawatjpv1
    deploy: 
      replicas: 1 
      labels: 
        - traefik.docker.network=webproxy
        - traefik.enable=true
        - traefik.http.routers.jirawatjpv1-https.entrypoints=websecure 
        - traefik.http.routers.jirawatjpv1-https.rule=Host("jirawatjpv1.xops.ipv9.xyz")
        - traefik.http.routers.jirawatjpv1-https.tls.certresolver=default
        - traefik.http.services.jirawatjpv1.loadbalancer.server.port=3000 
      resources: 
        reservations: 
          cpus: '0.1'
          memory: 10M
        limits: 
          cpus: '0.4'
          memory: 160M
networks: 
  webproxy: 
    external: true





## Gitea with PostgreSQL
This example defines one of the base setups for Gitea. More details on how to customize the installation and the compose file can be found in [Gitea documentation](https://docs.gitea.io/en-us/install-with-docker/).


Project structure:
```
.
├── compose.yaml
└── README.md
```

[_compose.yaml_](compose.yaml)
```
services:
  gitea:
    image: gitea/gitea:latest
    ports:
      - 3000:3000
    ...
  db:
    image: postgres:alpine
    environment:
    ...
```

When deploying this setup, docker compose maps the gitea container port 3000 to
the same port of the host as specified in the compose file.

## Deploy with docker compose

```
$ docker compose up -d
Creating network "gitea-postgres_default" with the default driver
Creating gitea-postgres_db_1 ... done
Creating gitea-postgres_gitea_1 ... done
Attaching to gitea-postgres_db_1, gitea-postgres_gitea_1
....
Starting gitea-postgres_db_1 ... done
Starting gitea-postgres_gitea_1 ... done
```


## Expected result

Check containers are running and the port mapping:
```
$ docker ps
CONTAINER ID        IMAGE                COMMAND                  CREATED             STATUS              PORTS                          NAMES
2f5624043da9        gitea/gitea:latest   "/usr/bin/entrypoint…"   56 seconds ago      Up 16 seconds       22/tcp, 0.0.0.0:3000->3000/tcp gitea-postgres_gitea_1
86acc768453e        postgres:alpine      "docker-entrypoint.s…"   57 seconds ago      Up 17 seconds       5432/tcp                       gitea-postgres_db_1
```

Navigate to `http://localhost:3000` in your web browser to access the installed
Gitea service.

![page](output.jpg)

Stop and remove the containers

```
$ docker compose down
```

To remove all Gitea data, delete the named volumes by passing the `-v` parameter:
```
$ docker compose down -v
```

