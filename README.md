# Ai Governance Assurance- Northwind
 
A fictional AI governance program built for **Northwind Retail**, a made-up retailer running
four AI systems, an internal HR chatbot, a customer-facing support assistant that can issue
refunds, a coding agent with repo access, and a decision-support model.
 
I built this to practice about AI security and compliance that's easy to underrate
from the outside: turning a technical AI risk into something a business can actually track,
own, fix, and *prove* it fixed, not just flag and forget.
 
The project covers the full lifecycle: AI inventory → risk register → control mapping (to
[NIST AI RMF](https://www.nist.gov/itl/ai-risk-management-framework), the
[OWASP LLM Top 10](https://owasp.org/www-project-top-10-for-large-language-model-applications/),
and [MITRE ATLAS](https://atlas.mitre.org/)) → an evidence register proving those controls
actually work, not just that they're documented → an executive dashboard leadership could
actually read.
 
**Start here:** [`06-traceability-thread.md`](./northwind-ai-governance/06-traceability-thread.md) it follows one red-team finding (a prompt injection attack that leaked customer data) end to
end: attack → logged risk → mapped controls → evidence gap → remediation ticket → independent
retest → closed. It's the single artifact that shows what the rest of the project is actually.
 
## What's in here
 
| File | What it is |
|---|---|
| `01-ai-inventory.md` | Every AI system Northwind runs, owner, data, autonomy level, risk tier |
| `02-ai-risk-register.md` | Identified risks, rated and assigned an owner and treatment |
| `03-control-mapping.md` | Controls mapped to NIST AI RMF / OWASP LLM Top 10 / MITRE ATLAS |
| `04-evidence-register.md` | How each control is tested, and its current pass/fail status |
| `05-executive-dashboard.md` | One screen risk posture summary for leadership |
| `06-traceability-thread.md` | One finding traced end to end, the core artifact |
| `07-written-report.md` | Full narrative report explaining the system's design choices |
| `08-executive-summary.md` | One page summary for a non-technical audience |
 
## Extending it myself
 
After building the base four system project, I added two systems and findings entirely on my
own **MarketPulse**, a marketing AI with a consent/regulatory risk I traced myself (autonomous
campaign sends not re-validating opt-outs at send time which is a TCPA/CAN-SPAM/GDPR-adjacent risk),
and **ExpenseAuditor**, a finance AI where I identified an insider fraud risk (an employee
structuring expense claims just under an auto approval threshold to avoid human review) and
built a full inventory through remediation thread for it, including the control design, evidence
test, remediation ticket, and retest. I did this specifically to test whether I understood the
framework well enough to apply it to a system I hadn't seen before, not just follow a template.
 
## Frameworks referenced
 
- [NIST AI Risk Management Framework (AI RMF 1.0)](https://www.nist.gov/itl/ai-risk-management-framework) and the Generative AI Profile (NIST AI 600-1)
- [OWASP Top 10 for LLM Applications (2025)](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
- [MITRE ATLAS](https://atlas.mitre.org/)


This project has come to fruition with the idea of this github repo: https://github.com/taimurijlal/AI-Security-Projects/tree/main/05-ai-governance-assurance

