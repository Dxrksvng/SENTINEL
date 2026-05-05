# SENTINEL — Vision

> The long view. Calibrated, not hyped.
>
> If the README answers *"what are you building?"*, this file answers *"why does this need to exist, and where is it going?"*

---

## The problem

AI agents are reaching production faster than the governance to oversee them.

The teams shipping these agents — banks, hospitals, public-sector platforms, enterprise SaaS — are caught between three pressures at once:

1. **Engineering** wants to ship. Agents now make decisions that used to require humans, and the surface area is growing weekly.
2. **Compliance and risk** owners are accountable for behaviour they cannot see. Existing observability tools were not built for non-deterministic systems.
3. **Regulators and customers** are catching up — the EU AI Act, NIST AI RMF, ISO/IEC 42001, and Thailand's PDPA are converging on the same question: *can you prove this agent behaved within policy?*

Today, the honest answer in most production deployments is **no**. Logs exist. Traces sometimes exist. But the bridge from "what the agent did" to "the article of the framework I need to report against" is hand-built every time.

That bridge is missing infrastructure.

---

## The solution

SENTINEL sits between AI agents and the people accountable for them.

It does three things, and only three things:

- **Observe.** Capture what the agent did, in a portable open-standard format.
- **Govern.** Evaluate runtime policy. Start in *observe* mode. Add masking, refusal, and escalation as customers earn the right to ask for them.
- **Evidence.** Produce audit-ready exports that map cleanly to the frameworks compliance officers already report against.

Equally important is what SENTINEL **does not** do:

- It does not claim to solve AI alignment.
- It does not claim to detect jailbreaks at scale.
- It does not auto-classify risk on behalf of a regulator.
- It does not replace input/output filters or capability evaluators that already exist upstream and downstream.

The point is **calibrated competence** in one layer of the stack — the layer that turns runtime behaviour into evidence — done well enough to be trusted by the people who have to sign their name to it.

---

## Where SENTINEL sits in the AI safety stack

```
Layer 5 — Existential / civilizational governance (treaties, AISIs)
Layer 4 — Frontier capability research (RSPs, Preparedness, FSF)
Layer 3 — Pre-deployment evaluation (Inspect AI, METR, Apollo)
Layer 2 — Production observability + governance              ◄── SENTINEL
Layer 1 — Application-level guardrails (Lakera, LlamaGuard, NeMo)
```

The layers above us are research-grade and policy-grade. The layer below us catches obvious bad inputs and outputs at the edges of a single call. SENTINEL works in the gap between them — the **runtime, system-level layer** where most regulatory questions actually live: *over the last quarter, did this agent behave within policy?*

---

## Why now

Three forces line up for the first time in 2026:

1. **Regulation is shipping, not just being drafted.** The EU AI Act enters substantive enforcement windows. Thailand's PDPA is being interpreted by the courts, not just published. ISO/IEC 42001 is being asked for in vendor questionnaires.
2. **Production agents are real.** A year ago, "AI agent in production" meant a chatbot. Now it means tools that move money, schedule clinical resources, and handle case routing inside government services.
3. **The observability standard is ready.** OpenTelemetry's GenAI semantic conventions are stabilising. Building on them today is no longer betting on a draft.

Earlier than this and the buyers were not ready. Later than this and the category gets defined by someone else.

---

## Long-term vision

SENTINEL aims to become the **runtime governance layer** for AI agents — the boring, trusted plumbing that lets ambitious AI deployments happen at all in regulated contexts.

That means three trajectories, in order:

1. **Evidence layer** for organisations already operating production agents. *(Phase 1–3.)*
2. **Policy layer** that gets specified and enforced consistently across an enterprise's agent fleet. *(Phase 3–4.)*
3. **Control plane** for autonomous AI systems where the agents themselves negotiate, defer, and escalate against shared policy. *(Phase 5+.)*

We do not need step three to be true today. Step one is enough to be a real business and a real public good. The later steps are what we are quietly designing toward, so we do not paint ourselves into a Phase-1-only corner.

---

## Founder posture

SENTINEL is being built solo, in public-but-careful mode, by an engineer who has shipped AI systems end-to-end and is choosing this category deliberately.

The values driving the build:

- **Honesty over hype.** Pre-alpha is pre-alpha. We will not claim traction we do not have.
- **Calibrated capabilities.** Each release ships with a clear statement of what it can and cannot do. No marketing in changelogs.
- **Composability over capture.** Customers can leave. Open standards on the wire.
- **Auditor empathy.** Evidence is shaped for the human who has to sign the report, not for engineers admiring the trace tree.
- **Self-host first for sensitive customers.** Hosted is a convenience tier, not the only door.

If those values resonate with how you think about AI in production, this is a project worth watching.
