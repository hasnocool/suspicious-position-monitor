# TODO — Suspicious Position Monitor

This file is the implementation source of truth. Update it when work starts, scope changes, blockers appear or acceptance criteria are met.

## Current state
- [x] Verify repository exists and was empty before initialization.
- [x] Establish Python/Rust architecture baseline.
- [x] Document market, news and public data source classes.
- [x] Define separate top-500 public-interest and top-500 wealth cohorts.
- [x] Define evidence-based wallet attribution policy.
- [ ] Implement runtime code.

## M0 — Repository and contracts
- [ ] Create Python 3.12+ package structure.
- [ ] Create Rust workspace and crates.
- [ ] Add common-envelope, market-fill, position-snapshot/change, news-item, event, person, filing, wallet-attribution and anomaly schemas.
- [ ] Add matching Python and Rust schema/fixture tests.
- [ ] Add PostgreSQL migrations and local configuration.
- [ ] Add structured logs, metrics, health checks and graceful shutdown.
- [ ] Add local format/lint/test/audit scripts.
- [ ] Add .env.example with placeholders only.
- [ ] Add .gitignore for secrets, databases, archives, downloaded documents and generated data.

## M1 — Hyperliquid
- [ ] Discover perpetual DEXes and market metadata dynamically.
- [ ] Discover HIP-3 stock-linked markets and configurable crypto markets.
- [ ] Implement resilient WebSocket trades collector.
- [ ] Persist public participant addresses as unattributed wallets.
- [ ] Implement account fill-history and clearinghouse-state queries.
- [ ] Reconstruct short/long position changes from fills.
- [ ] Reconcile reconstructed positions against snapshots.
- [ ] Handle partial fills, duplicate/out-of-order fills, reversals, reduce-only fills, liquidation and reconnect gaps.
- [ ] Add replay fixtures and market-data quality metrics.

## M2 — News and incidents
- [ ] Implement configurable Google News RSS feeds.
- [ ] Implement GDELT DOC JSON adapter and event-file importer.
- [ ] Implement CISA advisory/KEV feeds.
- [ ] Implement cloud/CDN/DNS/exchange status adapters.
- [ ] Add canonical URL and content-hash deduplication.
- [ ] Extract companies, people, tickers, jurisdictions and incident categories.
- [ ] Cluster articles into events with independent-source counts.
- [ ] Investigate currently supported authenticated Reddit access; do not depend on RSS.
- [ ] Add source-health and staleness monitoring.

## M3 — Anomaly detection
- [ ] Add per-wallet/per-asset historical baselines.
- [ ] Add robust notional and position-change anomaly metrics.
- [ ] Add leverage, funding, liquidity and concentration signals.
- [ ] Add issuer/sector/event mapping.
- [ ] Add explainable score versions and thresholds.
- [ ] Add alternate explanations and data-quality/confounder flags.
- [ ] Backtest using only information available at the time.

## M4 — Public figures and disclosures
- [ ] Define transparent watchlist scoring and geographic/category balancing.
- [ ] Build stable person/entity IDs and alias resolution.
- [ ] Integrate official US Senate eFD source.
- [ ] Review and implement US House source only for permitted use.
- [ ] Review OGE access and agency coverage.
- [ ] Integrate UK Register of Members' Financial Interests API.
- [ ] Review Canada's official registry access and reuse rules.
- [ ] Add country adapter template and source-license gates.
- [ ] Extract disclosures with page/section/row evidence.
- [ ] Add human review for name collisions and ambiguous extraction.

## M5 — Wealth and attribution
- [ ] Integrate optional Forbes-derived wealth-list provider.
- [ ] Discover list IDs from provider metadata and snapshot top 500.
- [ ] Preserve rank/net-worth history and provider update timestamps.
- [ ] Deduplicate people across cohorts.
- [ ] Add explicit documentary wallet-attribution workflow.
- [ ] Block heuristic name/wallet matches from verified status.
- [ ] Add correction, dispute and withdrawal handling.

## M6 — Dashboard and reports
- [ ] Build market and wallet watchlists.
- [ ] Build wallet position timelines and fill tables.
- [ ] Build news and event timelines.
- [ ] Build people, roles, disclosures and wealth pages.
- [ ] Build evidence viewer and exportable reports.
- [ ] Add analyst annotations and alert review.
- [ ] Add historical event studies and backtest views.

## Operating rules
- No automated trading.
- No person-wallet deanonymization or guessed attribution.
- No commercial use of restricted sources without appropriate permission.
- Every alert has source evidence and an uncertainty section.
- Update this TODO when work completes; include test evidence and remaining limitations.
