# Bug Bounty & Pentest Recon - Methodology

This covers how to run the **reconnaissance and reporting** phases of an authorized engagement well. It does not cover exploitation, payload crafting, or credential testing - those happen with dedicated licensed tooling (Burp Suite, your program's testing environment) under your own hands, inside the program's rules of engagement. Claude helping generate exploit chains isn't something this skill does, authorized or not - treat this file as the "before you touch anything live" and "after you found something" halves of the job.

## 1. Before anything else: scope

- Read the program's scope page (HackerOne/Bugcrowd/Intigriti listing, or the signed SOW) fully. Note: in-scope domains/apps, explicitly out-of-scope items, disallowed techniques (e.g. "no automated scanners," "no social engineering"), and reward/severity table.
- If a domain or subdomain isn't explicitly in scope, don't touch it, even if it looks related. When unsure, ask the program (through their platform) rather than assuming.

## 2. Time-budget the recon phase

Rough allocation that scales with engagement length - adjust to the program's size:

| Budget | Recon | Enrichment | Analysis | Reporting |
|---|---|---|---|---|
| 1 hour (quick look) | 30 min | 15 min | 10 min | 5 min |
| 4 hours | 1.5 hr | 1 hr | 1 hr | 30 min |
| 1 day | 3 hr | 2 hr | 2 hr | 1 hr |
| 1 week | spread across days, revisit recon as new assets surface |

## 3. Recon pipeline (passive - see `domain-infra-recon.md` for technique detail)

1. **Seed discovery** - root domains from scope, WHOIS/RDAP, public records for the org.
2. **Asset expansion** - certificate transparency, Wayback CDX, DNS records - build a list of subdomains/apps.
3. **Enrichment** - for each live asset: technology fingerprint, response headers, publicly-documented API surfaces (published OpenAPI/Swagger docs the org itself links to - not guessed paths).
4. **Triage** - which assets look highest-value (auth surfaces, anything handling user data) vs. low-value (static marketing pages) - this determines where you spend manual testing time, which happens outside this skill.
5. **Reporting** - see below.

## 4. What counts as "in scope" for this skill vs. not

**In scope (this skill helps with):**
- Building the asset inventory above
- Explaining a vulnerability class conceptually (e.g. "what is an IDOR and how would I recognize one while manually testing")
- Helping you write up a finding you already discovered through your own authorized manual testing
- Triaging severity for a finding you found (see rubric below)
- Drafting the submission itself

**Out of scope (this skill won't generate):**
- Regex catalogs or scripts built to hunt for live secrets/API keys in a target's exposed files
- Automated credential-stuffing/validation against discovered login endpoints
- Exploit code or step-by-step exploitation instructions for a specific CVE against a live target
- "Attack path" chains connecting multiple findings into a compromise narrative before you've actually verified each step yourself
- Cloud storage bucket or Kubernetes/CI-CD exposure sweeps

The line: describing how a vulnerability class works, generally, is security education (fine, and genuinely useful for triage/write-ups). Generating ready-to-run tooling aimed at a specific live target is the exploitation step, and that's excluded regardless of how authorized the engagement is - this skill can't verify your authorization, and the artifact would work the same way against an unauthorized target.

## 5. Severity triage (for findings you already have)

Use a simple rubric until your program specifies its own (many use CVSS):

- **Critical** - unauthenticated remote code execution, full account takeover at scale, mass PII exposure
- **High** - authenticated RCE, single-account takeover, significant data exposure requiring some precondition
- **Medium** - IDOR/broken access control on non-sensitive data, stored XSS in a low-privilege context, information disclosure
- **Low** - reflected XSS needing unusual interaction, verbose error messages, missing security headers with no demonstrated impact
- **Informational** - best-practice deviations with no direct security impact

Always report *demonstrated* impact, not theoretical worst-case - programs discount speculative severity.

## 6. Submission structure (HackerOne/Bugcrowd/Intigriti style)

1. **Title** - one line, specific (asset + vuln class + effect)
2. **Summary** - 2-3 sentences
3. **Steps to reproduce** - numbered, exact (URLs, parameters, request/response as needed)
4. **Impact** - what an attacker could actually do, grounded in what you demonstrated
5. **Suggested fix** (optional but appreciated)
6. **Supporting evidence** - screenshots/request logs, redacting any real user data you incidentally touched

See `reporting-templates.md` for a fuller client-report version (useful for private pentests vs. public bounty programs).
