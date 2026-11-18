# Roadmap

Each milestone requires tests, documented source limitations and evidence of completion before the next layer is considered production-ready.

## M0 — Repository and contracts
Goal: project foundation.
- Python 3.12+ package and Rust workspace.
- Shared JSON Schemas and fixtures.
- PostgreSQL schema/migrations and local configuration.
- Source registry and source-health model.
- Logs, metrics, graceful shutdown and bounded async queues.
- Local developer scripts; no GitHub Actions required.

Acceptance: both languages validate the same fixtures; services start/stop cleanly; no secrets/runtime data committed.

## M1 — Hyperliquid market monitor
Goal: reconstruct public stock-perpetual position changes.
- Discover DEXes and market metadata.
- Subscribe to configured HIP-3 stock and selected crypto markets.
- Capture/deduplicate market trades and observed participant addresses.
- Retrieve account fills and clearinghouse snapshots.
- Reconstruct signed positions with gap/reconciliation status.
- Persist market events and position snapshots.
- Add volume, open-interest, funding and liquidity metrics.

Acceptance: replay fixtures prove open/add/reduce/close/flip handling; snapshot reconciliation is measured; reconnect gaps are recovered or clearly flagged.

## M2 — News and incidents
Goal: establish a reliable event timeline.
- Google News RSS general and query feeds.
- GDELT DOC and selected event files.
- CISA advisory/KEV ingestion.
- Cloud/CDN/DNS/exchange status feeds.
- Source registry, health, retries, deduplication and timestamps.
- Entity/ticker extraction and event clustering.
- Reddit adapter through an approved authenticated route; legacy RSS disabled.

Acceptance: duplicate/syndicated articles cluster; publication/first-seen/retrieval times remain distinct; one source outage does not stop other feeds.

## M3 — Anomaly and event correlation
Goal: prioritize meaningful position changes.
- Robust wallet/asset baselines.
- Notional and concentration anomalies.
- Leverage/funding/liquidation features.
- Event-to-issuer/sector mapping.
- Configurable event-time windows.
- Explainable score components and alternate explanations.
- Analyst review and evidence export.

Acceptance: deterministic scoring fixtures, historical replay, no look-ahead leakage and documented false-positive review.

## M4 — Public people and disclosures
Goal: reliable public-interest and disclosure records.
- Official adapters for US House, Senate, OGE, UK Parliament and Canada.
- License/usage gates.
- Public-role discovery via official sources and Wikidata.
- Optional OpenSanctions enrichment only under a suitable license.
- Versioned top-500 public-interest selection.
- Traceable filing extraction with page/section references.
- Human review of ambiguous identities and extracted facts.

Acceptance: stable IDs and source links; every extracted fact has a document locator; restricted sources cannot enter disallowed deployments.

## M5 — Wealth top-500 and wallet evidence
Goal: separate dated wealth cohort.
- Integrate optional Forbes-derived wealth-list provider.
- Discover list identifiers dynamically.
- Snapshot top 500 and preserve ranking history.
- Deduplicate across cohorts.
- Human review for corrections and aliases.
- Explicit documentary wallet-attribution workflow.

Acceptance: provider schema changes fail safely; ranking snapshots are reproducible; heuristic matches cannot become verified links.

## M6 — Dashboard and reports
Goal: usable research interface.
- Market/position watchlists.
- Wallet pages and position timelines.
- News/event timeline with source links.
- Public figure and wealth-list pages.
- Disclosure document/evidence viewer.
- Alert review, annotations and exportable reports.
- Search by asset, wallet, person, company, incident and date.
- Historical event studies/backtests.

Acceptance: each alert traces to source fills, snapshots, news and documents.

## M7 — Additional venues and jurisdictions
- Add dYdX, Drift, GMX and selected EVM adapters.
- Add country disclosure adapters after legal/source review.
- Add regional news and official incident feeds.
- Improve cross-venue asset mapping.
- Add source coverage dashboards.

Acceptance: every adapter passes contract, replay, source-health and license tests before default enablement.
