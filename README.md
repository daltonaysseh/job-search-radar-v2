# job-search-radar-v2

Weekly NYC equity research / buy-side job tracker (entry-level only). See `tracker.html` for the current list, `postings.json` for the structured data, and `seen_postings.json` for the dedup log used to avoid re-researching already-classified postings.

## Scan policy notes for future runs

- Once a posting's `date_posted` and entry-level status are determined, that classification is permanent — never re-research an already-classified posting (efficiency requirement, this runs weekly not daily).
- **Exception: Workday-hosted postings** (`*.myworkdayjobs.com`). Workday deep-links can go blank/stale once a requisition closes — the URL stays reachable but shows no content, unlike a clean 404. So Workday postings get extra handling both directions:
  - **When adding a new Workday posting:** don't treat a non-empty fetch of the direct job URL as sufficient confirmation. Also cross-check that the job still appears in the company's live Workday search results (search the site/board for the exact title) before adding it to `postings.json`.
  - **On every subsequent run:** re-verify each Workday posting already in `postings.json` against the company's live search results (unlike every other source, these are NOT exempt from re-checking). If a previously-added Workday posting no longer appears in live search results, remove it from `postings.json` and `tracker.html` — but leave its entry in `seen_postings.json` so it isn't mistakenly re-added if it briefly reappears.
