# Security and privacy guidance

This portfolio has a public contact-write endpoint and browser recording/telemetry code. It is not security-audited. No supported-version programme, reporting deadline or production-readiness guarantee is established here.

## Recording and cross-window messages

frontend/index.html loads an external session-recording library, stores interaction data in browser storage, posts page-view metadata to a third-party endpoint and implements message-based recording export. The handlers do not check message origins in the supplied code and use wildcard reply targets.

Review or remove these scripts before public deployment. Verify consent, masking, origin restrictions, third-party data handling and contact-form exposure. This documentation does not establish that input data is safely masked or that recordings are transmitted only to authorised parties. No exploit or browser recording test was performed.

## Contact records

The endpoint validates minimum field lengths and email syntax, but binds the request directly to a JPA entity, including its ID field. Review server-controlled fields, maximum lengths, request limits, spam/rate controls and safe responses before exposing it broadly. CORS is not an authentication or anti-abuse mechanism.

Messages contain names, email addresses and free text stored in PostgreSQL. No retention/deletion workflow, moderation interface or email delivery service is supplied. Avoid sensitive messages and use synthetic data during testing.

## Configuration and schema

Compose credentials are local examples, not production secrets. Keep real connection details in private environment configuration. VITE-prefixed values are browser configuration and must not contain database credentials.

Startup SQL initialisation and Hibernate update can change the configured database. Review permissions, backups and migration strategy separately; no recovery mechanism is provided by this guide.

## Reporting

Use a verified private maintainer channel if available, or request one without posting sensitive details. No dedicated security mailbox or enabled private-reporting feature is confirmed by the export. Redact contact records, tokens, recordings and connection strings from reports.

These are static source observations and review priorities, not a complete vulnerability audit or implemented fixes.
