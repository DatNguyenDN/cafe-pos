# Cafe POS — NestJS + React Repository Skeleton

> Repo goal: Full-stack skeleton ready to `git clone` and run locally (NestJS backend with TypeORM + PostgreSQL, React frontend). Contains MenuModule for CRUD and basic Auth scaffold (JWT). Includes Docker Compose for PostgreSQL + backend + frontend dev convenience.

---

## 1) File tree (what you will get)

```
cafe-pos-nestjs/
├─ backend/
│  ├─ .env.example
│  ├─ Dockerfile
│  ├─ package.json
│  ├─ tsconfig.json
│  ├─ ormconfig.ts
│  ├─ src/
│  │  ├─ main.ts
│  │  ├─ app.module.ts
│  │  ├─ common/
│  │  │  ├─ guards/jwt-auth.guard.ts
│  │  │  └─ decorators/user.decorator.ts
│  │  ├─ auth/
│  │  │  ├─ auth.module.ts
│  │  │  ├─ auth.service.ts
│  │  │  ├─ jwt.strategy.ts
│  │  │  └─ dto/login.dto.ts
│  │  ├─ users/
│  │  │  ├─ users.module.ts
│  │  │  ├─ users.service.ts
│  │  │  └─ user.entity.ts
│  │  ├─ menu/
│  │  │  ├─ menu.module.ts
│  │  │  ├─ menu.controller.ts
│  │  │  ├─ menu.service.ts
│  │  │  └─ menu-item.entity.ts
│  │  ├─ database/
│  │  │  └─ seeds/seed.ts
│  │  └─ config/
│  │     └─ configuration.ts
│  └─ migrations/
├─ frontend/
│  ├─ package.json
│  ├─ vite.config.ts
│  ├─ src/
│  │  ├─ main.jsx
│  │  ├─ App.jsx
│  │  ├─ pages/Login.jsx
│  │  ├─ pages/AdminMenu.jsx
│  │  └─ services/api.js
├─ docker-compose.yml
├─ README.md
```

---

## 2) High-level features included

* NestJS (TypeScript) backend using TypeORM + PostgreSQL
* JWT Auth scaffold (login endpoint, JWT generation)
* `MenuModule` with CRUD endpoints: `GET /menu`, `POST /menu`, `PUT /menu/:id`, `DELETE /menu/:id`
* Database seeder to create initial admin user + sample menu items
* React + Vite frontend with simple login and Admin Menu UI (CRUD)
* Docker Compose to run `postgres`, backend, and optionally frontend

---

## 3) Backend — important files (you'll find full code in this repo)

* `.env.example`

  * `DATABASE_HOST=postgres`
  * `DATABASE_PORT=5432`
  * `DATABASE_USER=postgres`
  * `DATABASE_PASS=postgres`
  * `DATABASE_NAME=cafe_pos_dev`
  * `JWT_SECRET=your_jwt_secret`

* `src/main.ts` — bootstrap Nest app with global prefix `/api` and validation pipe.

* `src/app.module.ts` — imports TypeOrmModule with `ormconfig.ts`, AuthModule, UsersModule, MenuModule.

* `src/menu/menu-item.entity.ts` — TypeORM entity for menu items (id, name, price, category, available)

* `src/menu/menu.service.ts` — CRUD logic

* `src/menu/menu.controller.ts` — REST endpoints

* `src/auth` — JWT strategy & login DTO; `auth.service` validates user and issues JWT

* `src/database/seeds/seed.ts` — simple script to create admin user (email: [admin@cafe.local](mailto:admin@cafe.local) / password: password) and sample menu items

---

## 4) Frontend — important files

* `src/services/api.js` — axios instance with baseURL `http://localhost:4000/api`
* `src/pages/Login.jsx` — form performs POST `/api/auth/login`, saves token to localStorage
* `src/pages/AdminMenu.jsx` — fetch `GET /api/menu`, allows Create / Edit / Delete using modal forms

---

## 5) Docker Compose (development)

`docker-compose.yml` spins up:

* `postgres` (volume mounted)
* `backend` (build from ./backend, environment from .env)

You can run:

```bash
# start postgres + backend
docker compose up --build
```

The backend will connect to Postgres, run migrations (TypeORM `synchronize: true` in dev), and seed initial data.

---

## 6) How to run locally (no Docker)

### Prerequisites

* Node.js 18+
* PostgreSQL running locally

### Backend

```bash
cd backend
cp .env.example .env    # edit if needed
npm install
npm run build
npm run seed            # optional: run seeder script
npm run start:dev       # starts nestjs in watch mode (port 4000)
```

### Frontend

```bash
cd frontend
npm install
npm run dev
# open http://localhost:5173
```

Default admin credentials (from seeder):

* email: `admin@cafe.local`
* password: `password`

---

## 7) How menu changes work for the owner (non-dev workflow)

1. Owner logs into Admin web UI (AdminMenu page).
2. Owner uses Create / Edit / Delete forms to change menu items.
3. Frontend calls backend REST endpoints which update the database.
4. Staff devices (POS web or mobile) call `GET /api/menu` to fetch fresh menu.

**Make it realtime:** optionally add a WebSocket gateway in NestJS (using `@WebSocketGateway()` or `socket.io`) so when owner updates menu, server emits `menu:updated` and connected staff clients refresh automatically.

**Bulk changes:** Add import/export CSV to Admin UI so owner can modify menu in spreadsheet and upload.

**History & audit:** Add `MenuHistory` entity to log price changes, who changed it, and when.

---

## 8) Next steps I can do for you (choose any)

* A. Create the full repo locally and provide a ZIP that you can extract and `git init`/`git remote add`/push to GitHub. (I will prepare code in the canvas for you.)
* B. I can write the exact NestJS source files & React files and paste them into the repo (you can copy-paste). (I will prepare code in the canvas.)
* C. Setup CI + GitHub Actions for linting/tests and Docker build.

Tell me which option (A/B/C). After you choose, I will place the files in the canvas document so you can copy them or download the ZIP.

---

*If you need me to push directly to your GitHub, I will provide instructions for you to run locally (I can't push using your credentials).*
