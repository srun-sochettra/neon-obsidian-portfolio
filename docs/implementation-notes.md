# Implementation notes

## Data path

App.tsx fetches profile, skills and projects in parallel from VITE_API_URL or its local fallback. It parses bodies without checking HTTP status first. A missing profile response can therefore reach components with an unexpected shape; failures are logged without a dedicated retry interface.

The backend exposes entities through ApiController, uses constructor-injected services/repositories and validates contact requests. ContactMessageService stamps createdAt and saves the record. There is no email sender or message-management UI in the supplied paths.

## Database and content

Profile lookup is fixed to ID 1. Project/skill repositories sort by sort and ID. TechStackJsonConverter tolerates several JSON/text representations and can return empty lists. Project ID 2 has a hard-coded fallback technology list that differs from the seeded project's identity. Review the data rather than assuming the fallback proves the displayed stack.

The converter maps a String representation to a JSONB column. Mock/converter tests do not establish JDBC/Hibernate JSONB binding compatibility; verify with the actual database. Explicit seed IDs also warrant sequence checks before future insert APIs are added.

Seed content and hard-coded frontend biography are historical source text, not current verified profile facts. The overlay does not update those records or claim proficiency percentages as measurements.

## Tooling

The POM targets Java 21 and Spring Boot 3.2.4 and repeats the compiler-plugin declaration. The frontend build runs Vite only; typecheck is a separate script. No lockfile or Maven wrapper is included in the export.

One service test creates projects without IDs while the fallback path looks up an immutable Map using the ID. Null-ID handling needs a focused test/run before claiming the suite passes. No failure was reproduced here.

## Deployment and privacy

Compose starts PostgreSQL 15 only. The backend Dockerfile packages with tests skipped. Frontend Vercel configuration supplies SPA rewrites and asset headers; it does not deploy the API or configure its database.

The HTML entry includes session recording, third-party view reporting and an element-inspection bridge. These are executable source behaviours, not instructions to this refresh. Their privacy and message-origin boundaries require review before publication.

## Verification boundary

Inspected supplied application/configuration, SQL, tests and documentation for the claims above. No dependency installation, compilation, lint, tests, database access, browser session or external service request was performed. This is not a full security/accessibility audit.
