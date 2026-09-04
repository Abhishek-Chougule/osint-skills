# Domain & Infrastructure Recon (Passive)

Scope: mapping a domain's public footprint using sources that don't touch the target's own servers with anything beyond a normal, single, unauthenticated request. No scanning sweeps, no brute-force probing, no credential testing.

## 1. WHOIS / RDAP

- Current registration: RDAP (`rdap.org` bootstrap) or a registrar's WHOIS lookup. Note registrar, creation/expiry date, registrant org (often redacted by GDPR/privacy proxies - that's expected, not a finding).
- Historical WHOIS (ownership changes over time) is available through paid services (DomainTools, WhoisXML) - mention these exist but don't require them; note when a finding depends on data you don't have access to.

## 2. Certificate Transparency (subdomain discovery)

- `crt.sh?q=%25.example.com` lists every publicly-issued TLS certificate for the domain, which is the single best passive subdomain source - every subdomain that's ever gotten an HTTPS cert shows up here, no probing required.
- Cross-reference with a second CT log viewer (e.g. Censys' certificate search) if crt.sh is slow/down.
- This produces a *list of names that exist*, not a statement about what's currently live - treat it as a candidate list, not a finding.

## 3. DNS (public records only)

- Standard record lookup (A/AAAA/MX/TXT/NS/CNAME) via any public resolver (`dig`, `nslookup`, or a web-based DNS tool) - this is just reading what the domain owner already published.
- SPF/DMARC records live in TXT - see `email-security-audit.md` for how to interpret them.
- Reverse DNS on IPs already discovered can reveal shared hosting or CDN usage.

## 4. Wayback Machine / archive.org

- `web.archive.org/web/*/example.com/*` (CDX API) surfaces historical URLs that were once public - useful for finding old pages, deprecated apps, or content the org no longer wants indexed but that they themselves published at some point.
- Treat archived content as historical context, not current attack surface - always note the capture date.

## 5. Technology fingerprinting

- Passive only: response headers, HTML meta generator tags, JS library file names, `robots.txt`, `humans.txt`, favicon hash (via Shodan's favicon search or similar) - all things the site serves to any normal visitor.
- Tools: Wappalyzer (browser extension or public API), BuiltWith.
- Do not send crafted/malformed requests to see how the server reacts - that's active probing, out of scope here.

## 6. Public records for the organization behind the domain

- SEC EDGAR (US public companies), Companies House (UK), OpenCorporates (aggregator across many jurisdictions) - see `corporate-business-research.md` for depth.

## 7. What's explicitly NOT in this file

- Subdomain brute-forcing / prefix wordlist sweeps against the live DNS server (that's active scanning, not passive OSINT)
- Port scanning or service banner grabbing
- Looking for exposed `.git`/`.env`/admin panels via crafted paths
- Cloud storage bucket enumeration (guessing bucket names to find open ones)
- Any secret/API-key regex hunting against the target's live JS bundles or repos with intent to use found credentials

If the user's goal actually requires those, that's the active/exploitation phase of a pentest - it needs its own explicit authorization scope and isn't something this skill will generate tooling for. Point them to their engagement's rules of engagement and, if applicable, standard licensed tools (Burp Suite, Nessus, etc.) run by the authorized tester themselves.

## Output shape

For each subdomain/asset found, note: source (crt.sh / wayback / etc.), first-seen date if known, and whether you actually verified it currently resolves (a simple DNS lookup, not a scan) vs. it's just a historical name. Keep speculation about "attack surface" out of a pure recon note - that's an assessment step, not a listing step.
