# Architecture

## 1. Operating model

Start as a local-first, single-operator research system. The initial deployment should run on one workstation and tolerate internet interruptions, source outages and process restarts. Services should be separable so collectors can later run on different machines without changing event contracts.

The layers are:
1. Collectors: exchange messages, blockchain events, feeds, filings and ranking snapshots.
2. Normalization: provider payloads into shared domain events.
3. State/history: immutable observations, positions, people, evidence and event timelines.
4. Analysis: unusual-activity scoring and event correlation without inferring intent.
5. Research interface: watchlists, timelines, wallets, sources, alerts and evidence reports.

## 2. Python services

Python owns source adapters and research workflows because feeds and documents require provider-specific parsing and iterative extraction.

Target packages:
- collectors.news.google_news: Google News RSS feeds and query management.
- collectors.news.gdelt: GDELT DOC API and event-file ingestion.
- collectors.news.reddit: authenticated Reddit adapter; legacy endpoint probes disabled by default.
- collectors.news.status_feeds: CISA, cloud providers, exchanges and public incident feeds.
- collectors.disclosures.us_house, us_senate, us_oge, uk_parliament, canada: jurisdiction-specific adapters.
- collectors.rankings.wealth: optional public wealth-list providers.
- entities.wikidata: discovery of public roles, identifiers and aliases.
- documents: PDF/HTML/CSV/XML parsing, content hashes and page/section references.
- research: notebooks, source comparisons and backtest preparation.

Use Python 3.12+, asyncio, async HTTP clients, Pydantic boundary validation and an async PostgreSQL driver. Blocking PDF/OCR work must run in a bounded process worker, not in the event loop.

## 3. Rust services

Rust owns high-throughput, long-lived and deterministic core processing.

Target crates:
- domain: canonical IDs, enums, decimal financial types and domain objects.
- market-stream: WebSockets, reconnects, subscriptions, heartbeat and market metadata.
- position-engine: fill deduplication, signed-position reconstruction and snapshot reconciliation.
- event-engine: event handling, time windows, correlation and explainable anomaly scoring.
- storage: async PostgreSQL access, migrations, outbox and Parquet export.
- api: Axum HTTP/WebSocket API for the dashboard and research clients.

Use Tokio, reqwest, tokio-tungstenite, serde, sqlx, tracing and rust_decimal or another audited decimal implementation. Do not use floating point for money/position arithmetic.

## 4. Cross-language contracts

The shared contract lives in schemas/.
- JSON Schema is authoritative initially.
- Python validates with Pydantic models generated or tested against schemas.
- Rust uses Serde types and schema/fixture tests.
- Every event carries schema_version, event_id, event_type, source_id, occurred_at, observed_at, ingested_at and payload.
- Use UTC RFC 3339 timestamps.
- Use decimal strings for exact financial values in JSON.
- Use Arrow/Parquet for large historical batches and research exports.

Do not duplicate business rules in both languages. Designate one production owner for each capability.

## 5. Initial transport and storage

Use PostgreSQL as the shared durable event store and work queue/outbox in the first implementation. It supports concurrent services without introducing a message broker before one is needed.

Use a single transactional outbox publisher with bounded batches. Add NATS JetStream only when throughput, fan-out or replay requirements justify a separate broker. Do not let multiple processes write to the same DuckDB database concurrently.

PostgreSQL stores normalized, queryable records: source definitions/health, observations, fills, positions, wallets, attribution evidence, news, events, people, roles, filings, rankings, alerts and analyst review.

Store immutable compressed JSONL or Parquet partitions by provider/date. Preserve the original body only when legally and operationally permitted; otherwise preserve source URL, retrieval metadata, required extracted facts and a cryptographic content hash.

Use DuckDB on read-only or isolated Parquet snapshots for analytical SQL, feature research, correlation studies and backtesting. Keep production writes in PostgreSQL.

## 6. Ingestion flow

Provider -> async collector -> rate limit/retry/health check -> raw observation and provenance -> schema validation -> normalized domain event -> PostgreSQL outbox -> Rust processor -> position state/entity graph/event correlation -> alert and evidence bundle -> API/dashboard/report.

A failed source must not stop unrelated sources. Every adapter needs health state, last-success timestamp, lag estimate, error counters and circuit breaker.

## 7. Market data and address discovery

Hyperliquid is the first live connector:
1. Discover perpetual DEXs and market metadata.
2. Subscribe to market-wide trade streams for configured markets.
3. Record fill identity, time, price, size, side, transaction hash and exposed participant addresses.
4. Add observed addresses to the unattributed wallet registry.
5. Query public account fills and current positions for each known address.
6. Reconstruct signed positions from ordered fills.
7. Reconcile against periodic account snapshots.
8. Emit anomalies only when supported by sufficient observations.

A market-wide trade stream does not reveal every account's full history or prove a position was opened. Account history and position snapshots are needed.

## 8. News and filing flow

Poll feeds independently with conditional requests and jitter. Deduplicate by provider ID, canonical URL and content hash. Keep publication, first-seen and retrieval timestamps separately.

Extract organizations, tickers, people, countries and event categories with confidence. Cluster multiple reports about one incident. Keep original source statements separate from extracted assertions. Parse disclosure documents into candidate facts with page/section traceability. Never merge a name-only match into a person-wallet attribution.

## 9. API surface

Initial endpoints:
- /health, /sources, /markets
- /wallets and /wallets/{address}/positions and /fills
- /positions/anomalies and /alerts
- /news, /events and /events/{id}
- /people, /people/{id} and /people/{id}/disclosures
- /wealth-rankings and /public-figure-watchlist
- /evidence/{id} and /reports/{id}

The UI should expose provenance, event-time alignment and uncertainty. Every result links to source evidence.

## 10. Reliability and resource controls

- Async I/O only in async tasks.
- Bounded channels and database batches.
- Per-source concurrency/rate budgets.
- Timeouts for DNS, connect, request, body read and database operations.
- Exponential backoff with jitter; respect Retry-After.
- WebSocket heartbeat, reconnect and gap-recovery queries.
- Idempotent writes and replay-safe processors.
- Graceful cancellation and shutdown.
- Metrics for queue depth, dropped/retried events, source lag, RAM, CPU, DB latency and reconnects.
- Configurable retention and disk budgets for raw data.
