# SENTINEL

> **Runtime governance for AI agents in production.**
>
> Observability, runtime policy, and audit-ready evidence for organisations deploying AI agents into regulated environments.

---

| | |
|---|---|
| **Status** | Pre-Alpha · active development · solo founder |
| **Stage** | Phase 1 — MVP wire-up |
| **License** | AGPL-3.0 |
| **Last updated** | 2026-05-06 |
| **Customers** | None yet — currently in problem-validation phase |
| **Funding** | Self-funded |

---

## ⚠️ Repository Scope

This repository contains public documentation, architecture overviews, and development progress for SENTINEL.

Core implementation (runtime engine, policy system, and tracing adapter) is maintained in a private repository during the pre-alpha stage.

This is intentional, to ensure:

- security and responsible disclosure
- controlled rollout of compliance-critical systems
- prevention of misuse before proper safeguards are in place

Code will be progressively open-sourced where appropriate. AGPL-3.0 applies to all source once distributed.

### Current Status

Pre-alpha (private implementation, public planning).

### What's Public

- Architecture overview ([`ARCHITECTURE.md`](./ARCHITECTURE.md))
- Long-term vision ([`VISION.md`](./VISION.md))
- Phase-by-phase roadmap ([`ROADMAP.md`](./ROADMAP.md))
- Session-by-session development progress ([`PROGRESS.md`](./PROGRESS.md))
- Safety stance and self-imposed limits ([`SAFETY.md`](./SAFETY.md))
- Compliance-framework mappings ([`docs/compliance/frameworks.md`](./docs/compliance/frameworks.md))
- Security disclosure policy ([`SECURITY.md`](./SECURITY.md))

### What's Not Public (yet)

- Runtime engine (control-plane source)
- Policy enforcement system
- Tracing-adapter implementation
- Operator-surface implementation
- Internal schemas, attribute definitions, and storage details
- Customer-validation toolkit and design-partner conversations

Each of the items above has a clear gating condition before any of it moves into this repository. None of those conditions is "founder feels like it" — see [`ROADMAP.md`](./ROADMAP.md) for the gates.

---

## What SENTINEL is

SENTINEL sits between AI agents and the people accountable for them.

It does three things, and **only three things**, well:

1. **Observe** — capture what an agent did, in a portable open-standard format.
2. **Govern** — evaluate runtime policy. Observe mode first; masking, refusal, and escalation as customers earn the right to ask for them.
3. **Evidence** — produce audit-ready exports that map cleanly to the frameworks compliance officers already report against.

Everything else is intentionally out of scope. See [`VISION.md`](./VISION.md) for the long view, and [`ARCHITECTURE.md`](./ARCHITECTURE.md) for the public architecture overview.

---

## Why this exists

AI agents are reaching production faster than the governance to oversee them. Three groups feel this acutely:

- **Engineering teams** — instrument AI agents the way they instrument microservices, but the standards are still settling.
- **Compliance and risk officers** — accountable for behaviour they cannot see, with frameworks they must report against.
- **Customers and regulators** — increasingly demand traceability for any consequential AI decision.

Existing observability stacks were built for deterministic services. They do not produce evidence that holds up to an AI-aware audit. SENTINEL is being built to close that specific gap.

---

## Where SENTINEL sits in the AI safety stack

```
Layer 5 — Existential / civilizational governance (treaties, AISIs)
Layer 4 — Frontier capability research (RSPs, Preparedness, FSF)
Layer 3 — Pre-deployment evaluation (Inspect AI, METR, Apollo)
Layer 2 — Production observability + governance              ◄── SENTINEL
Layer 1 — Application-level guardrails (Lakera, LlamaGuard, NeMo)
Layer 0 — Model-level safety (RLHF, Constitutional AI)
```

SENTINEL **composes with** — never replaces — adjacent layers.

---

## Compliance frameworks targeted

