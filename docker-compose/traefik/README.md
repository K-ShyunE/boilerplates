
## Traefik 네트워크 생성
#### 이 네트워크를 이용해서 traefik 과 컨테이너 간의 통신
```
docker network create traefik-network
```

## Container 설정
#### 트래픽을 통해서 라우팅 받을 앱
```
# 앱의 docker-compose.yaml
service:
  any_app:
    image: any
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.<service_name>.rule=Host(`<any-domain>`)"
      - "traefik.docker.network=traefik_network" # docker 네트워크를 지정해줘야 traefik 에서 제대로 라우팅을 할수있다.
      - "traefik.http.routers.<service_name>.entrypoints=web,websecure"
      - "traefik.http.routers.<service_name>.tls=true"  # SSL/TLS 사용
    networks:
      - traefik_network

networks:
  traefik_network:
    external: true

```
