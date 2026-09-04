---
name: osint-ultimate
description: Comprehensive OSINT (open-source intelligence) toolkit covering domain/infrastructure recon (WHOIS/RDAP, DNS, certificate transparency, ASN/IP-range ownership, Shodan/Censys passive lookups), bug-bounty scoping and passive reconnaissance methodology, search-engine dorking and document-metadata forensics, social media investigation and cross-platform username verification, image/video geolocation verification (Bellingcat-style), corporate and business research (SEC/company-registry filings, ownership structure, employee/LinkedIn research), breach self-audit, email-authentication (SPF/DKIM/DMARC) posture auditing, and professional report writing. Use whenever the user wants to research a domain, IP range, company, username, or public online footprint; plan the recon phase of an authorized bug-bounty or pentest engagement; run a Google/search-engine dork or pull metadata from a public document; check public breach exposure; audit a domain's email-authentication records; verify a claim, photo, or video using OSINT techniques; or write up findings in a professional report. Trigger this for phrasing like "digital footprint audit," "recon on this domain," "who owns this company/IP," "find their public social accounts," or "check if my email's been breached," including when the target is the user's own accounts or organization.
---

# OSINT Ultimate

A router skill for open-source intelligence work. This skill is scoped to **passive, publicly-available-information research** - reading what's already public, not actively probing, exploiting, or breaching systems, and not locating or tracking a specific private individual.

## Hard boundaries (apply across every sub-skill)

1. **No exploitation.** This skill covers reconnaissance and reporting, not vulnerability exploitation. It does not include credential-testing tools, secret/API-key hunting against live third-party systems, exploit-to-CVE chaining, or "attack path" generation. If a task requires actually breaking into, authenticating against, or extracting data from a system you don't own, stop and say that's out of scope.
2. **No tracking of private individuals.** Nothing here is for locating a specific private person's home address, real-time location, phone number, or aggregating a dossier on someone who hasn't consented to that scrutiny. Social-media and image sub-skills are for verifying public claims/sources (journalism, research, brand protection) - not stalking, doxxing, or harassment.
3. **Scope check first.** For anything domain- or company-specific (recon, bug bounty), confirm with the user that they have authorization (they own the asset, or it's in-scope for a bug-bounty program / signed engagement) before proceeding. If they can't confirm that, keep the work to methodology/education rather than running it against a real named target.
4. **Attribute and don't overreach.** OSINT conclusions are provisional - cite sources, flag confidence level, and don't present inference as fact.

## Sub-skills

Read the relevant reference file(s) based on the task - don't load all of them for every request.

| Task | Reference |
|---|---|
| WHOIS/DNS history, subdomain discovery (passive sources only), ASN/IP-range ownership, Shodan/Censys passive lookups, tech-stack fingerprinting, certificate transparency | `references/domain-infra-recon.md` |
| Scoping a bug-bounty or pentest engagement, passive recon methodology, time-budgeting, findings severity, write-up structure | `references/bug-bounty-methodology.md` |
| Google/Bing dorking, cached/archived page retrieval, extracting metadata (author, software, revision history) from public PDFs/Office docs | `references/search-dorking-and-document-metadata.md` |
| Investigating/verifying public social media accounts, cross-platform username enumeration, sourcing claims, sock-puppet ethics, platform-specific search techniques | `references/social-media-osint.md` |
| Reverse image search, EXIF metadata, geolocation-from-photo/video verification (Bellingcat-style) | `references/image-geolocation-osint.md` |
| Company registries, SEC filings, ownership structure, employee/LinkedIn research, M&A/vendor due diligence | `references/corporate-business-research.md` |
| Checking your *own* email/accounts against public breach data | `references/breach-checking-selfaudit.md` |
| Public email-security posture audit (SPF/DKIM/DMARC) for a domain you own or are assessing with authorization | `references/email-security-audit.md` |
| Structuring findings into a client- or program-ready report | `references/reporting-templates.md` |

## Workflow

1. Clarify: what's the target (domain/company/account/image), what's the goal (bug bounty submission, journalism piece, due diligence, personal audit), and confirm authorization/consent where relevant.
2. Load the matching reference file(s) above.
3. Work passively - public records, public APIs, cached/archived pages, published metadata. Never attempt authentication, exploitation, or intrusive scanning.
4. Write up findings using `references/reporting-templates.md`, rating confidence per finding.
