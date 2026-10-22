# Neon Obsidian Portfolio

**A database-backed developer portfolio with a terminal-inspired React interface.**

A full-stack portfolio experiment by Srun Sochettra, combining a dark cyan-accented frontend with a Java REST API and PostgreSQL. Profile, project and skill records come from the database; the contact form stores visitor messages through the backend.

This React/Vite and Spring Boot project is separate from the Next.js portfolio and the egg-themed portfolio. No current live deployment is claimed here.

![Original portfolio cover](https://github.com/user-attachments/assets/55bc2a27-be87-45cb-86f5-b925fba6acf1)

The existing cover link is retained; its contents and availability were not checked during this refresh.

## What is implemented

- A terminal-style hero with typewriter text and pointer-driven tilt.
- About, grouped skills and ordered project sections populated through API requests.
- Project technology badges with a backend converter for several stored text/JSON formats.
- A contact form with pending/error states and database persistence.
- Server-side contact validation and structured responses for invalid fields or malformed JSON.
- Backend controller, service and converter tests, plus build-oriented CI configuration.

Project cards link to the contact section rather than individual live demos. Contact success means the API saved a record, not that an email was delivered or read. There is no supplied message inbox, admin editor or notification service.

## Architecture

```text
React + TypeScript + Vite
          |
     HTTP / JSON
          |
Spring MVC controllers -> services -> JPA repositories
                                      |
                                  PostgreSQL
```

| Layer | Source configuration |
| --- | --- |
| Frontend | React ^19.2.0, Vite ^7.3.1, TypeScript ~5.9.3 |
| Styling | Tailwind CSS ^4.2.1, Framer Motion ^12.35.0 |
| Backend | Java 21, Spring Boot 3.2.4, JPA and Jakarta Validation |
| API documentation | Springdoc 2.5.0 dependency and controller annotations |
| Local database | PostgreSQL 15 Alpine in Docker Compose |

These are declared versions, not a freshly installed or tested environment. The old PostgreSQL 16 badge did not match the supplied Compose image.

## Local setup

The following is a suggested sequence derived from the supplied configuration, not an executed installation. Use Java 21, Maven, Docker Compose and Node.js compatible with the frontend dependencies. No Maven wrapper or npm lockfile is included in this export.

### 1. Start the local database

From the repository root:

```sh
docker compose up -d
```

The supplied Compose file starts PostgreSQL only, exposes port 5432 and uses development credentials. It does not start the frontend/backend. Keep it local and do not reuse its credentials for a public deployment.

### 2. Configure and start the backend

In PowerShell, from the repository root:

```powershell
$env:DATABASE_URL = 'jdbc:postgresql://localhost:5432/portfolio'
$env:DATABASE_USERNAME = 'postgres'
$env:DATABASE_PASSWORD = 'postgres'
$env:ALLOWED_ORIGINS = 'http://localhost:5173'
Set-Location backend
mvn spring-boot:run
```

The values above match the local Compose example only. Use private environment configuration for other databases. Spring reads process environment variables; merely copying the root .env.example to .env does not supply them to this Maven command.

Startup enables SQL initialisation and Hibernate schema updates. Use an isolated database: schema.sql creates tables and data.sql inserts sample profile/skill/project rows. Existing matching IDs are left unchanged. This is not a versioned migration or a safe production schema-change plan.

### 3. Start the frontend

In another PowerShell terminal, from the repository root:

```powershell
Set-Location frontend
Set-Content -Path .env.local -Value 'VITE_API_URL=http://localhost:8080'
npm install
npm run dev
```

Set-Content replaces that file; edit an existing .env.local instead if it contains other settings. Vite reads configuration from its frontend project directory. The API client also defaults to the local port-8080 backend when the variable is absent.

Use the address printed by Vite. If it differs from the allowed origin, review ALLOWED_ORIGINS. A root environment example is a reference, not an automatic loader for both processes.

## API surface

| Method | Route | Behaviour |
| --- | --- | --- |
| GET | /api/profile | Profile ID 1, or 404 |
| GET | /api/projects | Projects ordered by sort then ID |
| GET | /api/skills | Skills ordered by sort then ID |
| POST | /api/contact_messages | Validated message persistence; returns 201 on save |

Contact fields require a non-blank name of at least two characters, an email-shaped value and a non-blank message of at least ten characters. No maximum field lengths or application-level rate limiter are supplied in these paths. See [SECURITY.md](SECURITY.md) before public deployment.

## Checks

From backend/:

```sh
mvn test
mvn clean package
```

From frontend/:

```sh
npm run typecheck
npm run lint
npm run build
```

The source contains 22 backend test methods across four test classes. These are test definitions, not fresh passing results. They do not verify PostgreSQL integration, email delivery or browser behaviour. No frontend test script is supplied.

CI builds the backend with tests skipped and builds the frontend without separate lint/typecheck steps. Its npm ci step also depends on a lockfile not included in this export. A workflow file is not proof of a successful run.

## Before publishing

- **Review recording scripts:** frontend/index.html includes interaction/session recording, a third-party page-view request and cross-window message handlers. This documentation does not remove them or establish consent, masking or safe origin handling.
- **Review content:** seed records and hard-coded UI text contain older academic/project descriptions. Skill percentages are authored display values, not measured proficiency.
- **Check failure states:** initial fetches do not check response status before parsing. API failures can produce empty sections or invalid profile data rather than a useful retry state.
- **Verify database mapping:** JSONB conversion and sample data need a real PostgreSQL round-trip test. A project-ID-specific fallback can mask missing technology data.
- **Check accessibility:** the hero has partial reduced-motion handling, but canvas noise, project tilt and other animation paths are not globally disabled. No accessibility-conformance claim is made.

Read [implementation notes](docs/implementation-notes.md) for the bounded source review. The Vercel config and backend Dockerfile are deployment scaffolding, not evidence of a current deployment or end-to-end release verification.

## Contributing and licence

See [CONTRIBUTING.md](CONTRIBUTING.md). Source is distributed under the existing [MIT License](LICENSE), copyright 2026 Srun Sochettra. No source, configuration or licence changes are included in this documentation overlay.
