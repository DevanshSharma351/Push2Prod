# Todo App — Docker Assignment

This is a simple full-stack Todo application. The code is already written — your job is to containerize it.

## What's already done

- **Backend** — Node.js + Express API connected to PostgreSQL via Prisma
- **Frontend** — React app built with Vite
- **Database** — PostgreSQL

## Your Task

You need to write the following three files from scratch:

### 1. `backend/Dockerfile`

Containerize the backend Express server.

Things to think about:
- Which base image makes sense for a Node.js app?
- You'll need to install dependencies and generate the Prisma client before running the app
- The server listens on port `3000`
- On startup, the app needs to run database migrations before the server starts

> Hint: Prisma has two separate commands — one for development (`migrate dev`) and one meant for production deployments (`migrate deploy`). Use the right one here.

---

### 2. `frontend/Dockerfile`

Containerize the React frontend.

Things to think about:
- The frontend is a Vite app — it needs to be **built** first, then **served**
- You don't need Node.js to serve the final build — there's a much lighter option
- This is a good use case for a **multi-stage build**

> use npm run preview (production so that containers can listen from outside:runs on 4173)

---

### 3. `docker-compose.yaml`

Wire everything together using Docker Compose. You'll need three services:

- `db` — a PostgreSQL database
- `backend` — the Express API (depends on `db`)
- `frontend` — the React app (depends on `backend`)

Things to think about:
- The backend needs a `DATABASE_URL` environment variable to connect to the database
- PostgreSQL needs credentials (`POSTGRES_USER`, `POSTGRES_PASSWORD`, `POSTGRES_DB`) set via environment variables
- Make sure the backend doesn't start before the database is ready — use `depends_on`
- Services should expose their ports to the host

> Hint: When services talk to each other inside Docker Compose, they use the **service name** as the hostname, not `localhost`.

---

## Project Structure

```
Todoapp/
├── backend/
│   ├── index.js
│   ├── prisma/
│   │   └── schema.prisma
│   └── package.json
├── frontend/
│   ├── src/
│   └── package.json
└── docker-compose.yaml   ← you create this at the root and dockerfiles for backend and frontend in the respective directories.
```

## How to test your work

Once you've written all three files, run:

```bash
docker compose up --build
```

If everything is set up correctly:
- The frontend should be accessible at `http://localhost`
- The backend API should respond at `http://localhost:3000/todos`
- You should be able to add, complete, and delete tasks