| Framework | Coverage area |
|---|---|
| NIST AI Risk Management Framework (AI RMF 1.0) | Govern / Map / Measure / Manage |
| ISO/IEC 42001:2023 — AI Management System | Controls vocabulary |
| EU AI Act (Regulation 2024/1689) | Articles relevant to high-risk and general-purpose AI |
| OWASP LLM Top 10 (v1.1, 2024) | LLM01 – LLM10 |
| MITRE ATLAS (v4) | Adversarial threat tactics |
| OECD AI Principles | Values mapping |
| PDPA Thailand (B.E. 2562) | Sections relevant to automated processing |

Mappings are advisory. Final regulatory determinations rest with your counsel.

A more detailed mapping reference lives in [`docs/compliance/frameworks.md`](./docs/compliance/frameworks.md).

---

## Engineering progress

The cadence is real. The detail is deliberately kept high-level.

- **Phase 0** (foundation) — complete. License, security/safety/contributing docs, workspace layout, CI for lint + types + tests + dependency/secret scans.
- **Phase 1** (MVP wire-up) — engineering thread substantially closed. Tracing adapter wired through to the control plane; first operator-surface page rendering live data; policy schema and validator landed; policy decisions are produced on the ingest path in observe mode; the first audit-ready evidence export is in place with a content hash an external auditor can recompute. A starter policy pack covering the EU AI Act risk tiers and PDPA cross-border references ships with the repo, and a Thai-and-English narration runbook is ready for the first design-partner recording. Next: render decisions on the operator surface and run the first design-partner conversations.

Session-by-session progress is logged in [`PROGRESS.md`](./PROGRESS.md). The full plan lives in [`ROADMAP.md`](./ROADMAP.md).

---

## Honest disclosures

Read this section. It is the difference between SENTINEL and AI-safety theatre.

- We do **not** detect jailbreaks. We support a schema; detection lives in upstream tools that already do it well.
- We do **not** auto-classify legal risk tier. Auto-classifying legal risk is a single point of catastrophic miscategorisation; risk tier is declared by the deploying organisation.
- We do **not** claim AI safety guarantees. Final safety judgment rests with the operator.
- We are **not** a frontier-AI alignment lab. We make production AI governable, not safe in the existential sense.
- We have **no** certifications (no SOC 2, no ISO 27001, no ISO 42001). Those follow customer traction; claiming them pre-attestation would be fraud.
- We have **no** paying customers yet. The product is in customer-validation phase.
- We have **no** investors yet. SENTINEL is self-funded by the founder.

Overclaiming AI safety capability is itself a safety failure — it leads operators to under-invest in real defences. We will not.

---

## Documentation map

| Document | What it answers |
|---|---|
| [`VISION.md`](./VISION.md) | Why this needs to exist; where it is going |
| [`ARCHITECTURE.md`](./ARCHITECTURE.md) | Public architecture overview |
| [`ROADMAP.md`](./ROADMAP.md) | Phase-by-phase plan |
| [`PROGRESS.md`](./PROGRESS.md) | Session-by-session build log |
| [`SAFETY.md`](./SAFETY.md) | Safety stance and self-imposed limits |
| [`SECURITY.md`](./SECURITY.md) | Vulnerability disclosure policy |
| [`CONTRIBUTING.md`](./CONTRIBUTING.md) | How (and when) to contribute |
| [`docs/compliance/frameworks.md`](./docs/compliance/frameworks.md) | Compliance-framework mapping reference |
| [`docs/FUTURE_AGI_RESEARCH.md`](./docs/FUTURE_AGI_RESEARCH.md) | Research backlog: agentic / frontier behaviours the data plane should be ready to carry (not current scope) |

---

## License

AGPL-3.0. See [`LICENSE`](./LICENSE).

---

## Contact

If you are a CISO, AI risk officer, head of compliance, or AI platform lead operating a production AI deployment with a regulatory deadline you are answerable for: I would like to talk.

Open a GitHub Discussion or reach the maintainer through the GitHub profile.

External code contributions are not being accepted yet — focus is on validation and design-partner conversations.
