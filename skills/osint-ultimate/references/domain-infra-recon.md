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

## 4. ASN / IP-range & network ownership

- Every IP address belongs to an Autonomous System (ASN) registered to an organization - `bgp.he.net`, RIPEstat, or a regional registry lookup (ARIN/RIPE/APNIC/LACNIC/AFRINIC WHOIS) shows the ASN, its full announced IP ranges, and the registrant org for any IP you've already found (e.g. from an A record or MX record).
- This is how you go from "one subdomain resolves to 203.0.113.4" to "that whole /24 belongs to the organization's own hosting, not a shared cloud provider" - useful for scoping what's actually the org's own infrastructure vs. third-party/CDN-fronted.
- `ipinfo.io` or similar give a quick org/ASN/geolocation summary for a single IP without needing the full registry lookup.
- A domain resolving to a range registered to a major cloud provider (AWS, Cloudflare, GCP) tells you it's shared infrastructure - don't attribute the whole IP range to the target org in that case, just the specific host.

## 5. Shodan / Censys (passive - reading existing scan data)

- Shodan and Censys continuously scan the public internet themselves and publish what they find (open ports, service banners, TLS certs, exposed panels) - querying their database reads results they already collected, the same way crt.sh reads certificates someone else logged. You are not sending any probe to the target by searching these services.
- Useful queries: `shodan.io` search by hostname/domain, by the IP ranges found via the ASN lookup above, or by favicon hash (for finding other assets served by the same login-panel template). Censys' certificate and host search overlaps with crt.sh but sometimes has broader banner/service detail.
- Treat what these show as a *point-in-time snapshot from their last scan*, not a live-verified current state - note the scan/"last seen" date next to any finding, the same as you would for a Wayback capture.
- This is for building an inventory (what services/versions does this org appear to expose) - it is not a substitute for, and should not be presented as, a live vulnerability scan. Don't extrapolate a banner/version match into "this is vulnerable to CVE-X" without the target's own confirmation or a licensed scanner's authorized output.

## 6. Wayback Machine / archive.org

- `web.archive.org/web/*/example.com/*` (CDX API) surfaces historical URLs that were once public - useful for finding old pages, deprecated apps, or content the org no longer wants indexed but that they themselves published at some point.
- Treat archived content as historical context, not current attack surface - always note the capture date.

## 7. Technology fingerprinting

- Passive only: response headers, HTML meta generator tags, JS library file names, `robots.txt`, `humans.txt`, favicon hash (via Shodan's favicon search or similar) - all things the site serves to any normal visitor.
- Tools: Wappalyzer (browser extension or public API), BuiltWith.
- Publicly linked documents (`filetype:pdf`/`filetype:xlsx` dorks) and their embedded metadata are also fair game as passive enrichment here - see `search-dorking-and-document-metadata.md`.
- Do not send crafted/malformed requests to see how the server reacts - that's active probing, out of scope here.

## 8. Public records for the organization behind the domain

- SEC EDGAR (US public companies), Companies House (UK), OpenCorporates (aggregator across many jurisdictions) - see `corporate-business-research.md` for depth.

## 9. What's explicitly NOT in this file

- Subdomain brute-forcing / prefix wordlist sweeps against the live DNS server (that's active scanning, not passive OSINT)
- Port scanning or service banner grabbing done by you directly against the target (Shodan/Censys' own pre-collected results, per §5, are the passive substitute for this)
- Looking for exposed `.git`/`.env`/admin panels via crafted paths
- Cloud storage bucket enumeration (guessing bucket names to find open ones)
- Any secret/API-key regex hunting against the target's live JS bundles or repos with intent to use found credentials

If the user's goal actually requires those, that's the active/exploitation phase of a pentest - it needs its own explicit authorization scope and isn't something this skill will generate tooling for. Point them to their engagement's rules of engagement and, if applicable, standard licensed tools (Burp Suite, Nessus, etc.) run by the authorized tester themselves.

## Output shape

For each subdomain/asset found, note: source (crt.sh / wayback / etc.), first-seen date if known, and whether you actually verified it currently resolves (a simple DNS lookup, not a scan) vs. it's just a historical name. Keep speculation about "attack surface" out of a pure recon note - that's an assessment step, not a listing step.
