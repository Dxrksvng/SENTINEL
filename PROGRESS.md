# SENTINEL — Progress Tracker

> Public progress log. Each session leaves a dated entry. Cadence is real; details are deliberately kept high-level — the implementation lives in a private repository.
>
> **Why this file exists publicly**: anyone visiting this repo should be able to verify that SENTINEL is being built — not pitched.

---

## TL;DR — current state

- **Date last updated**: 2026-05-06
- **Phase**: `1` — MVP wire-up. Engineering thread substantially closed: tracing adapter, ingest, read path, observe-mode policy decisions, audit-ready evidence export, starter policy pack, demo runbook, and an operator-surface decisions view all in place.
- **Active workstreams**: customer discovery (in parallel) + recording the first design-partner demo.
- **Blocking decisions**: none.
- **Next focus**: record the screen capture against the starter pack and send it to the first design partners.

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
- [x] Operator surface — decisions view: severity, action, agent, span, mode

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

### Session 7 — 2026-05-06 — end-to-end smoke against a realistic workload

- **Outcome**:
  - First true end-to-end run: a multi-stage pipeline shaped like a real consulting-AI workload (data profile → AI interpretation → story → slide plan) emits spans through the SDK, the control plane records observe-mode decisions against an installed policy, and the run is sealed with a hash-verified evidence bundle.
  - The LLM is forced to a local provider so the smoke is free to run, repeatable, and does not move customer-shaped data off the laptop. Three back-to-back pipeline invocations completed in roughly twenty seconds and produced fifteen spans, three decisions, and a bundle whose content hash recomputed cleanly.
  - The smoke script doubles as a design-partner demo: a single command, no cloud dependencies, exits zero on success.
- **Verified**: smoke script exits zero against a freshly started control plane; bundle hash recomputation matches the wire value byte-for-byte.
- **Skipped**: provider-level instrumentation that would surface inner LLM calls as their own spans — that is Phase 2 work and outside the current Phase 1 contract.
- **Hand-off**: render decisions on the operator surface, ship a starter policy pack inspired by what the smoke surfaced, and record the smoke as a short screen capture for outreach to the first design partners.

### Session 8 — 2026-05-06 — recording-ready demo + starter policy pack

- **Outcome**:
  - The smoke gained a camera-friendly presentation mode — banners, optional ANSI colour, configurable inter-section pacing — so it doubles as the on-screen content for a 60–90 second design-partner recording.
  - A demo runbook ships alongside it: pre-flight checklist, six chapter cues with Thai and English narration, a positioning guard that names the claims that must NOT be made on camera, and a failure table for aborting cleanly when something goes wrong.
  - A starter policy pack lands too — six declarative observe-mode rules that cover all five EU AI Act risk tiers and one cross-cutting external-LLM-provider rule. Every rule cites the framework article it supports (EU AI Act Articles 5, 12, 14, 50, 51, 53, 55; PDPA Section 28; ISO/IEC 42001; NIST AI RMF; OECD AI Principles), reacts to a single declarative attribute the SDK exposes today, and ships with a helper script that installs the pack into a running control plane.
- **Verified**: starter pack tests added (now 59 backend tests total); the pack loads via the same loader the policies endpoint uses, every rule carries at least one compliance reference, and the helper script installs all six rules end-to-end against a running API.
- **Skipped**: rules that depend on list-valued attributes (e.g. matching against `sentinel.safety.capabilities`) — they need a new policy operator and are deferred. Customer attestation, list-contains semantics, and per-tenant scoping are all still Phase 2 work.
- **Hand-off**: build the operator-surface view of decisions so a non-engineer can read the same evidence the API already returns — the last open Phase-1 thread before the recording goes out to design partners.

### Session 9 — 2026-05-06 — operator-surface decisions view

- **Outcome**:
  - The dashboard now ships a `/decisions` route that renders the same data the API exposes at `/v1/decisions`, with severity-coloured badges, an action column, agent / span / trace identifiers, and the originating policy name and reason. A non-engineer reading the page can answer "which agents tripped which rules and how recently".
  - The page surfaces an explicit "all decisions are observe-mode: recorded, never enforced" banner when every visible decision is in observe mode, so a recorded `BLOCK` is never mistaken for an enforced one. A small filter form supports narrowing by agent, span, or severity, with a Clear affordance and an empty-state hint that points at the starter pack.
  - Navigation links from the home page and the agents page now lead into the decisions view, so the demo can move from "what agents are observed" → "what policies fired" without typing a URL.
- **Verified**: dashboard typecheck and production build both clean; live smoke against a fresh control plane with the starter pack installed produced decisions across all five severity tiers (CRITICAL/HIGH/MEDIUM/LOW/INFO), all six starter rule names rendered, severity-filtered URLs narrowed correctly, and the filtered-empty state appeared for an unknown agent. Backend suite remains green at 59 tests.
- **Skipped**: cursor-driven next-page UI (the next-page cursor is surfaced as a hint but not wired to a Load-more button), span-detail drill-through, and cross-linking from a decision to the originating span. All deferred to Phase 2 once a real customer's volume justifies the UX work.
- **Hand-off**: the engineering threads under Phase 1 have all landed. Next is the recording itself — capture the smoke and the operator surface end-to-end, run it past the demo runbook's positioning guard, and send it to the first design partners.
