# Docker Configuration Files for Containers I use

## NGINX Proxy Manager
Docker Compose File
```yaml
version: '3'
services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    privileged: true
    restart: unless-stopped
    ports:
      - '80:80'
      - '81:81'
      - '443:443'
    volumes:
      - /home/$user/docker/appdata/nginx/data/:/data
      - /home/$user/docker/appdata/nginx/letsencrypt/:/etc/letsencrypt
```

## Gitea
Docker Compose File
```yaml
version: "3"

networks:
  gitea:
    external: false

services:
  server:
    image: gitea/gitea:latest
    container_name: gitea
    environment:
      - USER_UID=1000
      - USER_GID=1000
      - GITEA__database__DB_TYPE=mysql
      - GITEA__database__HOST=db:3306
      - GITEA__database__NAME=gitea
      - GITEA__database__USER=gitea
      - GITEA__database__PASSWD=gitea
    restart: always
    networks:
      - gitea
    volumes:
      - /home/$user/docker/appdata/gitea:/data
      - /etc/timezone:/etc/timezone:ro
      - /etc/localtime:/etc/localtime:ro
    ports:
      - "3000:3000"
      - "222:22"
    depends_on:
      - db

  db:
    image: mysql:8
    restart: always
    environment:
      - MYSQL_ROOT_PASSWORD=gitea
      - MYSQL_USER=gitea
      - MYSQL_PASSWORD=gitea
      - MYSQL_DATABASE=gitea
    networks:
      - gitea
    volumes:
      - ./mysql:/var/lib/mysql
```
## UptimeKuma
Docker Compose File
```yaml
version: '3.8'

services:
  uptime-kuma:
    image: louislam/uptime-kuma:latest
    container_name: uptime-kuma
    volumes:
      - uptime-kuma:/app/data
    ports:
      - "3001:3001"  # <Host Port>:<Container Port>
    restart: always

volumes:
  uptime-kuma:
```
## Heimdall
Docker Compose File
```yaml
---
version: "2.1"
services:
  heimdall:
    image: lscr.io/linuxserver/heimdall:latest
    container_name: heimdall
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
    volumes:
      - /home/$user/docker/appdata/heimdall/:/config
    ports:
      - 180:80
      - 1443:443
    restart: unless-stopped
```