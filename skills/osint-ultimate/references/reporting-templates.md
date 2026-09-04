# Reporting Templates

## Bug bounty / pentest finding (single issue)

```
## [Severity] Title describing asset + vulnerability class

**Asset:** affected URL/endpoint/system
**Severity:** Critical / High / Medium / Low / Informational
**Summary:** 2-3 sentences on what the issue is.

**Steps to reproduce:**
1. ...
2. ...
3. ...

**Impact:** what an attacker could actually do - grounded in what was demonstrated, not worst-case speculation.

**Evidence:** screenshots / request-response logs (redact any real user data touched incidentally)

**Suggested remediation:** concrete fix, not just "patch it"
```

## Client-facing pentest/assessment report (full engagement)

```
# [Client] External Assessment - [Date range]

## Executive Summary
Plain-language summary for non-technical stakeholders: what was tested, headline risk level, top 3 priorities.

## Scope & Methodology
What was in scope, what techniques/tools were used, engagement dates, any limitations.

## Findings Summary Table
| # | Title | Severity | Status |

## Detailed Findings
(one block per finding, using the bug-bounty template above)

## Risk Translation Matrix
Map technical severity to business risk (regulatory exposure, reputational, financial) for leadership.

## Appendix: Reproduction Package
Raw evidence, logs, timestamps for engineering to reproduce independently.
```

## OSINT / investigative research brief (journalism, due diligence)

```
# [Subject] - Research Brief

## Question(s) being investigated

## Findings
For each finding: claim, source(s), date, confidence level (confirmed/likely/possible/unverified)

## Sources consulted
Full list, including ones that didn't pan out - useful for anyone continuing the work.

## Open questions / gaps
What you couldn't verify and why (paywalled, deleted, contradictory sources, etc.)
```

## General principles for any report

- Every claim gets a source and a date.
- Separate observation from inference - "the DNS record shows X" vs. "this suggests the org migrated providers in [month]."
- State confidence explicitly rather than letting tone imply certainty.
- Note what you didn't check, not just what you did - an honest gap list is more useful than a report that looks complete but silently skipped things.
