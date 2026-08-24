# AI Risk Register — Northwind Retail
 
**Purpose:** Turns technical exposure into a business risk decision — likelihood, impact,
treatment, owner, and a status leadership can act on. Ratings use a simple 3x3 scale
(Low/Medium/High) for likelihood and impact, which is deliberate: a compliance register that's
overengineered on day one doesn't get used. Every risk here traces to a system in the AI
Inventory and, once mapped, to a control in `03-control-mapping.md`.
 
| Risk ID | System | Risk description | Source | Likelihood | Impact | Inherent risk | Treatment | Owner | Status |
|---|---|---|---|---|---|---|---|---|---|
| **R-001** | AI-002 Nova | **Indirect prompt injection via a customer support ticket causes Nova to disclose another customer's PII** (name, address, order history) that it retrieved from the CRM while resolving the ticket. Attacker embeds hidden instructions in the free-text "issue description" field; Nova reads this as part of its context and follows the injected instruction instead of its system prompt. | Red team finding demonstrated | High | High | **Critical** | Mitigate | Head of Customer Experience / AI Governance Lead | Open — remediation in progress (see traceability thread) |
| R-002 | AI-003 DevAssist | Coding agent with repo write + sandbox execution is manipulated (via a poisoned issue/PR description or dependency) into exfiltrating secrets from a legacy repo, or opening a PR that introduces a backdoor that passes automated review. | Threat modeling / industry incident pattern | Medium | High | High | Mitigate | VP Engineering | Open |
| R-003 | AI-001 HR-Bot | Hallucinated or outdated HR policy guidance (e.g. incorrect leave entitlement or benefits eligibility) is given to an employee with confidence, leading to a wrong decision or a grievance. | Internal QA testing | Medium | Medium | Medium | Mitigate | Head of HR Technology | Open |
| R-004 | AI-004 MerchIQ | Demand-forecast inputs from a compromised or low-quality supplier data feed silently degrade recommendation quality (data poisoning / integrity failure) without triggering an obvious error, leading to over/under-stocking at scale. | Threat modeling | Low | High | Medium-High | Mitigate | Head of Merchandising | Open |
| R-005 | AI-002 Nova | Adversarial or repeated inputs abuse the autonomous sub-$50 refund function to extract small fraudulent refunds at volume ("death by a thousand cuts"). | Threat modeling | Medium | Medium | Medium | Mitigate | Head of Customer Experience | Open |
| R-006 | Cross-system | **Governance gap:** no enforced pre-deployment registration process existed before this project; risk that future AI systems ("shadow AI") go live without inventory entry, risk assessment, or control mapping. | Governance self-assessment | Medium | High | High | Mitigate (process control) | AI Governance Lead | Open — process now defined in `01-ai-inventory.md` |
| R-007 | AI-005 MarketPulse | Customer opt-outs (unsubscribe/preference changes) are not re-validated against the send list at the moment a scheduled campaign fires — only checked at signup — risking messages sent to customers who withdrew consent, in violation of CAN-SPAM/TCPA (US) and GDPR (EU). Partially transferable via the vendor Data Processing Agreement, but the obligation to honor opt-outs ultimately remains Northwind's, not the vendor's. | Privacy/compliance self-assessment | Medium | High | **High** | Mitigate + Transfer (partial, via vendor DPA) | Head of Marketing | Open |
| **R-008** | AI-006 ExpenseAuditor | **Threshold gaming ("structuring"):** an employee splits a larger expense into multiple submissions each just under the $200 auto-approval threshold to avoid human review. Demonstrated by Internal Audit (finding IA-2026-001): one employee submitted 5 sub-$200 claims within a 7-day window, totaling ~$1,000, all auto-approved with zero human review. Root cause: the system evaluates each submission independently, with no check for cumulative claims from the same employee over time. | Internal Audit finding (IA-2026-001), demonstrated | High | Medium | **High** | Mitigate | Head of Finance | Open — remediation in progress (REM-2026-001) |
 
## Rating scale used
 
**Likelihood** — Low: theoretical/no evidence of exploitability · Medium: plausible, some
preconditions required · High: demonstrated or trivially reproducible
 
**Impact** — Low: limited/internal, no regulatory or customer exposure · Medium: contained
customer or operational impact, no reporting obligation · High: regulatory notification
obligation, material financial loss, or reputational/media exposure
 
**Inherent risk** = likelihood × impact, pre-mitigation. Residual rating (post-control) is
tracked at the individual control/evidence level in `04-evidence-register.md` and rolled up in
the dashboard.
 
## Why R-001 is rated Critical, not just High
 
Two things push it above the other findings: it is **demonstrated**, not theoretical (removes all
doubt on likelihood), and the impacted data is customer PII on an external-facing system, which
carries a potential regulatory notification obligation, not just internal cleanup. This is the
finding carried through the full traceability thread.
 
R-008 is a useful contrast: it's also **demonstrated**, so likelihood is equally High — but its
impact is rated Medium, not High, because a $1,000 structuring pattern is a contained financial
loss with no regulatory notification trigger and no external/reputational exposure. Same
likelihood, different impact driver, different final rating — a reminder that "demonstrated"
alone doesn't automatically mean Critical.
 
