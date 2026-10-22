# Contributing

Keep changes focused on this React/Vite and Spring Boot portfolio. Do not assume it shares runtime architecture or deployment configuration with another portfolio repository.

## Branch naming convention

Use a focused branch such as feature/contact-feedback, fix/profile-errors, docs/setup or test/converter. Conventional Commits with frontend, backend, db or docs scopes are useful for describing the affected area. These are collaboration conventions, not claims of enforced branch protection.

## Development and verification

Follow the README with an isolated PostgreSQL database and synthetic contact records. Review startup SQL before connecting to existing data.

Run backend tests/package and frontend typecheck/lint/build as relevant. Report exact commands, results and missing checks. Current CI skips backend tests and does not run separate frontend lint/typecheck; do not equate CI configuration with complete verification.

For database changes, test actual PostgreSQL JSONB reads/writes, schema compatibility and existing rows. For contact changes, test malformed/invalid input, oversized payloads, duplicate submissions, persistence failure and safe error feedback. A mocked repository test does not prove durable storage.

For UI changes, test failed API responses, missing profile, empty lists, long text, keyboard focus, narrow layouts and reduced motion. Review recording/telemetry scripts before collecting visitor interactions or using real contact data.

## Pull requests

Use the existing template, describing the problem, focused change, checks actually run, data/privacy impact and recovery approach. Keep unrelated dependency upgrades, generated files and historical biography edits separate. Follow [SECURITY.md](SECURITY.md) for sensitive reports.
