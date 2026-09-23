# QTurn

A full-stack medical appointment management system with role-based experiences for administrators, doctors, and patients. QTurn combines an Expo/React Native mobile client with a Spring Boot REST API, JWT authentication, and PostgreSQL persistence.

<p align="center">
  <img src="front/qturn/assets/images/logo.png" alt="QTurn logo" width="180" />
</p>

## What it demonstrates

- Mobile development with React Native, Expo Router, and TypeScript
- REST API design using Spring Boot and layered architecture
- Stateless authentication with Spring Security and JWT
- Relational persistence with PostgreSQL, JPA, and composite keys
- Role-oriented workflows for `ADMIN`, `DOCTOR`, and `PATIENT`
- Reproducible local infrastructure with Docker Compose
- Secure mobile token storage through Expo SecureStore

## Implemented features

### Patients

- Sign in and manage profile data
- Browse available appointment times
- Create, view, reschedule, and cancel appointments

### Doctors

- Review a daily appointment agenda
- Configure and manage work schedules
- Access appointment and patient details

### Administrators

- List, create, edit, and remove users
- Manage role-based user records from the mobile interface

### Platform

- JWT-based authentication with one-hour token expiration
- Protected API routes and centralized authentication errors
- PostgreSQL-backed users, schedules, and appointments
- Light/dark theme support in the mobile application

## Architecture

```text
Expo / React Native app
          |
          | HTTPS + JSON + Bearer JWT
          v
Spring Boot REST API
          |
          | Spring Data JPA
          v
      PostgreSQL
```

The repository is organized as a monorepo:

```text
.
├── back/qturn/       # Java 17 + Spring Boot API
├── front/qturn/      # Expo + React Native mobile client
├── compose.yml       # PostgreSQL and API services
└── .env.example      # Local Docker configuration template
```

## Technology stack

| Area | Technologies |
| --- | --- |
| Mobile | React Native, Expo, Expo Router, TypeScript |
| Backend | Java 17, Spring Boot 3, Spring Security, Spring Data JPA |
| Authentication | JWT, BCrypt, Expo SecureStore |
| Database | PostgreSQL |
| Mapping/tooling | MapStruct, Lombok, Maven |
| Infrastructure | Docker, Docker Compose |
| Testing | JUnit, Spring Boot Test, Jest, React Test Renderer |

## Run locally

### Requirements

- Docker Engine with Docker Compose
- Node.js 20+ and npm
- Expo Go, an Android/iOS emulator, or a web browser

### 1. Start PostgreSQL and the API

```bash
cp .env.example .env
```

Change `POSTGRES_PASSWORD` and set `JWT_SECRET` to a random value of at least 32 characters, then run:

```bash
docker compose up --build
```

The API will be available at `http://localhost:8080`. Its health endpoint is `GET /actuator/health`.

### 2. Start the mobile client

```bash
cd front/qturn
cp .env.example .env
npm ci
npm start
```

Set `EXPO_PUBLIC_API_URL` according to the runtime:

- Web/iOS simulator: `http://localhost:8080`
- Android emulator: `http://10.0.2.2:8080`
- Physical device: the development computer's LAN address, for example `http://192.168.1.20:8080`

The device and API host must be reachable from the same network when using Expo Go.

## Backend without Docker

```bash
cd back/qturn
export DB_URL=jdbc:postgresql://localhost:5432/qturn_db
export DB_USERNAME=qturn
export DB_PASSWORD=your-password
export JWT_SECRET=replace-with-at-least-32-random-characters
./mvnw spring-boot:run
```

Additional configuration is available through `SERVER_PORT`, `JPA_DDL_AUTO`, and `JPA_SHOW_SQL`.

## Main API routes

All routes except login and health checks require `Authorization: Bearer <token>`.

| Method | Route | Purpose |
| --- | --- | --- |
| `POST` | `/login` | Authenticate and issue a JWT |
| `POST` | `/register` | Register a user from an authenticated workflow |
| `GET/PUT/DELETE` | `/users/...` | Manage user records |
| `POST` | `/appointments` | Create an appointment |
| `GET` | `/appointments/available-times` | List available time slots |
| `GET` | `/appointments/doctor/{doctorId}/appointments` | Read a doctor's daily agenda |
| `PUT/DELETE` | `/appointments/{appointmentId}` | Reschedule or cancel an appointment |
| `GET/POST/PUT/DELETE` | `/work-schedules/...` | Manage doctor schedules |

## Validation

```bash
# Backend
cd back/qturn
JWT_SECRET=development-test-secret-at-least-32-characters ./mvnw test

# Mobile client
cd front/qturn
npm ci
npx tsc --noEmit
npm test
```

## Project status

QTurn is a functional academic full-stack project and portfolio case study. The core user, authentication, appointment, and schedule flows are implemented. Before production use, the next priorities are:

- enforce resource ownership and role authorization at method level;
- expand integration and end-to-end test coverage;
- add database migrations instead of automatic schema updates;
- complete waiting-list, appointment-history, and notification modules;
- add observability, rate limiting, and a production deployment profile.

The current source tree contains no production credentials or demo passwords. If this project was previously shared with local credentials, rotate them before reuse.

## Author

Developed by **Emiliano Muñoz** as an advanced Software Engineering academic project.
