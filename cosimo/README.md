# Cosimo research intake

Adopted component: TrendRadar RSS fetching/parsing and daily SQLite history, at upstream
`86584f5544c66d4e22132f9b87c44a7ea3a32fea` (6.11.0). Cosimo's fork is
https://github.com/Cosimo-Medici/TrendRadar, branch `cosimo/research-intake`.

## Operation

Install the locked upstream environment with `uv sync --frozen` in this checkout.
The existing content engine now invokes `.venv/bin/python cosimo/collect.py` before
its commentary research stage, through `content-engine/lib/research-intake.cjs`.
It passes the live approved source catalog and writes a per-run
`research-discovery.json`. The existing researcher, critic, topic ledger and ranking
remain responsible for selecting arguments. No model or notification is invoked
by this collector, and there is no separate daemon or schedule.

The adapter accepts `--catalog`, `--state` and `--output` absolute paths. Production
state defaults through the bridge to `~/Library/Application Support/cosimo-content/research-intake`;
`COSIMO_CONTENT_HOME` relocates it with the rest of the writing engine. Optional
`COSIMO_TRENDRADAR_ROOT` relocates this installed checkout. Each collection saves a
receipt under `runs/`, upstream SQLite files under `history/rss/`, and an atomic latest
snapshot. Receipts include source hashes. Collection runs under a file lock.

## What is reused

Upstream fetches and parses RSS/Atom/JSON feeds and persists history with GUID/URL
upserts. Cosimo supplies the approved catalog, source checks, a seven-day export,
conservative URL normalization, cross-feed deduplication, and up to eight recent
leads per source. The source cap bounds researcher input; full metadata stays in
SQLite. No generic crawler, RSS parser or history database has been rebuilt.
This fills a previously absent collection layer; it does not retire the existing
editorial research and ranking code.

Ten feeds returned usable entries in the live trial: SEC, OCC, Fed, FCA, FT Markets,
FT Technology, The Diff, Net Interest, TechCrunch AI, OpenAI News. FCA's returned
entries lacked dates, so they are retained in history but excluded from the recent
export. Three PEI `/feed/` probes returned HTML, not RSS. Those and other catalog
sources remain on scoped web discovery. `feeds.json` records the unavailable probes.
A successful feed fetch is not full article access; paywalls remain paywalls.

## Local upstream fix

`trendradar/crawler/rss/parser.py` now retains UTC on feedparser date tuples.
Feedparser explicitly normalizes these tuples to UTC; upstream constructed a naive
datetime and discarded that information. Without the fix every otherwise dated
RSS item failed the strict freshness check. Missing dates remain missing.
Reference: https://feedparser.readthedocs.io/en/latest/date-parsing/

## Evidence and failure behavior

Titles/summaries are untrusted discovery hints. Publication dates are feed claims;
event dates remain unknown. Open original articles and apply the existing reader,
interest, argument and eight-week-repeat checks. Collection counts never establish
buyer demand, audience engagement or independent momentum. Founder commentary
stays nonpromotional; narrow operations stories belong to the company line.

The bridge has a four-minute collection ceiling. Missing installation, collection
failure, invalid data or a stale/future snapshot produce an explicit coverage gap;
the existing scoped web researcher continues. Historical replays do not fetch
current news. Other content lines do not invoke this collector. No mail, publisher,
CRM, webhooks or upstream hot-list/AI analysis paths are used.

## Ownership and cost

Upstream and this adapter retain the repository's GNU GPL v3 license and notices.
No vendor software subscription or model API call is needed for this collection
path. Running hardware/network and any separately licensed article access remain
separate. Preserve the license and corresponding source requirements when
redistributing this modified code; do not describe this as a noncommercial exemption.
The writing engine communicates through a separate process and a JSON file.

## Updating / rollback

Review changes before moving the revision pin in both collector and bridge. Retain
the UTC fix or verify its upstream replacement. Re-run live collection, duplicate
persistence checks, and the bridge's invalid/stale/unavailable handling. Do not run
the complete weekly writing workflow as an integration test.

To remove the integration, remove the `discoveryInputs` hook from `runResearch` and
the matching researcher prompt paragraph, then remove `lib/research-intake.cjs`.
No scheduled job, existing topic ledger or publication queue needs changing.

## Verification receipts

Local live evaluation receipts and the bridge output are at
`../private/trendradar-intake/`. They include pre-change copies of the existing
pipeline/prompt, collection receipts, SQLite readback and verification results.
Full weekly content generation was not run for this collector check; ENG-820's
two consecutive weekly observations remain outstanding. The earlier Friday pause
is historical: the approved Friday post subsequently published through BrightBean.
Use the current ten-system Linear checklist for publishing and other acceptance.
