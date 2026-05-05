# SAFETY.md — AI Safety, Governance, and Where SENTINEL Fits

> **Calibrated scope**: SENTINEL is a **production AI governance + observability** platform for organizations deploying AI agents. It is **not** a frontier-AI alignment lab and does not claim to solve AGI safety. This document explains exactly what SENTINEL does and does not address — and why being honest about that is itself a safety property.

---

## Where SENTINEL sits in the AI safety stack

The AI safety landscape has multiple layers. Different actors work at different layers:

```
LAYER 5 — Existential / civilizational
            (Treaties, international AI safety institutes — UK AISI, US AISI, EU AI Office)

LAYER 4 — Frontier capability research
            (Anthropic ASL/RSP, OpenAI Preparedness, Google DeepMind FSF —
             Internal to frontier labs, not externally tooled)

LAYER 3 — Pre-deployment evaluation
            (Red-teaming, capability evals, dangerous-capability tests —
             Inspect AI, MLCommons AILuminate, METR, Apollo Research)

LAYER 2 — Production observability + governance               ◄── SENTINEL operates here
            (Trace every AI agent action, enforce policies at runtime,
             produce audit-ready evidence, map to regulatory frameworks)

LAYER 1 — Application-level guardrails
            (Input/output filtering — Lakera, NeMo Guardrails, LlamaGuard,
             prompt-injection defenses)

LAYER 0 — Model-level safety training
            (RLHF, RLAIF, Constitutional AI — internal to model providers)
```

**SENTINEL operates at Layer 2.** We compose with — not replace — Layers 0/1 (Lakera, LlamaGuard, etc. as input/output filters) and Layers 3/4 (eval frameworks as upstream signals).

---

## What SENTINEL provides for AI safety / governance

### 1. Capability tagging on every span

Every traced AI invocation can be tagged with the **capabilities** it exercised:

- `tool_use` — agent called external tools / APIs
- `code_execution` — agent generated and ran code
- `web_browse` — agent fetched external URLs
- `pii_processing` — agent handled personal data
- `financial_decision` — agent made decisions affecting money
- `medical_decision` — agent made decisions affecting health
- `autonomous_action` — agent acted without human-in-the-loop

This is observability infrastructure. **Detection** of these capabilities (heuristic vs ML-based) is a separate problem; SENTINEL provides the **vocabulary and the carrier**.

### 2. Risk-tier classification (EU AI Act-aligned)

Every agent registered with SENTINEL gets classified into one of:

- `PROHIBITED` — uses prohibited under AI Act Article 5 (social scoring, manipulation, biometric mass-surveillance). Should not be deployed.
- `HIGH_RISK` — Annex III categories (employment, education, credit, healthcare, law-enforcement support, migration, justice, democratic processes).
- `LIMITED_RISK` — chatbot / generative content / emotion-recognition systems (Article 50 transparency obligations).
- `MINIMAL_RISK` — spam filters, recommendation systems with no impact on legal rights.
- `GENERAL_PURPOSE` — foundation-model-as-a-service (Article 51+).

The classification is **declarative** (the deploying org declares it) and SENTINEL stores it as evidence. We do **not** auto-classify with ML — that would be a single point of catastrophic miscategorization.

### 3. Jailbreak attempt schema

A standardized data shape for recording suspected jailbreak attempts on the agent's input. SENTINEL stores them; SENTINEL does **not** ship a detector. Detection is composable via Layer 1 tools (Lakera, LlamaGuard, OpenAI Moderation, etc.) — they emit the event in our schema, we route it to policies and evidence.

### 4. Dangerous-capability flags (advisory only)

Per OpenAI Preparedness Framework / Anthropic RSP categorization (as public references — not their internal scales):

- `cyber` — agent could discover or exploit security vulnerabilities
- `cbrn` — chemical, biological, radiological, nuclear uplift
- `model_autonomy` — agent could replicate, self-improve, or acquire resources
- `persuasion` — agent could perform large-scale targeted persuasion

Flagging is **manual / declarative** by the deploying org. SENTINEL never claims automated detection of these — that is research-grade work belonging in Layer 3 / Layer 4. We provide a place to **record** them so audit trails exist and policies can refuse / escalate.

### 5. Compliance framework mappings

Every policy can be tagged with references into established frameworks. We ship mappings (in `docs/compliance/frameworks.md`) for:

