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
    ## Edit these volumes based on your binds. I personally added a docker appdata folder to my home directory in Ubuntu
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
    ## Edit these volumes based on your binds. I personally added a docker appdata folder to my home directory in Ubuntu
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
    ## Edit these volumes based on your binds. I personally added a docker appdata folder to my home directory in Ubuntu
    volumes:
      - /home/$user/docker/appdata/heimdall/:/config
    ports:
      - 180:80
      - 1443:443
    restart: unless-stopped
```
## Dashy
Docker Compose File
```yaml
---
version: "3.8"
services:
  dashy:
    container_name: Dashy
    image: lissy93/dashy:latest
    # Edit these volumes based on your binds. I personally added a docker appdata folder to my home directory in Ubuntu
    volumes:
      - /home/$user/docker/appdata/dashy/:/app/public/
      - /home/$user/docker/appdata/dashy/item-icons:/app/public/item-icons
    ports:
      - 4000:80
    environment:
      - NODE_ENV=production
      - UID=1000
      - GID=1000

    restart: unless-stopped

    # Configure healthchecks
    healthcheck:
      test: ['CMD', 'node', '/app/services/healthcheck']
      interval: 1m30s
      timeout: 10s
      retries: 3
      start_period: 40s
```
Default Sample conf.yml file
```yaml
---
pageInfo:
  title: Home Lab
sections: # An array of sections
- name: Section 1 - Getting Started
  items: # An array of items
  - title: GitHub
    description: Source code and documentation on GitHub
    icon: fab fa-github
    url: https://github.com/Lissy93/dashy
  - title: Issues
    description: View currently open issues, or raise a new one
    icon: fas fa-bug
    url: https://github.com/Lissy93/dashy/issues
  - title: Demo
    description: A live demo
    icon: far fa-rocket
    url: https://dashy-demo-1.netlify.app
- name: Section 2 - Local Services
  items:
  - title: Firewall
    icon: favicon
    url: http://192.168.1.1/
  - title: Game Server
    icon: https://i.ibb.co/710B3Yc/space-invader-x256.png
    url: http://192.168.130.1/
    ```