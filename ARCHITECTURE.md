# SENTINEL — Architecture

> Public overview. Implementation details, schemas, and internal interfaces live in the private repository.
>
> This page describes **what the system is** and **the constraints we design under** — not how it is built.

---

## Component map

```
┌──────────────────┐
│ Customer AI app  │   (LangChain / LangGraph / raw provider SDKs / agent frameworks)
│  ┌────────────┐  │
│  │ Tracing    │  │   ← lightweight library
│  │ adapter    │  │     emits standards-based telemetry
│  └─────┬──────┘  │
└────────┼─────────┘
         │  (open standard transport)
         ▼
┌─────────────────────────────────────────────┐
│  Control plane                              │
│   - ingest                                  │
│   - policy evaluation                       │
│   - evidence pipeline                       │
└──────┬──────────────────────────────────────┘
       │
       ▼
┌─────────────────────────────────────────────┐
│  Operator surface                           │
│   - agent overview                          │
│   - policy management                       │
│   - audit-ready exports                     │
└─────────────────────────────────────────────┘
```

This diagram is intentionally coarse. SENTINEL has three layers — a tracing adapter on the customer side, a control plane that ingests and reasons about behaviour, and an operator surface that humans use to manage policy and produce evidence.

---

## Decided (public-knowledge choices)

### Languages & frameworks

- Backend: Python (typed, async).
- Customer-facing tracing adapter: Python first; other languages added once a customer needs them.
- Operator surface: TypeScript on a modern React framework.
- Strict typing, lint, and format gates run in CI on every push.

### Observability standard

- We follow **OpenTelemetry GenAI semantic conventions** as our baseline. Building on an open standard means customers are not locked into our backend, and exports remain portable.
- We add a small set of compliance-relevant attributes on top of that baseline. The exact attribute set is part of the implementation and is intentionally not published here.

### Repository & licensing

- AGPL-3.0 license.
- Monorepo internally; this public repo is a curated subset (see [Repository Scope](./README.md#repository-scope) in the README).
- Conventional Commits, standalone git repo.

---

## Open (deliberately undecided)

These decisions are **kept open on purpose** until customer reality makes the answer obvious. Locking in too early is a common founder mistake.

- **Trace storage backend** — analytics-oriented vs operationally-simple. Decided after the first paying customers reveal real ingest patterns.
- **Policy enforcement posture** — passive observation first; more active interventions added in an order driven by what design partners actually need. False-positive cost is non-trivial for any active mode, so the sequencing is deliberate.
- **Multi-tenancy posture** — single-tenant deploys vs hosted multi-tenant. Single-tenant is more likely first because early enterprise buyers prefer it.
- **Authentication surface** — deferred until the operator surface stabilises.

---

## Non-goals (for clarity)

These are intentional NOT-features. Saying "no" loudly is part of positioning.

- **Not a model gateway.** We do not proxy LLM calls. Customers stay on their existing provider clients; we observe.
- **Not a fine-tuning or training tool.** We watch production behaviour; we do not modify models.
- **Not a general SOC2/ISO compliance platform.** Vanta and Drata own that. We are AI-specific.
- **Not a prompt-management product.** That is a different category.
- **Not zero-overhead by default.** Inline enforcement modes have measurable cost. We will publish the overhead numbers when the implementation stabilises.

---

## Design principles

What we design under, not how:

1. **Composability over capture.** Plug in next to existing input/output filters and capability evaluators — do not try to replace them.
2. **Open standards on the wire.** Customers can leave. Lock-in is earned by usefulness, not by proprietary formats.
3. **Calibrated claims.** No "AI alignment solved." No "jailbreak detection." We do observability, policy, and evidence — well.
4. **Auditor-shaped output.** Evidence has to map cleanly to the frameworks compliance officers already report against (NIST AI RMF, ISO/IEC 42001, EU AI Act, PDPA).
5. **Self-hostable from day one for sensitive customers.** Hosted is a convenience tier, not the only option.

---

## Threat surface (high-level)

Full threat modelling lives with the implementation. Publicly we name the surfaces we take seriously:

- Customer telemetry confidentiality.
- Policy-rule integrity.
- Ingest abuse.
- Operator-surface web-application risk.
- Tracing-adapter supply-chain integrity.

Each surface has a corresponding mitigation strategy in the private design docs and is reviewed before any release that crosses a trust boundary.
