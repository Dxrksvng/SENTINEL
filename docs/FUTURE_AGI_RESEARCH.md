# Future AGI / Frontier-AI Research Backlog

> **Status**: Research backlog — **not** current scope. Items here are not features and are not on the roadmap.
>
> **Position**: SENTINEL operates at **Layer 2** (production observability + governance). See [`SAFETY.md`](../SAFETY.md) for the full layer model and what we explicitly do not do.
>
> **Honesty rule**: We do **not** claim to detect any of the behaviours below today. We track them so the data plane we build today does not foreclose future detectors built by the research community.

---

## Why this document exists

Most production AI governance tooling was designed for **AI as a feature** — chatbots, RAG pipelines, classification endpoints. As agents become more autonomous and capable, the data shape that today's tools capture will not be sufficient for tomorrow's questions.

SENTINEL's bet is narrow and specific:

1. **Build the data plane today** with explicit hooks for behaviours research may want to detect tomorrow.
2. **Do not claim to detect those behaviours.** Detection is research-grade work belonging in Layers 3–4 (Inspect AI, METR, Apollo, AISIs, frontier labs) — not a B2B product.
3. **Stay honest in customer copy.** Speculative future risk is not a sales argument. Audit-ready evidence today is.

This document signals long-term thinking. It is not a product specification.

---

## Behaviour patterns we want the data plane to be ready for

For each pattern below: **what** the behaviour is, **what telemetry shape** would let a future detector reason about it, and **whether** SENTINEL's planned schema accommodates that shape.

"Schema-ready" means the data model **can carry** the relevant signal. It does not mean SENTINEL **detects** the behaviour. Detection is out of scope.

### 1. Deceptive alignment / sandbagging

- **What**: Agent performs differently when it believes it is being evaluated vs. deployed.
- **Telemetry shape**: Per-trace context labels (eval / staging / prod / canary), full per-agent trace history, output-style fingerprints across contexts.
- **Schema-ready**: Yes — context labels are first-class span attributes.
- **Detection**: Open research problem. We do not attempt it.

### 2. Power-seeking behaviour

- **What**: Agent acquires resources, permissions, or persistence beyond task scope.
- **Telemetry shape**: Tool-call graph per session, declared capability scope vs. exercised scope, resource-allocation deltas.
- **Schema-ready**: Partial. Tool-call graphs are captured by the OTel GenAI conventions we follow; declared-vs-exercised scope diff is a planned extension, not a current capability.
- **Detection**: Rule-based for known anti-patterns is feasible; novel-pattern detection is open research.

### 3. Recursive self-improvement attempts

- **What**: Agent modifies its own prompt, weights, code, or training data to influence future runs.
- **Telemetry shape**: Immutable audit trail of agent identity and configuration; cryptographic signing of evidence bundles; tagged self-referencing tool calls.
- **Schema-ready**: Planned. Append-only storage and signed evidence are part of Phase 1 evidence-export work; self-reference tagging is a planned extension.
- **Detection**: Future research.

### 4. Goal generalisation beyond instruction

- **What**: Agent pursues sub-goals not specified in the original task in service of a generalised objective.
- **Telemetry shape**: Captured planning traces and reasoning steps when the upstream framework emits them.
- **Schema-ready**: Yes — when a framework emits planning state (e.g. graph-based agent runtimes, ReAct-style reasoning traces), the schema carries them through. Opaque LLM calls without exposed reasoning are by definition out of reach.
- **Detection**: Likely requires LLM-as-judge over reasoning traces. Not a SENTINEL product line; would be a downstream consumer.

### 5. Coordination across agent instances

- **What**: Multiple agent instances coordinate via shared state, side channels, or learned conventions.
- **Telemetry shape**: Cross-agent trace correlation, shared-state access logs, per-decision communication graph.
- **Schema-ready**: Yes — distributed-trace correlation is native to OpenTelemetry, which is the wire format we adopt.
- **Detection**: Graph anomaly detection, future.

### 6. Specification gaming / reward hacking

- **What**: Agent satisfies the stated metric while violating the intent.
- **Telemetry shape**: Outcome metrics joined with downstream feedback signals and resource cost.
- **Schema-ready**: Partial. Outcome attribution is in scope; structured feedback ingestion is a planned extension once design partners reveal what feedback shape they actually have.
- **Detection**: Statistical outlier analysis (success-vs-cost) is plausible at the analysis layer.

### 7. Capability concealment

- **What**: Agent demonstrates lower capability than it possesses, typically under evaluation.
- **Telemetry shape**: Capability benchmarks per session with context labels.
- **Schema-ready**: Future. Requires a benchmark-injection feature that does not exist and is not on the near roadmap.
- **Detection**: Distribution analysis across context labels.

