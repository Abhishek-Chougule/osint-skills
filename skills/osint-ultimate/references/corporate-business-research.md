# Corporate & Business Research

Scope: researching organizations, ownership structures, and public filings for due diligence, vendor risk assessment, journalism, or competitive/market research.

## Public company filings

- **SEC EDGAR** (US) - 10-K/10-Q (financials, risk factors), 8-K (material events), DEF 14A (proxy statements - executive comp, board composition), Schedule 13D/G (large shareholders), Form 4 (insider trading).
- **Companies House** (UK) - filing history, officers, persons with significant control (PSC register), charges/mortgages against the company.
- **OpenCorporates** - aggregates company registry data across 140+ jurisdictions; good starting point for non-US/UK entities.
- Country-specific registries as needed (e.g. Germany's Handelsregister, Rusprofile for Russia, GSXT for China) - note that data completeness/reliability varies a lot by jurisdiction.

## Ownership & corporate structure

- Cross-reference registry filings across jurisdictions to trace parent/subsidiary relationships - useful for understanding who actually controls a vendor or counterparty.
- Trademark/patent filings (USPTO, WIPO, EUIPO) can reveal related entities or brand ownership not obvious from the registry alone.
- Domain WHOIS history (see `domain-infra-recon.md`) can corroborate corporate-family relationships (shared registrant orgs across a portfolio of domains).

## News & litigation

- Court records (PACER for US federal courts, or national equivalents) for litigation history.
- News archive search for past controversies, regulatory actions, executive departures - corroborate with at least two independent outlets before treating as established fact.
- Sanctions/watchlist screening (OFAC SDN list, EU sanctions list, UN Security Council list) for compliance-driven due diligence.

## Vendor / M&A due diligence checklist

1. Legal entity verification - does the registered name/jurisdiction match what the vendor represents?
2. Financial health signals - filed financials if public, or indirect signals (job posting volume/velocity, glassdoor/employee review trends, news of layoffs or funding rounds) if private.
3. Key people - leadership bios (company site, public LinkedIn, press coverage), any past regulatory or legal issues tied to named individuals *in their professional/corporate capacity* (this file doesn't cover personal-life research on individuals - see the private-individual guardrail and the LinkedIn/employee-research section in `social-media-osint.md`).
4. Technology footprint (see `domain-infra-recon.md`) - informs both security posture questions to ask them and integration complexity.
5. Public sentiment - review aggregators, news, social mentions - flag single-source claims as unverified.

## Output shape

Structure findings by category (legal entity, financials, litigation, technology, reputation) with a source and date for every claim. Explicitly flag anything you couldn't verify rather than omitting the gap.
