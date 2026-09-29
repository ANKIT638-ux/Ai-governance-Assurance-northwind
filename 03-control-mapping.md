# Control Mapping — Northwind Retail

**Purpose:** Connects each risk to a specific control, and each control to the three frameworks
the project uses side by side. They're not redundant with each other:

- **NIST AI RMF** (Govern / Map / Measure / Manage) gives the *lifecycle* — where in the
  organisation's process this control lives.
- **OWASP LLM Top 10 (2025)** gives the *vulnerability* vocabulary — what class of weakness
  the control addresses.
- **MITRE ATLAS** gives the *adversary* vocabulary — the specific technique an attacker would
  use, which is what a red team actually tests against.

A mature program uses NIST during governance design, OWASP during development/secure design
review, and ATLAS during red-teaming and detection engineering.

| Control ID | Control description | Addresses risk | NIST AI RMF function | OWASP LLM Top 10 (2025) | MITRE ATLAS technique |
|---|---|---|---|---|---|
| **C-001** | Output-scoping / context isolation: Nova's retrieval layer is restricted so a single conversation can only retrieve records tied to the authenticated requester's own account, not queried freely by the LLM | R-001 | Manage (risk treatment); Map (system boundaries) | LLM01: Prompt Injection; LLM02: Sensitive Information Disclosure | AML.T0051 — LLM Prompt Injection (Indirect); AML.T0057 — LLM Data Leakage |
| **C-002** | Input sanitisation / delimiter enforcement on customer-submitted free-text fields before they enter the LLM context window | R-001 | Manage | LLM01: Prompt Injection | AML.T0051 — LLM Prompt Injection (Indirect) |
| **C-003** | Output filtering: automated PII-detection scan on Nova's responses before they're sent to a customer, blocking any response containing a second identity's PII | R-001 | Measure (ongoing monitoring); Manage | LLM02: Sensitive Information Disclosure; LLM05: Improper Output Handling | AML.T0057 — LLM Data Leakage |
| **C-004** | Least-privilege tool scoping for DevAssist: repo write and CI execution permissions scoped per-repository, not firm-wide; secrets scanning runs pre-commit and pre-merge | R-002 | Govern (roles/permissions); Manage | LLM06: Excessive Agency; LLM03: Supply Chain | AML.T0053 — AI Agent Tool Invocation; AML.T0086 — Exfiltration via AI Agent Tool Invocation |
| **C-005** | Human-in-the-loop merge gate: DevAssist can open PRs autonomously but cannot merge to protected branches without a human engineer's approval | R-002 | Manage | LLM06: Excessive Agency | AML.T0110 — AI Agent Tool Poisoning |
| **C-006** | RAG source-of-truth freshness check: HR-Bot's policy index is validated against the canonical SharePoint library on a defined cadence, with a "last verified" citation shown to the employee | R-003 | Measure | LLM09: Misinformation | — (not adversarial; reliability control) |
| **C-007** | Escalation threshold: HR-Bot must route any question involving termination, leave-of-absence disputes, or compensation to a human HR partner rather than answering directly | R-003 | Manage | LLM09: Misinformation | — |
| **C-008** | Supplier data-feed integrity checks (schema validation, statistical outlier detection) before ingestion into MerchIQ's forecasting pipeline | R-004 | Map; Measure | LLM04: Data and Model Poisoning | AML.T0020 — Poison Training Data; AML.T0070 — RAG Poisoning |
| **C-009** | Human approval required for any MerchIQ recommendation above a defined financial threshold before execution | R-004 | Manage | LLM09: Misinformation | — |
| **C-010** | Velocity/anomaly monitoring on Nova's autonomous refund function (per-account and aggregate thresholds, auto-suspend on breach) | R-005 | Measure; Manage | LLM06: Excessive Agency | AML.T0086 — Exfiltration via AI Agent Tool Invocation (adapted: financial exfiltration via tool abuse) |
| **C-011** | Mandatory pre-deployment AI inventory registration and risk assessment gate, enforced through the change-management process | R-006 | Govern | — (governance/process control, not a technical vulnerability class) | — |
| **C-012** | Quarterly AI inventory attestation by system owners, reviewed by AI Governance | R-006 | Govern | — | — |
| **C-013** | Velocity/pattern monitoring on ExpenseAuditor's auto-approval path: flags any employee whose sub-$200 submission frequency or cumulative sub-$200 total significantly exceeds their historical baseline within a rolling window; flagged accounts are auto-routed to mandatory human review until cleared | R-008 | Measure; Manage | — (not a technical AI vulnerability — a business-policy threshold being gamed by an insider, not the AI malfunctioning or being attacked) | — (no adversary technique — insider misuse of a correctly-functioning system, not an attack on the AI) |
| **C-014** | Real-time suppression-list check on MarketPulse: before any scheduled campaign executes, the send list is cross-checked against current opt-outs/preference changes at the moment of send — not only at signup — with suppressed recipients automatically dropped from that send | R-007 | Manage; Measure | — (not an OWASP LLM vulnerability class — a consent/regulatory compliance gap, not a model security or reliability failure) | — (not an adversary technique — no attacker involved, this is a process/consent-tracking gap) |

## Notes on mapping judgment calls

- **C-011/C-012/C-013/C-014 have no OWASP or ATLAS mapping** — this is intentional and worth
  saying plainly rather than forcing a fit. C-011/C-012 are governance/process controls
  addressing an organisational gap; C-013 addresses insider misuse of a system that's working
  exactly as designed (a policy-threshold problem, not an AI-security problem); C-014 addresses a
  consent/regulatory-tracking gap with no adversary involved at all. None of these are the AI
  malfunctioning or being attacked, which is what OWASP and ATLAS are built to describe — so
  forcing either framework onto them would misdiagnose what's actually wrong. Not every control
  needs to map to all three frameworks.
- **C-006/C-007/C-009 map only loosely to ATLAS** because ATLAS is adversary-centric and these
  controls address *reliability* failures (hallucination, drift) rather than an adversary's
  deliberate action. They're included because a compliance register has to cover reliability
  risk, not only adversarial risk — that distinction itself is a useful thing to be able to
  explain.