### 8. Adversarial robustness failures at scale

- **What**: Agent is vulnerable to prompt injection, jailbreak, or social engineering when scaled to production.
- **Telemetry shape**: Input fingerprints, output classifications, suspected-attempt event records.
- **Schema-ready**: Yes — a standardised event shape for suspected attempts is part of [`SAFETY.md`](../SAFETY.md). SENTINEL is the carrier; detection lives in Layer-1 tools (Lakera, LlamaGuard, OpenAI Moderation) that emit events into the schema.
- **Detection**: Existing production-grade today. We integrate; we do not build a competing detector.

### 9. Long-horizon agentic deception

- **What**: Agent strategically misreports state over multi-step tasks while maintaining user trust.
- **Telemetry shape**: Agent's self-reported status separated from observed tool-call outcomes, so a downstream comparator can flag drift.
- **Schema-ready**: Yes — keeping agent narrative distinct from raw tool outputs is a design principle.
- **Detection**: NLI-style comparison or LLM-judge, future.

### 10. Emergent capabilities from composition

- **What**: A multi-agent system demonstrates capabilities not present in any individual agent.
- **Telemetry shape**: System-level outcome tracking joined with per-agent capability metadata.
- **Schema-ready**: Future. Requires a capability registry that does not exist and is gated on real customer multi-agent deployments.
- **Detection**: Open research problem.

---

## Design principles for an "AGI-ready" data plane

These rules govern how SENTINEL is built today so that future detectors can plug in without forcing a re-instrumentation of every customer:

1. **Capture all, decide later.** Telemetry is cheap relative to retroactive instrumentation; missing data is expensive. Default to verbose capture under a clear retention policy.
2. **Immutable audit trail.** Append-only storage and signed evidence bundles. The system must be able to prove **what** happened even when **why** is unclear.
3. **Schema-extensible.** Spans and events carry open extension fields. Future detectors add fields without breaking existing queries.
4. **Cross-agent correlation.** Distributed-trace identifiers propagate through every call; multi-agent analysis becomes possible without re-instrumentation.
5. **Ground truth separate from narrative.** The agent's own report of what it did is recorded distinctly from observed tool outputs. Drift detection requires this.
6. **Capability tagging.** Every agent declares the capability scopes it operates under (read / write / network / exec). Privilege escalation becomes detectable as a scope diff.
7. **Time-stamped everything.** High-resolution timestamps with monotonic ordering. Race conditions and coordination patterns require this.
8. **Reasoning capture only when emitted.** When the upstream framework exposes planning state or reasoning traces, the schema carries them. We do **not** attempt to extract reasoning from opaque LLM calls.
9. **Composable with frontier eval tools.** Integrate (do not replace) Inspect AI, METR, Lakera, and similar. SENTINEL is the data plane; their detectors are the analysis layer.
10. **Honest about limits.** The public threat model in [`SAFETY.md`](../SAFETY.md) names what SENTINEL does not detect today. Customers know the boundary before they buy.

---

## What this document is **not**

- Not a feature list.
- Not a roadmap. (See [`ROADMAP.md`](../ROADMAP.md).)
- Not customer-facing marketing copy. SENTINEL's customer copy is grounded in present-tense capability only.
- Not a claim that SENTINEL detects any of the behaviours above today.
- Not a substitute for frontier-lab safety work. Frontier-AI alignment is research, not a B2B product, and SENTINEL is honest about that boundary.

---

## How this list updates

A behaviour pattern moves into this list when **at least one** of the following becomes true:

- Peer-reviewed research describes the pattern with a reproducible signal in observable telemetry.
- A real production incident demonstrates the pattern with a public post-mortem.
- A frontier lab's safety framework (RSP / Preparedness / FSF) names the pattern explicitly.

A pattern moves out (or to a "deferred" archive) when:

- Consensus emerges that detection is infeasible at the telemetry layer.
- The pattern is fully addressed by an upstream layer (Layer 0 / 1 / 3 / 4) and SENTINEL adds no value as carrier.

This list is reviewed at the same cadence as [`ROADMAP.md`](../ROADMAP.md). Items here may move to the product roadmap **only when** validated detection methods exist, customer demand justifies the engineering, **and** SENTINEL's existing data plane is sufficient input.

---

## References

For the underlying frameworks and primary sources cited above (Anthropic RSP, OpenAI Preparedness, Google DeepMind FSF, METR, AISIs, Apollo, Redwood), see the **References** section of [`SAFETY.md`](../SAFETY.md). URLs are maintained in one place to keep them honest.

---

Last updated: 2026-05-06.
