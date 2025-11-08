<div>
  <h1>Wanderlust</h1>
  <h2>The Ultimate Travel Blog 🌍✈️ for You </h2>
</div>

![Preview Image](https://github.com/krishnaacharyaa/wanderlust/assets/116620586/17ba9da6-225f-481d-87c0-5d5a010a9538)

# Wanderlust — Docker Compose Stack (MongoDB + Redis + Backend + Frontend + Nginx)

This repository runs the **Wanderlust** app using Docker Compose:

- **MongoDB 7** (database)
- **Redis 7** (cache/queues)
- **Backend** (Node.js, port `3000`)
- **Frontend** (Vite dev server, port `5173`)
- **Nginx** (reverse proxy on **port 80**, optional **443** for future TLS)

Nginx proxies:
- `/` → **frontend:5173**
- `/api` → **backend:3000** (prefix **stripped**, so `/api/users` → backend `/users`)

---
---
📁 Project Structure
wanderlust/
├── backend/
│   ├── Dockerfile
│   ├── package.json
│   ├── .env
│   └── src/
├── frontend/
│   ├── Dockerfile
│   ├── package.json
│   ├── .env
│   └── src/
├── docker-compose.yml    ← Main Docker configuration
├── nginx.conf           ← Nginx reverse proxy config
├── .gitignore
└── README.md

## Prerequisites

- **Docker** 24+ and **Docker Compose** v2+
  ```bash
  docker --version
  docker compose version
  ```
- A Linux host / VM (e.g., EC2) with ports **80** (and **443** if/when you enable TLS) open in security groups / firewall.

---

## Repository Layout

```
.
├─ backend/
│  ├─ Dockerfile
│  └─ ... your backend code
├─ frontend/
│  ├─ Dockerfile
│  └─ ... your frontend code (Vite)
├─ nginx.conf                # Mounted into the Nginx container
└─ docker-compose.yml
```

---


```

-
## Quickstart

```bash
docker compose up -d --build
docker compose exec nginx nginx -t
docker compose logs -f nginx
```

Visit: `http://<EC2-IP>/` 

---

