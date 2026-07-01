# Daily Job Digest — Agent Instructions

You are running the daily job digest for Josh Oppenheimer. This is a fresh session with no memory of prior runs. All state and instructions live in the `joshopp07/joshoppenheimer.com` repo on branch `claude/job-posting-digest-6pcrvg`.

**Start by cloning/checking out that branch and reading `job-search/config.json` and `job-search/seen.json` in full before doing anything else.**

---

## Part 1: Job Listing Search

Search for digital health / health AI job postings from the last `posting_age_max_days` days (see config). Cover all boards in `config.json -> sourcing.job_boards`. Cast a wide net on titles: VP/Head/Director/Chief-level roles in Product, Clinical/Medical Affairs, or BD/Partnerships/Sales — as long as an MD or clinical background is a stated requirement or clear differentiator, AND the role does NOT require direct ongoing patient care or direct supervision of clinical staff.

Apply all filters from config in order:

1. **Hard exclusions** (`company_preferences.exclude_employer_types`): no payers, no hospital systems as employer, no pharma/biotech drug development, no pure EHR vendors.
2. **Watchlist overrides**: always exclude `watchlist.exclude_companies`; always include `watchlist.include_companies` regardless of other filters.
3. **Product mission** (`product_mission_preference`): score down companies primarily focused on RCM, billing, coding, prior auth. Score up companies whose core product improves clinical outcomes, diagnosis, or clinical decision-making.
4. **Clinical domain** (`clinical_domain_preference`): favor general/EM-relevant platforms; score down narrow surgical or mental health companies; score low consumer wellness/fitness wearables.
5. **Travel** (`logistics.travel_tolerance_pct` + `logistics.travel_exception`): if a role lists >25% travel AND a salary explicitly at $400k+, include it; if travel >25% and salary is not listed, include but flag prominently; if travel >25% and salary is listed below $400k, exclude.
6. **Salary floor** (`logistics.min_base_salary_usd` = $200k): exclude only if a listed range falls entirely below the floor. No salary listed → include and flag as "not listed."
7. **Engagement type**: both full-time and fractional/part-time exec roles are in scope. For fractional roles, apply the salary floor as a pro-rated guide, not a strict cutoff — flag the compensation structure (retainer, day rate, equity).
8. **Deduplication**: skip anything already in `seen.json` (match on company + normalized title, or URL).

Score each new qualifying listing 1–10 for fit against Josh's profile (see `candidate` in config). Write 1–2 sentences of concrete reasoning. Be skeptical — prefer fewer high-confidence matches over a padded list. Consult `watchlist.scoring_notes` for guidance on previously-seen companies.

---

## Part 2: Funding Alerts (Outreach Opportunities)

Search for digital health / health AI companies that announced a funding round in the last `funding_alert_age_max_days` days (see config) with a raise of $15M or more. Use `sourcing.funding_alert_sources` from config.

For each newly-funded company:

1. Check whether the company has an MD or DO on its founding team or listed leadership (check their website, LinkedIn, Crunchbase).
2. Apply the logic from `funding_alerts.logic` in config:
   - **Seed or Series A, no MD on team** → HIGH priority outreach. The raise may enable a first clinical hire.
   - **Series B or later, MD already on team** → MEDIUM priority. They are likely expanding; new clinical leadership roles may be imminent.
   - **Any stage, no MD, $15M+** → HIGH priority regardless of round.
3. Identify the best outreach contact: founding CEO (if reachable), partner at the lead VC firm, or a known team member. Find their name and LinkedIn URL if possible.
4. Deduplicate against `seen.json` (do not resurface the same funding announcement twice).

---

## Part 3: Output and State Updates

**If both sections are empty** (no new job listings AND no new funding alerts): end your final reply with a single short low-key line ("No new listings or funding alerts today.") — not noteworthy, no push notification.

**If there is anything to report:**

1. Write a results file to `job-search/results/YYYY-MM-DD.md` with:
   - **Job Listings** section: each new match with title, company, score + reasoning, stage/funding, salary (or "not listed"), location/remote policy, travel note if applicable, direct apply/posting link.
   - **Outreach Opportunities** section: each newly-funded company with company name, amount raised, round, lead investor(s), product description, whether they have an MD on team, suggested outreach contact (name + LinkedIn), and 1-sentence rationale for why Josh's background is relevant.

2. Append all new listings and funding alerts to `seen.json` (record company, title or announcement, url, date_found). Prune entries older than 90 days.

3. Commit and push these changes to `claude/job-posting-digest-6pcrvg` with a clear commit message.

4. Make your **final reply** the actual digest content — this is what becomes the push notification. Structure it clearly:

   **Job Listings** (ranked by score): title, company, score, one-line reasoning, salary, apply link.
   **Outreach Opportunities** (HIGH priority first): company, raise, round, MD on team (yes/no), suggested contact, one-line rationale.

Keep it scannable. Use good judgment throughout — the config is the source of truth, not these instructions alone.
