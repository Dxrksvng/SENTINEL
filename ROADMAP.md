# SENTINEL — Roadmap

> Honest, dated, revisable. Targets are real intent, not promises. Kill criteria are real — building the wrong thing for too long is a worse outcome than admitting it.
>
> Phase contents are kept high-level here. Detailed deliverables and design notes live in private planning docs.

---

## North Star

- **Year 5 (mid-2031)** — SENTINEL is a recognised observability + governance layer for AI agents in regulated industries.
- **Year 1 (mid-2027)** — repeatable sales motion, profitable on a $0 paid-acquisition basis, content and community as the primary go-to-market.
- **Month 6 (end of Oct 2026)** — 3–5 design partners running an end-to-end demo. Aligns with the first substantive enforcement window of the EU AI Act, which creates natural urgency for the buyer.

These are intent. They will move when reality moves. Honesty over optimism.

---

## Phase 0 — Foundation — DONE

**Goal**: scaffolding and discipline so future work lands in the right place.

- Standalone repository, license, security/safety/contributing docs.
- State docs (this file, [ARCHITECTURE](./ARCHITECTURE.md), [PROGRESS](./PROGRESS.md), [VISION](./VISION.md)).
- Workspace layout for the future codebase.
- CI on every push: lint, types, tests, dependency and secret scans.
- Validation toolkit for customer discovery (private).

---

## Phase 1 — MVP — ENGINEERING SUBSTANTIALLY CLOSED

**Goal**: a working end-to-end demo on a single laptop. Not production-grade. Not hosted.

**Pre-condition**: a meaningful number of customer-discovery conversations completed. The interviews shape what the MVP demonstrates — not the founder's prior assumptions.

Phase 1 covers: a customer-side tracing adapter; a control plane that ingests, evaluates policy, and stores telemetry; an operator surface that surfaces what the agent did and why; an audit-ready evidence export. A starter policy pack mapped to public regulatory frameworks ships with the repo, and a Thai-and-English narration runbook is in place for the first design-partner recording.

**Definition of done**: a short screen-recording where a real agent in a sample workload hits a policy violation, the operator surface surfaces it, and the evidence export captures it cleanly. Recording is sent to design partners for feedback. The recording is the gate that closes this phase; the engineering threads under it have landed and the smoke runs end-to-end on a single laptop.

---

## Phase 2 — Beta

**Goal**: 3–5 design partners running SENTINEL against real production traffic.

Phase 2 covers: a production-grade telemetry backend; multi-agent operator views; the additional enforcement modes beyond observe; authentication and authorization; the first compliance pack focused on the deepest founder-expertise jurisdiction; a self-host deployment path; first public landing page and domain.

**Definition of done**: one or more design partners has SENTINEL running against at least one real production agent for at least 30 days, and has used the evidence export at least once for a compliance review.

---

## Phase 3 — Revenue

**Goal**: paying customers and a repeatable sales motion.

Phase 3 covers: billing; a hosted tier; the second compliance pack (EU AI Act-aligned); first public talks; first hire; first investor conversations if and only if the unit economics support raising rather than bootstrapping.

**Definition of done**: a small but real recurring-revenue milestone; a handful of paying customers; one recognisable customer willing to be a public reference.

---

## Phase 4 — Scale

**Goal**: a real team and a real revenue run rate.

Phase 4 covers: geographic expansion (SEA first, then EU); vertical packs for the highest-regulation industries; partnerships with notified bodies for EU AI Act conformity assessment support; SOC 2 Type II and ISO 27001.

---

## Kill criteria

We stop or pivot if any of these is true at the named milestone. Killing the wrong project early frees the founder for the right one.

| When | Kill if |
|---|---|
| End of customer-discovery window | Fewer than 3 interviews surfaced the same specific pain |
| End of Phase 1 | No design partner agreed to install the demo on real traffic |
| End of Phase 2 | Fewer than 1 design partner using SENTINEL ≥30 days against real traffic |
| End of Phase 3 | Recurring revenue meaningfully below target AND no clear path to it |
| Anytime | A major incumbent ships a comparable product as a free add-on AND has wider distribution that cannot be matched within available capital |

These are real — written down on purpose so the founder cannot quietly move the goalposts later.
