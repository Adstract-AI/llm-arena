# LLM Arena Monorepo

## Project Overview
LLM Arena is a platform for comparing and exploring large language models through human interaction.

Users can start a blind battle conversation, receive two anonymous model answer streams, and continue the conversation across multiple turns. The vote is cast on the full conversation transcript. The system stores battles, turns, responses, and votes so model performance can be reviewed over time.

The project also includes a direct chat mode where users can choose a Vezilka model and chat with it normally. This is useful when they want more control and want to try out the Vezilka models outside the blind arena flow.

The project is focused on evaluating Macedonian fine-tuned LLMs alongside broader global providers.

## Clone With Submodules
To clone the full repository together with its submodules, use:

```bash
git clone --recurse-submodules <repository-url>
```

If you already cloned the repository without submodules, run:

```bash
git submodule update --init --recursive
```

## Setup Overview
This root folder is the combined workspace for the full LLM Arena stack.

It brings together:
- the frontend app in `llm-arena-frontend`
- the backend app in `llm-arena-backend`
- the shared root Docker Compose setup for running the whole stack together

The root setup is useful when you want one command that starts:
- frontend
- backend
- postgres

## What Lives Here
- `llm-arena-frontend/`: the React frontend
- `llm-arena-backend/`: the Django backend
- `docker-compose.yml`: the combined stack definition
- `.env`: root Docker Compose values
- `.env.example`: example template for the root Docker Compose overrides

Each app folder also has its own Docker files, app-specific compose file, and README for working on that part in isolation.

## Root Docker Compose
The root [docker-compose.yml](/Users/itonkdong/Work/Fax/INSOK/llm-arena/docker-compose.yml) is the local-build combined stack.

It starts:
- frontend on `http://localhost:5173`
- backend on `http://localhost:8000`
- postgres on `localhost:5432`

The backend uses the deployment-style Dockerfile from `llm-arena-backend`, and the frontend uses the deployment-style Dockerfile from `llm-arena-frontend`.

Use `.env.example` as the template reference for the root compose overrides.

If you want to run the stack from already-pushed Docker images instead of building locally, use [docker-compose.images.yml](/Users/itonkdong/Work/Fax/INSOK/llm-arena/docker-compose.images.yml). It pulls:
- `itonkdong/llm-arena-backend:latest`
- `itonkdong/llm-arena-frontend:latest`

The image-based compose file also includes Nginx as the only public entrypoint. It exposes `80:80` and routes:
- `/` to the frontend container
- `/api/` to the backend container
- `/admin/` to the backend container
- `/static/` and `/media/` to the backend container

In [docker-compose.images.yml](/Users/itonkdong/Work/Fax/INSOK/llm-arena/docker-compose.images.yml), frontend, backend, and Postgres are internal only, and the frontend is configured with `VITE_API_BASE_URL=/api`.

When deploying on a VM or domain, make sure the backend environment allows the public host:
- add the VM IP or domain to `DJANGO_ALLOWED_HOSTS`
- add the public HTTP/HTTPS origin to `CSRF_TRUSTED_ORIGINS`
- update OAuth redirect URIs to use the public domain instead of localhost

## Running The Full Stack
From this root folder:

```bash
docker compose up --build
```

Then open:

```text
http://localhost:5173
```

To run the full stack from the pushed images:

```bash
docker compose -f docker-compose.images.yml up
```

Then open:

```text
http://localhost
```

When you run the root Docker Compose setup, the backend startup flow also seeds the database with the required initial data and creates a default Django superuser with username `admin` and password `admin`.

After the first cold start, you can disable this automatic setup behavior by setting `AUTO_START_SETUP=false`.

## Setup Commands
The backend also includes two useful setup commands for local preparation and reset flows.

`setup_project`
- prepares the backend for normal use
- seeds the required base data
- creates the default admin user when needed

Example:

```bash
cd llm-arena-backend
conda run -n adstract-backend python manage.py setup_project --full-auto
```

`hardreset`
- recreates the working backend database state for a fresh start
- is useful during heavy schema changes or when you want a clean seeded environment again
- runs the setup flow again after resetting

Example:

```bash
cd llm-arena-backend
conda run -n adstract-backend python manage.py hardreset
```

`AUTO_START_SETUP`
- controls whether the backend container automatically runs setup on startup
- when `AUTO_START_SETUP=true`, container startup runs the setup logic automatically
- when `AUTO_START_SETUP=false`, the backend starts without auto-running setup and you can run `setup_project` or `hardreset` manually

Typical usage:
- first local Docker boot: leave `AUTO_START_SETUP=true`
- later boots on an already prepared environment: set `AUTO_START_SETUP=false`

## When To Use This Root Setup
Use the root setup when you want the whole product running together.

Use the per-app folders when you want to work on:
- backend only
- frontend only
- app-specific Docker or admin configuration

## Project Context
This project was developed as part of the Vezilka project under the guidance of Assistant Teachers Ema Pandilova and Dimitar Peshevski.

The student developers are:
- Andrea Stevanoska
- Viktor Kostadinoski
- Gorazd Filipovski

All contributors listed above are from the Faculty of Computer Science and Engineering (FINKI), Skopje.

FINKI also developed, trained, and fine-tuned all Vezilka models used in this broader project context.
