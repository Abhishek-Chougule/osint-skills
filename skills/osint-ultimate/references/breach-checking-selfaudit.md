# Breach Checking - Personal / Own-Organization Self-Audit

Scope: checking whether *your own* email addresses, or accounts belonging to an organization you're authorized to assess, appear in known public data breaches. Not for checking a third party's email/accounts without their authorization.

## How to check

- **Have I Been Pwned (HIBP)** - the standard public breach-notification service. The website (`haveibeenpwned.com`) lets you search a single address for free, no account or key needed. The *API* (for programmatic/repeated lookups) requires a paid API key as of HIBP's current terms - if a task calls for checking more than a handful of addresses you administer, mention this rather than assuming a free scripted path exists. "Notify me" is HIBP's free subscription for ongoing monitoring of your own address.
- **Mozilla Monitor** (monitor.mozilla.org, formerly Firefox Monitor) - also backed by HIBP data, free single-address lookup with a simpler ongoing-monitoring signup; a fine alternative front-end to the same underlying data.
- **Browser built-ins** - Chrome, Firefox, and Safari all have built-in breached-password checking against your saved passwords - often the fastest path for a personal audit.
- **Domain-wide check** - if you administer a company domain, HIBP supports domain-level breach search after a domain-ownership verification step (proves you actually control the domain before showing results) - this is the intended path for a company self-audit, not for checking someone else's domain.
- **Paid third-party breach-data services** (DeHashed, Intelligence X, and similar) exist and are sometimes used in authorized incident-response/security engagements to see actual leaked credential values (not just "was this address in a breach"). These are a different tier than the self-audit tools above - only relevant when the user has an explicit authorized-security-engagement context, and even then, treat any credentials surfaced as something to rotate/remediate, never to test/use.

## What to do with a positive result

1. If a breach exposed a password, and you've reused that password anywhere, change it everywhere it was reused - this is usually the highest-value action.
2. Enable MFA on the affected account and any account sharing that password.
3. For a breach exposing more than credentials (SSNs, financial data), follow the breached organization's own guidance and consider a credit freeze/monitoring where relevant.
4. For an organizational self-audit, this feeds into the "legacy/decommissioned system exposure" pattern - an old breach involving an address on infrastructure you've since retired is lower urgency than one on an active system; note this distinction in any write-up rather than treating every hit as equally severe.

## Boundary

Checking a specific *other* person's email against breach databases - even public ones - without their consent or a clear authorized-security-assessment relationship crosses from "self-audit" into researching a private individual. HIBP's own terms restrict bulk/third-party lookups for this reason. If the request is about checking someone else's exposure, redirect to: they should check it themselves, or (for an employee within an org you administer) go through the domain-verified organizational flow above rather than looking up their personal address directly.
