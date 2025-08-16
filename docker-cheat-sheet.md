🐳 Docker Compose Cheat Sheet
📂 Project Structure
~/docker/
├── portainer/docker-compose.yml
├── netdata/docker-compose.yml
└── freshrss/docker-compose.yml
Run with:

cd freshrss && docker compose up -d
🔑 Environment Variables
.env

MYSQL_ROOT_PASSWORD=supersecurepassword
MYSQL_DATABASE=appdb
docker-compose.yml

environment:
  MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
  MYSQL_DATABASE: ${MYSQL_DATABASE}
🏷 Labels
labels:
  - "traefik.enable=true"
  - "environment=production"
Filter:

docker ps --filter "label=environment=production"
❤️ Health Checks
healthcheck:
  test: ["CMD", "curl", "-f", "http://localhost:8080"]
  interval: 30s
  retries: 3
🌐 Networks
networks:
  frontend:
  backend:

services:
  web:
    networks: [frontend]
  db:
    networks: [backend]
Macvlan LAN example:

networks:
  lan:
    driver: macvlan
    driver_opts:
      parent: eth0
    ipam:
      config:
        - subnet: "192.168.1.0/24"
          gateway: "192.168.1.1"
⚡ Profiles
services:
  db:
    image: mysql
    profiles: ["dev"]
Run:

docker compose --profile dev up -d
🔄 Restart Policies
restart: unless-stopped
🗂 Version Control
git init
git add docker-compose.yml .env
git commit -m "Initial commit"
📦 External Volumes
volumes:
  shared-data:
    external: true
services:
  app:
    volumes:
      - shared-data:/data
Create:

docker volume create shared-data
📑 Multi-File Config
docker-compose.yml

services:
  app:
    image: myapp:latest
    ports: ["80:80"]
docker-compose.override.yml

services:
  app:
    image: myapp:dev
    ports: ["8080:80"]
    volumes: ["./src:/app/src"]
Run:

docker compose -f docker-compose.yml -f docker-compose.override.yml up -d
🔔 Auto Updates (Watchtower)
services:
  watchtower:
    image: containrrr/watchtower
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    command: --cleanup --interval 3600
✅ Best Practices Recap

Use .env files for secrets

Add labels for automation & organization

Always include health checks

Separate with custom networks

Use profiles for optional services

Apply restart policies

Track with Git

Use external volumes for shared data

Split configs with override files

Enable automatic updates