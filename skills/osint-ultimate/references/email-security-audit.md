# Email Security Posture Audit (Public DNS Records)

Scope: reading a domain's own published email-authentication records to assess spoofing/phishing risk. This is reading public DNS TXT records - the same thing any mail server does when deciding whether to trust an email claiming to be from that domain. Applies to a domain you own or are assessing with authorization (client engagement, vendor security review).

## Records to check

- **SPF** (`v=spf1 ...` TXT record) - which servers are authorized to send mail for the domain. Look for: overly permissive mechanisms (`+all` is a red flag - allows anyone to spoof), too many DNS lookups (SPF has a 10-lookup limit; exceeding it breaks validation), stale includes pointing at decommissioned services.
- **DKIM** - published as `selector._domainkey.example.com` TXT records; the selector name is often guessable from common providers (`google`, `k1`, `s1`, `mandrill`, etc.) or found in a sample email's headers. Check key length (1024-bit is weak by current standards; 2048-bit is standard).
- **DMARC** (`_dmarc.example.com` TXT) - the policy (`p=none/quarantine/reject`), reporting addresses (`rua=`/`ruf=`), and alignment mode. `p=none` means the domain isn't actually protected against spoofing yet, just monitoring - a common and worth-flagging gap.
- **BIMI** (`default._bimi.example.com`) - brand logo display; requires DMARC enforcement first, so its presence without a strong DMARC policy is inconsistent.
- **MTA-STS / TLS-RPT** - enforce/report on TLS transport security for inbound mail; absence isn't unusual but worth noting as a hardening opportunity.

## What to infer from MX records

- MX records reveal the mail provider (Google Workspace, Microsoft 365, ProtonMail, self-hosted) which contextualizes what "normal" SPF/DKIM setup should look like for that provider.
- A mismatch (e.g. MX pointing to Microsoft 365 but SPF only including an unrelated marketing platform) can indicate a misconfiguration or a forgotten legacy sender.

## Reporting a finding

State the record as found (quote the actual TXT value), what's wrong with it, and the concrete fix (e.g. "change `p=none` to `p=quarantine` after confirming legitimate senders are all covered by SPF/DKIM, then to `p=reject`"). This is a config-hardening recommendation, not an exploit - appropriate to hand directly to the domain owner.
