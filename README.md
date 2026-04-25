# Steam Infra

Orchestrates the Steam Games Archive application using Docker Compose. Manages three services: the Nuxt frontend, the FastAPI backend and a PostgreSQL database.

## Services

| Service     | Image                                                                        | Port |
| ----------- | ---------------------------------------------------------------------------- | ---- |
| `postgres`  | postgres:16                                                                  | —    |
| `steam-api` | gitlab.lnu.se:5050/1dv027/student/jk224vx/exercises/steam-api               | 8000 |
| `steam-data`| gitlab.lnu.se:5050/1dv027/student/jk224vx/exercises/steam-data              | 3000 |

All services run on a shared bridge network `steamnet` (`172.20.0.0/24`). `steam-api` has a static IP (`172.20.0.10`) so that Nginx can route traffic to it reliably.

## Prerequisites

- Docker and Docker Compose installed
- Access to the GitLab registry: `sudo docker login gitlab.lnu.se:5050`

## Setup

1. Clone this repository
2. Copy the example env file and fill in the values:
   ```bash
   cp .env.example .env
   ```
3. Start all services:
   ```bash
   sudo docker compose up -d
   ```

## Environment Variables

| Variable                      | Description                              |
| ----------------------------- | ---------------------------------------- |
| `POSTGRES_PASSWORD`           | Password for the PostgreSQL user         |
| `JWT_SECRET`                  | Secret key for signing JWT tokens        |
| `JWT_ALGORITHM`               | Algorithm used for JWT (e.g. HS256)      |
| `ACCESS_TOKEN_EXPIRE_MINUTES` | Token expiry duration in minutes         |
| `GOOGLE_CLIENT_ID`            | Google OAuth client ID                   |
| `GOOGLE_CLIENT_SECRET`        | Google OAuth client secret               |
| `GOOGLE_REDIRECT_URI`         | OAuth redirect URI                       |
| `CORS_ORIGINS`                | Allowed CORS origins for the API         |
| `NUXT_PUBLIC_API_BASE_URL`    | API base URL used by the browser         |
| `NUXT_API_BASE_URL`           | API base URL used by Nuxt server-side    |

## Common Commands

```bash
# Start all services
sudo docker compose up -d

# Pull latest images
sudo docker compose pull

# Restart a single service with the latest image
sudo docker compose pull <service>
sudo docker compose up -d --force-recreate <service>

# View logs
sudo docker compose logs <service> --follow

# Stop a single service
sudo docker compose stop <service>
```
