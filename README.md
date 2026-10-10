# Project 50+10 - Dance School

Web platform for managing a dance school. It allows students and their guardians to view the schedule and request coaching lessons, teachers to manage their availability and requests, and administrators to monitor the school's activities.

## Features

- Sign-up, login, and password recovery.
- User management with JWT token authentication and role-based permissions.
- Viewing and management of schedules, academic years, dance styles, rooms, and teachers.
- Coaching lesson requests and management, including teacher availability.
- Schedule viewing, costume/inventory management, billing, and auditing.
- Administration dashboard.

## Technologies

- **Frontend:** React, React Router, and Vite.
- **Backend:** Node.js, Express, and JavaScript.
- **Database:** Microsoft SQL Server, accessed through the `mssql` package.
- **Backend tests:** Vitest and Supertest.

## Project structure

- `frontend/` — React web application.
- `backend/` — REST API, authentication, and business logic.
- `backend/sql/dancaF.sql` — SQL script for creating the database schema.

## Prerequisites

- Node.js and npm.
- An accessible Microsoft SQL Server instance.
- Database credentials and, if CAPTCHA validation is enabled, the reCAPTCHA secret key.

## Setup

### 1. Database

Create a database in SQL Server and run the `backend/sql/dancaF.sql` script against it to create the tables. Make sure the instance and database are accessible from the computer where the backend will run.

### 2. Backend

Create the `backend/.env` file with the following settings:

```env
PORT=3000
DB_SERVER=localhost
# Optional, if the instance uses an explicit TCP port:
# DB_PORT=1433
DB_NAME=database_name
DB_USER=username
DB_PASSWORD=password
JWT_SECRET=a_strong_secret_key
# Optional, required for operations that validate reCAPTCHA:
RECAPTCHA_SECRET_KEY=recaptcha_secret_key
```

`DB_SERVER` can also specify a named SQL Server instance, for example `localhost\SQLEXPRESS`. If you set `DB_PORT`, the connection uses that port instead of the instance name.

Install the dependencies and start the server:

```bash
cd backend
npm install
npm run dev
```

The backend starts at `http://localhost:3000`. The API only starts after connecting to the database. The `GET /api/health` endpoint can be used to check the status of the server and database connection.

To start without development mode, use `npm start`.

### 3. Frontend

In a second terminal window:

```bash
cd frontend
npm install
npm run dev
```

Vite displays the application's local address in the terminal. By default, the frontend uses the API at `https://api-vivadarte.onrender.com`. To point it to the local backend, create `frontend/.env.local` with:

```env
VITE_API_URL=http://localhost:3000
```

Restart the Vite server after changing environment variables.

## Useful commands

Run the backend test suite:

```bash
cd backend
npm test
```

Run the tests once, without watch mode:

```bash
npm run test:run
```

Create a production build of the frontend:

```bash
cd frontend
npm run build
```

Preview the built version locally:

```bash
npm run preview
```

Generate JSDoc documentation for the backend controllers, models, and routes:

```bash
cd backend
npm run docs
```

## API

The API is organized under the `/api` prefix and includes the following modules:

- `/api/auth` — authentication and account management.
- `/api/escola` — school structure and schedules.
- `/api/coaching` — coaching requests, lessons, and availability.
- `/api/agenda` — schedule.
- `/api/inventario` — inventory and costumes.
- `/api/auditoria` — audit records.
- `/api` — billing operations.

Most routes require authentication; some operations are restricted to specific roles. See the files in `backend/src/routes/` for available endpoints and permissions.
