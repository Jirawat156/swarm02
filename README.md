# Ref
- https://github.com/docker/awesome-compose/tree/master/gitea-postgres

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

![image](https://user-images.githubusercontent.com/119155285/224615182-6e964224-1674-42ec-9b58-446eb300a7dc.png)


