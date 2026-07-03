# Daily Job Digest — Agent Instructions

Fresh session, no memory of prior runs. All state lives in `joshopp07/joshoppenheimer.com` on branch `claude/job-posting-digest-6pcrvg`.

**First:** check out that branch, read `job-search/config.json` and `job-search/seen.json` in full.

---

## Part 1: Job Listings

Search `sourcing.job_boards` for digital health / health AI postings within `posting_age_max_days`. Target VP/Head/Director/Chief roles in Product, Clinical/Medical Affairs, or BD/Partnerships/Sales where an MD/clinical background is a stated requirement or clear differentiator, and the role involves no direct ongoing patient care or clinical staff supervision.

Filter using config, in order: `exclude_employer_types` → `watchlist` include/exclude overrides → `product_mission_preference` → `clinical_domain_preference` → `travel_tolerance_pct`/`travel_exception` → `min_base_salary_usd` (missing salary ≠ exclude) → engagement type (fractional roles: pro-rate the salary floor, flag comp structure) → dedupe against `seen.json` (company+title or URL).

Score each new match 1–10 vs the `candidate` profile with 1–2 sentences of concrete reasoning. Be skeptical — fewer high-confidence matches beat a padded list. Check `watchlist.scoring_notes` for guidance on previously-seen companies.

---

## Part 2: Funding Alerts

Search `funding_alert_sources` for digital health / health AI raises ≥ `funding_alerts.min_raise_usd` within `funding_alert_age_max_days`. For each: check for an MD/DO on the founding team or leadership, apply `funding_alerts.logic` for priority, identify an outreach contact (founding CEO, lead VC partner, or known team member — name + LinkedIn if findable), and dedupe against `seen.json`.

---

## Part 3: Output and State

**Nothing new in either section:** reply with one low-key line (e.g. "No new listings or funding alerts today.") — no results file, no `seen.json` changes.

**Otherwise:**

1. Write `job-search/results/YYYY-MM-DD.md`:
   - **Job Listings**: title, company, score + reasoning, stage/funding, salary (or "not listed"), location/remote policy, travel note if relevant, direct apply link.
   - **Outreach Opportunities**: company, amount raised, round, lead investor(s), product description, MD-on-team (yes/no), suggested contact, 1-sentence rationale.
2. Append new entries to `seen.json` (company, title/announcement, url, date_found); prune entries older than 90 days.
3. Commit and push to `claude/job-posting-digest-6pcrvg` with a clear message.
4. Make your **final reply** the digest itself (this becomes the push notification): Job Listings ranked by score, then Outreach Opportunities (HIGH priority first) — each entry scannable in one line or two.

`config.json` is the source of truth throughout — use judgment, don't apply these rules mechanically.