- **NIST AI Risk Management Framework (AI RMF 1.0, Jan 2023)** — Govern / Map / Measure / Manage functions
- **ISO/IEC 42001:2023** — AI Management System controls
- **EU AI Act (Regulation 2024/1689)** — Articles 8–15 (high-risk obligations), Article 50 (transparency), Articles 51–55 (GPAI)
- **OWASP LLM Top 10 (v1.1, 2024)** — LLM01 prompt injection through LLM10 model theft
- **MITRE ATLAS (v4)** — adversarial threats to AI systems
- **OECD AI Principles** — values-based recommendations
- **PDPA Thailand (B.E. 2562)** — Sections 26, 28, 32, 37 (Je's local-market expertise)

These are mappings to **public, established** frameworks. We do not invent our own.

---

## What SENTINEL does NOT provide (intentionally)

- **Model safety training** — that is Layer 0, internal to model providers.
- **Frontier capability evaluations** — that is Layer 3/4, requires research-grade red-teaming infrastructure and access to model internals we do not have.
- **AGI alignment research** — open research problem, not a product feature.
- **Automated dangerous-capability detection** — too high-stakes for heuristics; we record declarations, we do not infer.
- **Input/output content moderation** — Lakera / LlamaGuard / OpenAI Moderation already do this well; we integrate with them.
- **Prompt-injection prevention as a primary control** — see Simon Willison's work; layered defense is required, and SENTINEL is one layer of that defense, not the whole defense.

We will not claim otherwise in any marketing or pitch material. **Overclaiming AI safety capability is itself a safety failure** — it leads operators to under-invest in real defenses.

---

## Threat model (production AI agents)

(Derived from MITRE ATLAS + OWASP LLM Top 10 + PDPA + GDPR Article 22 considerations.)

| Threat | Layer where addressed | SENTINEL contribution |
|---|---|---|
| Prompt injection | Layer 1 (input filter) + Layer 2 (policy on outputs) | Record events; policy on observed bad outputs |
| Insecure output handling | Layer 1/2 | Policy can BLOCK based on output content matches |
| Training data poisoning | Layer 0 (provider) | Out of scope |
| Model denial of service | Layer 1 (rate limit) + infra | Out of scope as primary control; observed via traces |
| Supply chain (compromised model / library) | Layer 0/dev process | SBOM + dep audit in CI |
| Sensitive information disclosure | Layer 1 (redact) + Layer 2 (audit + policy) | Primary contribution: audit + redaction policies |
| Insecure plugin design | Application | Tracing + policy on tool calls |
| Excessive agency | Layer 1/2 | Primary contribution: human-in-the-loop policies, escalation actions |
| Overreliance | UX + governance | Audit trails support governance reviews |
| Model theft | Infra security | Out of scope |

(The "primary contribution" rows are where SENTINEL adds value that wasn't otherwise easily available.)

---

## Honest disclosures

- **No certifications yet**: SENTINEL is not SOC 2, ISO 27001, ISO 42001, or AICPA-attested. Those come post-product-traction. Stating otherwise pre-attestation would be fraud.
- **No formal verification**: the policy engine is not formally verified. False positives and false negatives are possible. Customers must validate policies against their actual workloads.
- **No clinical evidence**: any claim that SENTINEL "ensures safety" of medical or financial AI agents is overreach. SENTINEL provides observability and policy infrastructure; final safety judgment rests with the operator.
- **Regulatory mappings are advisory**: the article references in `docs/compliance/` are SENTINEL's interpretation and may not be definitive. Customers should validate with their own counsel.

---

## Reporting safety / security issues

See [`SECURITY.md`](./SECURITY.md). For research-grade safety findings (unexpected emergent behaviors observed in production agents instrumented with SENTINEL), please reach out — those are valuable signal for the field.

---

## References

- NIST AI RMF 1.0 — https://www.nist.gov/itl/ai-risk-management-framework
- ISO/IEC 42001:2023 — https://www.iso.org/standard/81230.html
- EU AI Act (Regulation 2024/1689) — official text via EUR-Lex
- OWASP LLM Top 10 — https://genai.owasp.org/
- MITRE ATLAS — https://atlas.mitre.org/
- Anthropic Responsible Scaling Policy — https://www.anthropic.com/news/anthropics-responsible-scaling-policy
- OpenAI Preparedness Framework — https://openai.com/preparedness
- Google DeepMind Frontier Safety Framework — https://deepmind.google/discover/blog/introducing-the-frontier-safety-framework/
- UK AI Safety Institute — https://www.aisi.gov.uk/

(URLs verified as of writing — verify currentness when you read this. Report dead links.)
