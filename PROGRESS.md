# SENTINEL — Progress Tracker

> Public progress log. Each session leaves a dated entry. Cadence is real; details are deliberately kept high-level — the implementation lives in a private repository.
>
> **Why this file exists publicly**: anyone visiting this repo should be able to verify that SENTINEL is being built — not pitched.

---

## TL;DR — current state

- **Date last updated**: 2026-05-05
- **Phase**: `1` — MVP wire-up. Read path through the full stack is in place; enforcement is next.
- **Active workstreams**: customer discovery (in parallel) + technical end-to-end demo.
- **Blocking decisions**: none.
- **Next focus**: hooking policy decisions into the ingest path; first audit-ready evidence export.

## Quick links

- [README](./README.md) — what SENTINEL is and why
- [VISION](./VISION.md) — the long view
- [ROADMAP](./ROADMAP.md) — phase-by-phase plan
- [ARCHITECTURE](./ARCHITECTURE.md) — public architecture overview
- [SAFETY](./SAFETY.md) — safety stance and limits we put on ourselves

---

## Phase tracker

Status legend: `[ ]` not started · `[~]` in progress · `[x]` done · `[!]` blocked · `[-]` deferred

### Phase 0 — Foundation

- [x] Standalone repository, license, security/contributing/safety docs
- [x] State docs (this file, ROADMAP, ARCHITECTURE, VISION)
- [x] Workspace layout for the future codebase
- [x] Continuous integration: lint, types, tests on every push
- [x] Continuous security: dependency and secret scans

### Phase 1 — MVP

- [x] Tracing adapter — emits standards-based telemetry from a customer agent
- [x] Tracing adapter — one-call setup helper for new integrations
- [x] Control-plane ingest — receive and persist agent telemetry
- [x] Control-plane read path — list observed agents and recent activity
- [x] Operator surface — first page rendering live data from the control plane
- [x] Policy schema and validator — refuses malformed rules at load time
- [ ] Policy decisions on ingest — observe mode first
- [ ] Evidence export — bundle telemetry + decisions for an agent over a window

### Phase 1 — MVP follow-ups (customer-driven)

- [ ] Attribute mappings for the agent frameworks customers actually use
- [ ] Choice of long-term telemetry store (deferred until ingest patterns are real)
- [ ] Enforcement modes beyond observe — masking, refuse, escalate
- [ ] Article-level mapping packs for compliance frameworks (waits for ≥5 customer interviews)

### Phase 2 — Beta

- [ ] First design partners onboarded under a private agreement
- [ ] First end-to-end demo from agent to evidence
- [ ] Public landing page
- [ ] Domain registration

### Phase 3 — Revenue

- [ ] Billing
- [ ] First paying customer
- [ ] First small revenue milestone
- [ ] First hire

(See [ROADMAP.md](./ROADMAP.md) for detail.)

---

## Decisions log

Architectural and product decisions, with reasoning. Reversibility tagged so future-me knows the cost of changing course.

| Date | Decision | Reasoning | Reversible? |
|---|---|---|---|
| 2026-05-03 | License: AGPL-3.0 | Open-core posture: protect against straight commercial fork while keeping community trust | Easy |
| 2026-05-03 | Public repo is documentation-only during pre-alpha | Prevents premature copy of incomplete safeguards; implementation matures in private | Easy |
| 2026-05-03 | Languages: Python (backend + adapter) + TypeScript (operator surface) | Matches founder's stack; ecosystem fit | Hard |
| 2026-05-03 | Observability built on OpenTelemetry GenAI conventions | Industry-standard, vendor-neutral, portable for customers | Hard |
| 2026-05-03 | Build first, deploy never (this phase) | No customer = no need for cloud spend; avoid premature ops decisions | n/a |

---

## Open questions

Tracked here so they get answered, not forgotten. Each one is gated on a real signal, not founder guesswork.

1. Long-term telemetry store — decide once ingest volume from real customers is measurable.
2. Pricing tiers — wait for budget signals from interviews before locking numbers.
3. Geographic focus order — Thailand/SEA first, or EU first. Wait for demand signal from interviews.
4. Self-host vs hosted — enterprise tier likely demands self-host. Decide post-design-partner #1.
5. Brand/domain — do not register before problem validation completes.

---

## Session log

One entry per working session. The point is cadence, not detail. Detail belongs in private design notes.

```
### Session N — YYYY-MM-DD — <short tag>
- Outcome:
  - …
- Verified:
  - … (CI green, manual smoke, etc.)
- Skipped (and why):
  - …
- Hand-off for next session:
  - …
```

---

### Session 1 — 2026-05-03 — foundation

- **Outcome**:
  - Standalone repository created with license, security, contributing, and safety docs.
  - Public state docs (this file, ROADMAP, ARCHITECTURE) initialised.
  - Workspace layout established for the future codebase.
  - CI configured for lint, types, and tests on every push.
- **Verified**: CI workflows ran green on the initial commit.
- **Skipped**: domain registration, public landing page, paid infrastructure — all gated on problem validation.
- **Hand-off**: customer-discovery work proceeds in parallel; code work begins in Session 2.

### Session 2 — 2026-05-04 — tracing adapter wired to control plane

- **Outcome**:
  - Customer-side tracing adapter now emits standards-based telemetry.
  - One-call setup helper added so adopting the adapter is friction-light.
  - End-to-end smoke from a sample agent through to persisted telemetry passed.
- **Verified**: tracing adapter and control-plane test suites both green; manual end-to-end smoke confirmed.
- **Skipped**: control-plane read endpoints — bundled into the next session so the operator surface gets useful data in one cut.
- **Hand-off**: implement read path so the operator surface has something live to render.

### Session 3 — 2026-05-04 — read path through the full stack

- **Outcome**:
  - Control plane now exposes a paginated listing and a derived agent-overview view.
  - Operator surface gained its first live page, served against real telemetry from the control plane.
  - Empty, error, and populated states all rendered correctly.
- **Verified**: backend test suite expanded and green; operator surface type-checks clean and builds successfully.
- **Skipped**: in-browser click-through smoke — flagged as a follow-up before the next external demo.
- **Hand-off**: hook policy decisions into the ingest path next; observe mode first to keep blast radius small.
