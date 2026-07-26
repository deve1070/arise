# Arise -Progress Tracking App

Track growth. Build responsibility. Connect tutors, parents, and children.

---

## Tech Stack

| Layer    | Technology           |
| -------- | -------------------- |
| Frontend | React +              |
| Backend  | NestJS + TypeORM     |
| Database | PostgreSQL 15        |
| Cach     | Redis                |
| Dev env  | Docker, GitLab CI/CD |
| Mobile   | Flutter              |

---

## Getting Started

### Prerequisties

- Docker Desktop installed and running
- Git

### 1. Clone and enter the project directory

```bash
git clone https://github.com/arise
cd arise
```

### 2. Copy the environment file

```bash
cp env.example .env
```

### 3. Start everything

```bash
docker compose up --build
```

This starts:

- PostgreSQL database on port 5432
- Redis cache on port 6379
- NestJS server on http://localhost:3001
- React frontend on http://localhost:5173

### 4. Open the app

Go to http://localhost:5173

---

## Project Structure

```
arise/
├── backend/
|   ├── src/
|   |   ├── auth/
|   |   ├── users/
|   |   └── common/
|   |       ├── decorators/
|   |       ├── guards/
|   |       └── filters/
|   ├── Dockerfile.dev/
|   ├── nest-cli.json/
|   ├── package.json/
|   └── tsconfig.json
|
├── Frontend/
|   ├── src/
|   |   ├── api/
|   |  ├── components/auth/
|   |  ├── pages/
|   |  |   ├── auth/
|   |  |   ├── tutor/
|   |  |   ├── parent/
|   |  |   └── child/
|   |  ├── store/
|   |  └── types/
|   ├── Dockerfile.dev/
|   ├── package.json/
|   ├── vite.config.ts/
|   └──tailwind.config.js/
├── docker-compose.yml
├── .env
└──README.md
```

---

## API Endpoints

| Method | Route | Auth | Description |
| POST | /api/auth/register | Public | Create account(any role) |
| POST | /api/auth/login | Public | Loign,return JWT token |
| GET | /api/auth/me | JWT | Get current user profile |

### Example: Register a tutor

```bash
curl -X POST http://localhost:3001/api/auth/register \
    -H "Content-Type: application/json" \
    -d '{
        "email": "tutor@example.com",
        "fullName": "John Mekonnen",
        "role": "TUTOR",
        "password":"password123"
    }'
```

```bash
curl -X POST http://localhost:3001/api/auth/login \
    -H "Content-Type: application/json" \
    -d '{
        "email": "tutor@example.com",
        "password":"password123"
        }'
```

# Example: Get current User

```bash
curl http://localhost:3001/api/auth/me \
    -H "Authorization: Bearer YOUR_JWT_TOKEN"
```

---

## User Roles

| Role | Description | Dashboard route |
| Turor | Creates ladders,logs sessions | /tutor/dashboard |
| PARENT | Views summaries,sends encouragment | /parent/dashboard |
| CHILD | Tracks own progress, earns badges | /child/dashboard |

---

## Development Notes

- `synchronize: true` is set in TypeORM for dev - the DB schema updates automatically.
  Switch to migrations before going to production.
- The `.env` file has placehoder secrets. Change `JWT_SECRET` and `POSTGRES_PASSWORD` with your own values.before any deployment.

-Hot reload is enabled for both backend(NestJS watch mode) and fronted (Vite HMR).
any file change rebuilds automatically without restarting Docker.
