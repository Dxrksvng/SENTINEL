# SENTINEL — Progress Tracker

> Public progress log. Each session leaves a dated entry. Cadence is real; details are deliberately kept high-level — the implementation lives in a private repository.
>
> **Why this file exists publicly**: anyone visiting this repo should be able to verify that SENTINEL is being built — not pitched.

---

## TL;DR — current state

- **Date last updated**: 2026-05-06
- **Phase**: `1` — MVP wire-up. End-to-end read path **and** audit evidence export are in place; the first external demo is the next gate.
- **Active workstreams**: customer discovery (in parallel) + technical end-to-end demo.
- **Blocking decisions**: none.
- **Next focus**: surface decisions on the operator surface, ship a starter policy pack, and record the first end-to-end demo against a real sample agent.

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
- [x] Policy decisions on ingest — observe mode first
- [x] Evidence export — bundle telemetry + decisions for an agent over a window

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

### Session 4 — 2026-05-06 — long-horizon research backlog separated from current scope

- **Outcome**:
  - Published `docs/FUTURE_AGI_RESEARCH.md` — a research backlog of frontier / agentic-AI behaviours we want the data plane to be ready to carry signal for, without claiming detection capability today.
  - Document explicitly defers detection to research-grade upstream work (Inspect AI, METR, frontier labs) and reinforces SENTINEL's Layer-2 positioning.
  - Cross-linked from the README documentation map so the public scope stays navigable.
- **Verified**: cross-links to `SAFETY.md` and `ROADMAP.md` resolve; tone matches the "calibrated claims" posture in `SAFETY.md` (no overclaim of detection).
- **Skipped**: any change to roadmap or product copy. The research backlog is intentionally separate from the build plan.
- **Hand-off**: resume Phase 1 work — policy decisions on the ingest path (observe mode), then first evidence export bundle.

### Session 5 — 2026-05-06 — policy decisions on ingest (observe mode)

- **Outcome**:
  - Ingest path now evaluates the active policy set against every incoming span and persists matched policies as Decision records alongside the spans.
  - Observe-mode contract is explicit: even rules whose declared action would block or escalate produce only a record; ingest behaviour is unchanged. Buggy policies fail open with a log line.
  - The ingest response surface adds a count of matched decisions; a paginated, filterable read endpoint exposes the decision history for the operator surface and for the upcoming evidence export.
- **Verified**: full backend test suite green and expanded; lint and strict type-checks clean.
- **Skipped**: surfacing decisions on the operator surface and adding a starter policy pack — both kept out of this session to keep the change focused on the ingest contract. They are the natural follow-ups.
- **Hand-off**: build the first audit-ready evidence export — bundle spans + decisions for an agent over a time window into a structure an external auditor can consume.

### Session 6 — 2026-05-06 — first audit-ready evidence export

- **Outcome**:
  - The control plane now generates a self-contained, time-windowed evidence bundle for a single agent, joining the spans observed in the window with the decisions that reference them.
  - The bundle is identified by a versioned format string and stamped with a content hash an external auditor can recompute to verify integrity. Window semantics are explicit and half-open, so two adjacent windows neither overlap nor leave gaps.
  - Agent-level metadata in the bundle is derived from the most recent in-window span — handy when an agent's version or framework changed mid-window.
- **Verified**: full backend suite green (now 48 tests); lint and strict type-checks clean.
- **Skipped**: persisted bundles, signed download URLs, and async generation. Phase 1 returns the bundle inline; the heavier pipeline waits for real customer scale.
- **Hand-off**: surface decisions on the operator surface, ship a starter policy pack, and record the first end-to-end demo against a sample agent — the natural next step before the first external design-partner conversation.
