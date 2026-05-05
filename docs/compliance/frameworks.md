# Compliance Framework Mappings

> Reference mappings between SENTINEL primitives and public regulatory frameworks. These are **advisory** — customers must validate with their own legal/compliance counsel.

This file is a **map**, not a **manual**. It tells you where to look. The full mapping packs (which articles trigger which policies for which agent types) live in `docs/compliance/packs/<framework>/` and are populated **after** customer interviews confirm which articles are operationally painful (per the validation phase — see `validation/`).

---

## NIST AI Risk Management Framework (AI RMF 1.0)

Four high-level functions:

| NIST function | SENTINEL primitive |
|---|---|
| **Govern** | Policy library, version-controlled rules, audit trail of policy changes |
| **Map** | Agent discovery, capability tagging, risk-tier classification |
| **Measure** | Trace ingest, dashboard metrics, incident counts |
| **Manage** | Runtime policy enforcement (block/redact/escalate), evidence export |

Subcategories within each function (Govern 1.1 through Manage 4.3) are mapped in `packs/nist-ai-rmf/` once the wedge is validated.

---

## ISO/IEC 42001:2023 — AI Management System

A management-system standard (like ISO 27001 for InfoSec). Annex A controls map onto SENTINEL primitives:

| ISO 42001 Annex A control area | SENTINEL primitive |
|---|---|
| A.2 — Policies related to AI | Policy YAML library (version-controlled) |
| A.3 — Internal organization | Audit trail of who authored / approved each policy |
| A.4 — Resources | Compute/storage tracking (Phase 3) |
| A.5 — Assessing impacts | Risk-tier classifier + capability tags |
| A.6 — AI system life cycle | Trace coverage from dev → staging → prod |
| A.7 — Data for AI systems | PII redaction + data-classification attributes |
| A.8 — Information for interested parties | Evidence export endpoint |
| A.9 — Use of AI systems | Human-in-the-loop policy actions (ESCALATE) |
| A.10 — Third-party relationships | SBOM + dep audit |

---

## EU AI Act (Regulation 2024/1689)

Key articles and SENTINEL coverage:

| Article | Topic | SENTINEL primitive |
|---|---|---|
| **5** | Prohibited practices | Risk-tier `PROHIBITED` blocks deployment |
| **6 + Annex III** | High-risk classification | Risk-tier `HIGH_RISK` tagging |
| **9** | Risk management system | Threat model + policy library |
| **10** | Data governance | PII tagging, training-data hash attributes |
| **11** | Technical documentation | Auto-generated from trace evidence |
| **12** | Record-keeping | **Core SENTINEL function** — append-only trace storage |
| **13** | Transparency to deployers | Article 50 chatbot disclosures via policy |
| **14** | Human oversight | ESCALATE action, oversight events on spans |
| **15** | Accuracy, robustness, cybersecurity | Drift detection (Phase 3), security baseline |
| **16** | Provider obligations | Provider/deployer separation in metadata |
| **50** | Transparency for limited-risk | Chatbot-disclosure policy template |
| **51–55** | General-purpose AI models | GPAI tier + dangerous-capability flags |
| **72** | Post-market monitoring | Incident tracking + drift signals |
| **73** | Reporting of serious incidents | 72-hour breach workflow |

Enforcement timeline (as of writing — verify when reading):
- 2024-08-01: Act enters into force
- 2025-02-02: Prohibited practices apply
- 2025-08-02: GPAI rules apply
- **2026-08-02**: High-risk obligations apply (the deadline driving customer urgency)
- 2027-08-02: High-risk safety components apply

---

## OWASP LLM Top 10 (v1.1, 2024)

| ID | Threat | SENTINEL primitive |
|---|---|---|
| LLM01 | Prompt injection | Span attribute for input source; policy on observed exfiltration patterns |
| LLM02 | Insecure output handling | Output-content policy actions |
| LLM03 | Training data poisoning | Training-data-hash attribute (declarative) |
| LLM04 | Model denial of service | Trace volume metrics; rate limit signals |
| LLM05 | Supply chain | SBOM + pip-audit / npm-audit in CI |
| LLM06 | Sensitive information disclosure | PII redaction + audit |
| LLM07 | Insecure plugin design | Tool-call tracing + policy |
| LLM08 | Excessive agency | Human-oversight policies, ESCALATE action |
| LLM09 | Overreliance | Evidence trails support governance reviews |
| LLM10 | Model theft | Out of scope — infra concern |

---

## MITRE ATLAS

Mapping at the tactic level (techniques mapped in `packs/mitre-atlas/` once validated):

| ATLAS tactic | SENTINEL primitive |
|---|---|
| Reconnaissance | Trace volume anomalies |
| Resource Development | Out of scope |
| Initial Access | Auth events on the SDK |
| ML Model Access | API key usage tracking |
| Execution | Tool-call tracing |
| Persistence | Out of scope |
| Defense Evasion | Jailbreak schema events |
| Discovery | Out of scope |
| Collection | PII tagging |
| Exfiltration | Output-policy enforcement |
| Impact | Incident tracking |

---

## OECD AI Principles

Values-based; not directly enforceable but useful for narrative:

- Inclusive growth, sustainable development, well-being
- Human-centred values and fairness
- Transparency and explainability
- Robustness, security, safety
- Accountability

SENTINEL's contribution is principally **transparency, accountability, and security** — observability + audit trails + policies.

---

## PDPA Thailand (B.E. 2562 / 2019)

| PDPA Section | Topic | SENTINEL primitive |
|---|---|---|
| **26** | Lawful basis for processing | Span attribute for declared lawful basis |
| **28** | Cross-border data transfer | Region-based block policy |
| **32** | Data subject rights | Per-subject trace queries |
| **37** | Security measures + breach notification | 72-hour workflow + incident tracking |
| **39** | Records of processing activities | Trace storage = built-in ROPA |
| **41** | DPO appointment | Audit-trail metadata field |

PDPC enforcement context (Thailand):
- PDPA fully enforced since June 2022
- ก.ล.ต. and BOT issuing AI-specific guidance for financial services
- Cross-border transfer rules (s. 28) are the most operationally painful section per industry feedback

---

## How a customer uses these mappings

1. Identify which framework(s) they are subject to.
2. Pick the relevant article/section IDs.
3. Tag each policy with `compliance_refs: [{framework: ..., reference: ..., note: ...}]`.
4. At evidence-export time, group decisions by `compliance_refs.framework` to produce a per-framework audit pack.

Mappings improve as customers tell us which articles actually bite. **The only good map is one that has been walked.**
