# Social Media OSINT - Verification & Public-Interest Research

Scope: verifying claims, sourcing stories, researching organizations' public presence, and brand/impersonation monitoring using information people or organizations have themselves made public. Not for locating, tracking, or building a dossier on a private individual.

## Before starting: who is the subject?

- **Public figure or organization** (company account, politician, journalist reporting in their public capacity, a brand) - this file applies normally.
- **Private individual** - stop and reconsider. Even fully public profile information, aggregated deliberately about one named private person, can amount to building a targeting dossier. If the user's stated goal is to locate, monitor, or compile information on a specific private person (an ex, a coworker, a stranger), don't proceed with this - that's true regardless of how the request is framed (e.g. "for safety," "to verify someone I'm dating," "background check"). Suggest legitimate channels instead (official background-check services with consent, employer HR processes, law enforcement if there's a genuine safety concern).

## Verification techniques (for public accounts/claims)

- **Account provenance**: creation date (many platforms show or algorithmically expose this), follower/following ratio and growth pattern, cross-posting patterns - useful for spotting inauthentic/bot accounts, not for profiling a real person's life.
- **Cross-platform corroboration**: does the same claim/photo appear from independent accounts, or does it trace back to one source being amplified? Chain of custody matters more than any single post.
- **Archived versions**: Wayback Machine / archive.today snapshots of a profile or post before it was edited or deleted - standard practice for verifying what was actually said before a correction.
- **Reverse-searching profile photos**: see `image-geolocation-osint.md` - useful for spotting fake/stolen-photo accounts (a common inauthentic-account signal), not for identifying a real person from a photo.
- **Metadata on posts**: timestamps, geotags the poster themselves attached (for verifying *when/where an event happened*, e.g. journalism fact-checking), language/timezone clues in posting patterns for authenticity assessment.

## Platform-specific public search

- Most platforms' own advanced search (date ranges, from:account, near:location for geotagged public posts) is the first stop - it's the platform surfacing its own indexed public content.
- Google/Bing site-restricted search (`site:twitter.com "phrase"`) for content a platform's own search misses or for deleted-but-cached posts.
- Aggregator tools (Social Searcher, etc.) - useful for brand-mention monitoring across platforms; note these only surface what's already public and indexed.

## Sock puppets / research accounts

If your organization's policy allows using a non-attributed research account to view public content (common in journalism and trust & safety work):
- Don't use it to send messages, follow/friend the subject, or otherwise interact - passive viewing only keeps this in OSINT territory rather than becoming a social-engineering approach.
- Don't impersonate a specific real person.
- Follow your organization's own ethics/legal guidance - platform ToS violations carry their own consequences independent of anything else.

## Brand/impersonation monitoring (legitimate business use)

- Searching for accounts using your company's name/logo/handle variants to catch impersonation or fraud - same techniques as above, applied to your own org's namespace.

## Source evaluation checklist (for any social-sourced claim)

1. Is this the original post, or a screenshot/repost that could be altered?
2. Does the account's history support authenticity (long-standing, consistent) or does it look purpose-created?
3. Can you find at least one independent corroborating source?
4. What's your confidence level - state it explicitly in any write-up rather than presenting inference as fact.
