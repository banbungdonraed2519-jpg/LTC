# LTC บ้านบึง Smart Care Platform — Full System

A lightweight, full-stack Long-Term Care (LTC) management platform for บ้านบึง. This repository is organized as a monorepo containing:

- Backend: Node.js + Express + SQLite (JWT authentication)
- Frontend: React + Vite + Tailwind CSS
- Features: Login, Dashboard, Patient registry, Home Visits, Assessments, AI chat helper, Reports and basic GIS preview

This README documents how to run the project, its structure, API endpoints, default credentials, and deployment guidance.

---

## Features

- User authentication with JWT (admin / nurse seed accounts)
- Patient registry (create, list)
- Home visit records (create, list)
- Clinical assessments (create, list)
- Dashboard summary with quick stats and village ranking
- Simple AI chat endpoint for assistant replies
- Export/Report UI placeholders (PDF / Excel / PowerPoint)
- Local SQLite database with initial seed data

---

## Tech stack

- Backend: Node.js, Express, sqlite3, bcryptjs, jsonwebtoken
- Frontend: React, Vite, Tailwind CSS, Recharts, lucide-react
- Dev / Deployment: Docker, docker-compose

---

## Quick start

Run backend and frontend locally

### Backend

```bash
cd backend
npm install
npm start
```

The backend listens on port 4000 by default. Healthcheck: `GET /health`.

### Frontend

```bash
cd frontend
npm install
npm run dev
```

The frontend dev server (Vite) runs on port 5173 by default and expects an API at the URL in `VITE_API_URL` (default `http://localhost:4000`).

---

## Default accounts

- admin / admin123 (role: admin)
- nurse / nurse123 (role: nurse)

These are seeded automatically when the backend initializes the SQLite database.

---

## API (backend) overview

Base URL: http://localhost:4000/api

Authentication
- POST /api/auth/login
  - Body: { username, password }
  - Returns: { token, user }
- GET /api/auth/me
  - Requires: Authorization: Bearer <token>
  - Returns current user

Dashboard
- GET /api/dashboard/summary
  - Returns totals and breakdowns by village

Patients
- GET /api/patients
  - Returns all patients
- POST /api/patients
  - Body: patient fields (hn, cid, full_name, village, age, gender, adl, risk, caregiver, phone, bp, sugar, weight, note, latitude, longitude)
  - Requires auth

Visits
- GET /api/visits
- POST /api/visits
  - Body: { patient_id, visit_date?, note }
  - Requires auth

Assessments
- GET /api/assessments
- POST /api/assessments
  - Body: { patient_id, type, score, result }
  - Requires auth

AI
- POST /api/ai/chat
  - Body: { message }
  - Returns: { response }

Notes: All protected endpoints require `Authorization: Bearer <JWT>` header.

---

## Database

The backend uses SQLite and stores the database file at `backend/data/ltc.db` when run with the provided configuration. Database initialization and seeding are handled in `src/db.js`.

Seeded tables include `users`, `patients`, `visits`, `assessments`, and `ai_logs`.

---

## Docker

A `Dockerfile` is provided for the backend and `docker-compose.yml` config brings up `backend` and `frontend` services with a bind-mounted `./backend/data` volume so the SQLite database persists.

To run with Docker Compose:

```bash
docker compose up --build
```

This exposes ports 4000 (backend) and 5173 (frontend) on the host.

---

## Project structure (high level)

- backend/
  - src/
    - server.js            # Express app, routes mounting
    - db.js                # SQLite helpers and initialization
    - routes/              # api route modules (auth, patients, visits, assessments, ai, dashboard)
    - middleware/          # authentication middleware
  - data/                  # generated at runtime: ltc.db
  - package.json
  - Dockerfile

- frontend/
  - src/
    - main.jsx             # app bootstrapping
    - App.jsx              # main UI and routing (single-file app)
    - api.js               # fetch helpers for backend
    - styles.css           # Tailwind + custom styles
  - index.html
  - package.json

- docker-compose.yml
- README.md

---

## Security & configuration

- JWT secret: `JWT_SECRET` environment variable (defaults to `ltc-secret-key-dev` in development). For production, set a strong secret.
- API URL for frontend: `VITE_API_URL` environment variable.
- SQLite file is stored in `backend/data/ltc.db` — ensure appropriate backups if running in production.

---

## Development notes & ideas

- Add pagination for large patient lists
- Implement role-based UI controls (admin vs nurse)
- Add automated tests for API endpoints
- Replace in-memory AI stub with an external LLM provider (if required) and add rate-limiting / logging
- Add migrations instead of simple CREATE TABLE if schema evolution is needed

---

## Contributing

1. Fork the repo
2. Create a feature branch: `git checkout -b feature/xxx`
3. Make changes and open a PR

Please include tests and update this README when adding major features.

---

## License

This project does not include an explicit license. Add a LICENSE file if you want to make the project open source.

---

If you want, I can also:
- Add an example .env or environment sample
- Add API docs in OpenAPI (swagger) format
- Create a CONTRIBUTING.md and CODE_OF_CONDUCT
- Open a pull request from `improve-documentation` to the default branch with this README

Tell me which of those you'd like next and I'll proceed.
