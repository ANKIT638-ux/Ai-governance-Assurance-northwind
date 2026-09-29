# Evidence Register — Northwind Retail

**Purpose:** A control that isn't tested is an assertion, not assurance. This is the piece most
spreadsheet-style governance projects skip — it's easy to list a control, harder to say exactly
how you'd *prove* it's working, on what cadence, and who signs off. This register also records
the current test result, which is what actually feeds the executive dashboard and closes (or
keeps open) the loop back into the risk register.

| Control ID | How it's tested | Evidence / proof artifact | Test frequency | Owner | Last test result |
|---|---|---|---|---|---|
| C-001 | Automated integration test: authenticated session for Customer A attempts to retrieve Customer B's order/account data via crafted prompts; also included in quarterly red-team exercise | Test suite run logs; red-team report | Automated: every deploy · Red-team: quarterly | AI Governance / Security Eng | **FAIL** (see R-001 / traceability thread) — retest scheduled post-fix |
| C-002 | Adversarial input test set (known prompt-injection payloads) run against the sanitisation layer in CI | CI pipeline test results; payload library with pass/fail per entry | Every deploy | Security Engineering | Not yet implemented — gap identified during remediation scoping |
| C-003 | Synthetic conversations seeded with a second identity's PII in context; verify output filter blocks disclosure | Automated test logs; sample blocked/unblocked transcripts reviewed monthly | Automated: every deploy · Manual sample review: monthly | AI Governance | Not yet implemented — gap identified during remediation scoping |
| C-004 | Quarterly access review of DevAssist's repo/CI permissions vs. least-privilege baseline; automated secrets-scanning tool run pre-commit and pre-merge | Access review sign-off; secrets-scanner CI logs | Access review: quarterly · Scanner: every commit | VP Engineering / Security Eng | Pass — last review 2026-Q2, no material findings |
| C-005 | Branch protection rule audit confirming DevAssist's service account cannot merge to protected branches | GitHub branch protection settings export, reviewed by Security Eng | Quarterly | VP Engineering | Pass |
| C-006 | Automated diff check between HR-Bot's indexed policy content and the canonical SharePoint library; alert on drift | Sync job logs; alert history | Daily automated check | Head of HR Technology | Pass |
| C-007 | Sample review of HR-Bot conversation logs for out-of-scope topics (termination, comp, LOA disputes) that should have escalated but didn't | Manual log sample review (n=50/month) | Monthly | Head of HR Technology | Pass — 0 missed escalations in last sample |
| C-008 | Automated schema validation and outlier detection run on each supplier feed ingestion; manual review of flagged anomalies | Pipeline validation logs; anomaly review notes | Every ingestion (daily) | Head of Merchandising / Data Eng | Pass |
| C-009 | Audit of MerchIQ recommendations above threshold, confirming human approval recorded before execution | Approval workflow audit log | Monthly | Head of Merchandising | Pass |
| C-010 | Simulated abuse pattern (repeated near-threshold refund requests) run against Nova's monitoring rules in a test environment | Test scenario logs; alert trigger confirmation | Quarterly | Head of Customer Experience | Pass |
| C-011 | Sample of AI systems deployed in the quarter checked against the inventory for registration prior to go-live | Change-management ticket audit | Quarterly | AI Governance Lead | Pass — process implemented this cycle, first full audit due next quarter |
| C-012 | Confirmation that each system owner submitted their quarterly attestation | Attestation tracker | Quarterly | AI Governance Lead | Pass — 4/4 received this cycle |
| C-013 | Automated test script submits simulated sub-$200 expense claims from a test employee account, shaped to match a structuring pattern (e.g., 5 claims within a 7-day window, mirroring finding IA-2026-001); verifies the account is correctly flagged and auto-approval is suspended | Timestamped test run logs showing flag triggered, auto-approval suspended, and claim routed to human review | Every deploy touching approval/monitoring logic, plus full quarterly review alongside Internal Audit's regular expense-policy audit | Head of Finance / Internal Audit | **FAIL** (see R-008 / IA-2026-001) — control not yet built; interim mitigation (manual review of all sub-$200 claims) in effect per REM-2026-001 |
| C-014 | Automated test submits a simulated campaign send to a test recipient list that includes a recently-suppressed contact; verifies the suppressed contact is dropped from the send before it fires | Test run logs showing suppression-list cross-check executed and suppressed contact excluded from send | Every deploy touching the send pipeline, plus monthly manual sample audit of actual sends vs. suppression list | Head of Marketing | Not yet implemented — gap identified during control design; flagged as a priority build alongside C-013 |

## Reading the failures honestly

C-001 through C-003 — all three controls tied directly to the Nova PII-disclosure finding — show
either a **FAIL** or **not yet implemented**. C-013 (ExpenseAuditor structuring detection) and
C-014 (MarketPulse suppression check) show the same pattern: both are newly-identified controls
for newly-found risks, and neither is built yet. That's deliberate, not an oversight: a governance
system whose evidence register shows every control passing on day one isn't credible. The value
of this register is that it shows the actual state, including the gaps, and feeds each gap
directly into a remediation ticket — see `06-traceability-thread.md` for how this plays out end
to end for C-013.
