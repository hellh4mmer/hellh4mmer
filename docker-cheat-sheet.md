# 🐳 Docker Compose Cheat Sheet

## 📂 Project Structure
```
~/docker/
├── portainer/docker-compose.yml
├── netdata/docker-compose.yml
└── freshrss/docker-compose.yml
```

Run:
```bash
cd freshrss && docker compose up -d
```

---

## 🔑 Environment Variables

.env
```env
MYSQL_ROOT_PASSWORD=supersecurepassword
MYSQL_DATABASE=appdb
```

docker-compose.yml
```yaml
services:
  db:
    image: mysql:latest
    environment:
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
      MYSQL_DATABASE: ${MYSQL_DATABASE}
```

---

## 🏷 Labels

docker-compose.yml
```yaml
services:
  app:
    labels:
      - "traefik.enable=true"
      - "environment=production"
```

Filter:
```bash
docker ps --filter "label=environment=production"
```

---

## ❤️ Health Checks

```yaml
services:
  app:
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8080"]
      interval: 30s
      retries: 3
```

---

## 🌐 Networks

Basic custom networks:
```yaml
networks:
  frontend:
  backend:

services:
  web:
    image: nginx:alpine
    networks:
      - frontend
  db:
    image: mysql:latest
    networks:
      - backend
```

Macvlan LAN example:
```yaml
networks:
  lan:
    driver: macvlan
    driver_opts:
      parent: eth0
    ipam:
      config:
        - subnet: "192.168.1.0/24"
          gateway: "192.168.1.1"
```

---

## ⚡ Profiles

```yaml
services:
  db:
    image: mysql
    profiles: ["dev"]
```

Run:
```bash
docker compose --profile dev up -d
```

---

## 🔄 Restart Policies

```yaml
services:
  app:
    image: myapp:latest
    restart: unless-stopped
```

---

## 🗂 Version Control

```bash
git init
git add docker-compose.yml .env
git commit -m "Initial commit"
```

---

## 📦 External Volumes

docker-compose.yml
```yaml
volumes:
  shared-data:
    external: true

services:
  app:
    image: myapp:latest
    volumes:
      - shared-data:/data
```

Create:
```bash
docker volume create shared-data
```

---

## 📑 Multi-File Config

docker-compose.yml
```yaml
services:
  app:
    image: myapp:latest
    ports:
      - "80:80"
```

docker-compose.override.yml
```yaml
services:
  app:
    image: myapp:dev
    ports:
      - "8080:80"
    volumes:
      - "./src:/app/src"
```

Run:
```bash
docker compose -f docker-compose.yml -f docker-compose.override.yml up -d
```

---

## 🔔 Auto Updates (Watchtower)

```yaml
services:
  watchtower:
    image: containrrr/watchtower
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    command: --cleanup --interval 3600
```

---

## ✅ Best Practices Recap

- Use .env files for secrets  
- Add labels for automation & organization  
- Always include health checks  
- Separate with custom networks  
- Use profiles for optional services  
- Apply restart policies  
- Track with Git  
- Use external volumes for shared data  
- Split configs with override files  
- Enable automatic updates