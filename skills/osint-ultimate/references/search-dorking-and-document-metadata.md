# Search-Engine Dorking & Document Metadata

Scope: using search engines' own advanced operators to surface publicly indexed content a normal query would miss, and reading the metadata already embedded in public documents. Both are reading what's already published/crawled - no requests are sent to the target beyond fetching a page or file that's already publicly linked and indexed.

## 1. Advanced search operators ("dorking")

All major engines (Google, Bing, DuckDuckGo) support operators that narrow a search rather than probe a target:

- `site:example.com` - restrict results to one domain; combine with keywords to find specific public content (e.g. `site:example.com "confidential"` to check if a sensitive-sounding page was ever indexed - this finds *indexed, published* pages, not a scan for hidden ones).
- `filetype:pdf` / `filetype:xlsx` / `filetype:docx` - find publicly linked documents of a given type on a domain, e.g. `site:example.com filetype:pdf`.
- `intitle:` / `inurl:` - narrow by page title or URL text (e.g. `inurl:investor-relations` to find a company's IR pages).
- `"exact phrase"` - useful for tracing where a specific sentence/claim originated or was copied from.
- `-keyword` - exclude noise terms.
- `before:`/`after:` (Google) or date-range filters (Bing) - narrow to a publication window, useful when corroborating when a claim first appeared.

**What this is for:** finding a company's published org chart PDF, an old press release, a public API doc, a conference-talk slide deck, a leaked-but-since-removed page still in a search index. It surfaces content the organization itself published and a crawler indexed.

**What this is not for:** operators aimed at finding exposed credentials, admin panels, open directories of sensitive files, or database dumps (`intitle:"index of" password`, `inurl:admin`, `filetype:env`, "Google hacking database"-style vulnerability dorks) are exploitation reconnaissance, not passive research - they're excluded here the same way port scanning is (see `bug-bounty-methodology.md` §4 and `domain-infra-recon.md` §7). If a dork's purpose is to locate a live system's secrets or entry points rather than an organization's own published content, don't run it.

## 2. Cached and archived pages

- Search engines' own "cached page" links have mostly been retired; the reliable path is `web.archive.org` (Wayback Machine) - see `domain-infra-recon.md` §4 for the CDX API approach to bulk-listing archived URLs.
- `archive.today` (archive.ph) is a manual-submission archive - useful when someone has archived a specific page/post themselves (common for verifying a since-deleted social media post or news article).
- Always note the capture date next to anything sourced from an archive - a page's content at capture time may differ from what a live fetch would show.

## 3. Document metadata (PDFs, Office files, images)

Public documents carry embedded metadata that's often more revealing than their visible content - author names, internal usernames, software/OS versions, template origins, and (for files that went through revisions) prior-draft text.

- **`exiftool`** - the single best tool here; works across PDF, Office (docx/xlsx/pptx), and image formats. Run it against any publicly downloaded file: `exiftool document.pdf`.
- **PDF-specific fields worth checking**: `Author`, `Creator`/`Producer` (reveals the software - e.g. a specific version of Word or a scanner model), `CreationDate`/`ModDate`, and sometimes embedded prior form-field data.
- **Office (docx/xlsx/pptx) files**: these are zip archives - `docProps/core.xml` and `docProps/app.xml` inside hold author, last-modified-by, company, and total-edit-time fields even without running a separate tool (unzip and read the XML directly if `exiftool` isn't available).
- **Common findings**: an internal employee's Windows username or email in the `Author`/`Last Modified By` field of a document meant to look anonymous; a template's "Company" field revealing the actual originating organization behind a shell entity; a `Producer` string revealing what internal system generated a report.
- **Caveat**: like EXIF on images (see `image-geolocation-osint.md`), document metadata is one corroborating data point, not proof - it can be stripped, edited, or inherited from a template and not reflect the actual current author.

## 4. Combining with other sub-skills

- Corporate research: a vendor's public PDF (pricing sheet, SOC 2 summary, org chart) often has metadata worth checking before/alongside the registry lookups in `corporate-business-research.md`.
- Domain recon: dorking a domain for `filetype:pdf`/`filetype:xlsx` is a normal enrichment step alongside `domain-infra-recon.md`'s other passive sources - it's still "read what's already public," just via a different index.
- Bug bounty: if a program's scope explicitly allows it, dorking a target's own domain for publicly indexed (not hidden) documentation is fair game as recon enrichment - never as a way to hunt for accidentally-exposed secrets (that's excluded per `bug-bounty-methodology.md`).

## Output shape

For any dork result or metadata finding, note: the exact query/tool used, the source URL and its index/capture date, and the specific field or content that's the finding (not just "found something interesting"). Treat a single metadata field as a lead to corroborate, not a conclusion on its own.
