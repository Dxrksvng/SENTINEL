# Security Policy

## Reporting a vulnerability

Until SENTINEL has a formal security mailbox set up, report vulnerabilities by emailing the maintainer directly. Do **not** open a public GitHub issue for security-sensitive problems.

We will acknowledge within 72 hours and aim to provide a status update within 7 days.

## Scope

This is a pre-MVP project. Once SENTINEL has paying customers, this policy will be revised to reflect a formal coordinated-disclosure timeline (typically 90 days) and a public security advisory channel.

## What we ask of contributors

- Never commit secrets, API keys, or credentials. Use `.env.example` for templates.
- Pre-commit: run `gitleaks detect` (or equivalent) before pushing.
- Treat any data flowing through SENTINEL as potentially containing PII — apply principle of least privilege.
- LLM prompt-injection is in-scope: any retrieved or user-provided content must be treated as untrusted input within the policy engine.

## Known limitations (pre-MVP)

- No authentication on the API yet — do not deploy publicly.
- No encryption at rest configured for trace storage yet.
- No tenant isolation logic.

These are tracked in [`PROGRESS.md`](./PROGRESS.md) and [`ROADMAP.md`](./ROADMAP.md). They will be addressed before any customer-facing deployment.
