# Job Posting Digest

Automated daily scan for digital health / health AI roles that fit Josh Oppenheimer's
background, delivered as a push notification each morning.

## How it works

A scheduled trigger fires once a day into a **fresh** Claude Code session (no memory of
prior runs). That session:

1. Checks out this repo on branch `claude/job-posting-digest-6pcrvg` and reads
   `config.json` (matching criteria) and `seen.json` (dedup state).
2. Searches job boards for postings from the last 7 days.
3. Filters against the criteria in `config.json` (function, company type, salary floor,
   exclusions) and removes anything already in `seen.json`.
4. Scores each new match 1-10 with a short reasoning, and includes a direct apply link.
5. If there's nothing new, it stays silent — no notification.
6. If there are new matches, it writes a dated digest to `results/YYYY-MM-DD.md`, appends
   the listings to `seen.json`, commits + pushes, and the final reply (which becomes the
   push notification) is the digest itself.

## Tuning the search

Edit `config.json` directly:

- `role_preferences` — which functions/titles count as a fit, and what disqualifies a role
  (e.g. direct patient care).
- `company_preferences` — stage, sector, hard exclusions (payers, hospital systems,
  pharma/biotech, EHR vendors), and clinical domain leanings.
- `logistics` — location, travel tolerance, minimum base salary.
- `sourcing` — which boards to check and how fresh a posting must be.
- `watchlist` — add company names to `include_companies` (always surface) or
  `exclude_companies` (never surface), as you learn what you actually want.

Changes take effect on the next scheduled run — no need to touch the trigger itself.

## State files

- `seen.json` — listings already surfaced, used for de-duplication. Entries older than
  90 days are pruned automatically.
- `results/YYYY-MM-DD.md` — full archive of each day's new matches.

## Schedule

Runs daily around 8:00am ET (`0 12 * * *` UTC). Adjust via the trigger if the timing drifts
across daylight saving changes.
