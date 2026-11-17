
# CampusHub - Docker Hands-On

## Overview

CampusHub is a nine-service application orchestrated using Docker Compose.

## Architecture

- Gateway: Nginx reverse proxy and public entry point.
- Frontend: React application built using Vite.
- API: Node.js and Express backend.
- Worker: Processes registration email jobs.
- PostgreSQL: Stores events and registrations.
- Redis: Caches event data.
- RabbitMQ: Message broker.
- MinIO: Object storage for event images.
- MailHog: Captures outgoing emails for testing.

## Requirements

- Docker Desktop
- Docker Compose

## Setup

1. Clone the repository.
2. Copy `.env.example` to `.env`.
3. Configure the environment variables.
4. Build and start the application:

```bash
docker compose up -d --build --wait --wait-timeout 300
```

5. Open the application:

http://localhost:8081

## Useful Commands

Check services:

```bash
docker compose ps
```

View logs:

```bash
docker compose logs -f
```

Stop services:

```bash
docker compose down
```

## Architecture Decisions

### 1. Multi-stage frontend build
Node.js builds the React application, while an unprivileged Nginx image serves static files. This reduces the final image size and avoids including Node.js in production.

### 2. Separate frontend, backend and data networks
Network segmentation limits unnecessary communication between services and improves isolation.

### 3. Persistent named volumes
PostgreSQL and MinIO use named volumes so database records and uploaded images survive container recreation.

## Security

- Services use environment variables for credentials.
- `.env` is excluded from Git.
- Frontend, API, worker and gateway use non-root execution.
- Only the gateway is exposed on public host interfaces.
