# Suspicious Position Monitor

A local-first financial intelligence and research platform that detects unusual position changes, monitors breaking events, and links observations to public records with auditable evidence.

**Status:** architecture baseline only. Production collectors and runtime code have not yet been implemented.

## What it investigates

1. Market activity: new shorts, increases in existing shorts, reductions, liquidations, abnormal notional, leverage and funding.
2. World events: cyber incidents, outages, geopolitical developments, legislation, sanctions and corporate announcements.
3. Public records: financial disclosures, official roles, public-interest people, wealth rankings and wallet addresses explicitly attributed by documentary evidence.
4. Relationships: whether a verified event and unusual position change are close in time and relevant to the same issuer, sector or market.

This is an evidence-gathering tool, not an automated trading system. A correlation is not proof of motive, insider trading, wrongdoing or causation. The system must not deanonymize wallets or guess that an address belongs to a person.

## Core design

- Python 3.12+ for source adapters, news/document ingestion, entity research and experimentation.
- Rust stable for resilient market streaming, event normalization, position reconstruction, scoring and the API.
- Shared versioned JSON Schemas; Arrow/Parquet for interchange and historical research.
- PostgreSQL for concurrent service state and queryable records.
- Parquet for immutable raw-event archives; DuckDB for local analytical queries and backtests.
- Async, non-blocking network and database operations; bounded queues, backpressure, timeouts and graceful shutdown.
- Source provenance and timestamps on every observation.
- Local-first operation with optional scaling.
- No GitHub Actions dependency in the initial project.

## Two separate top-500 cohorts

### A. Global public-interest watchlist — 500 people

A transparent, multi-category selection of current or recent public office holders and prominent public decision-makers. This is a project-defined prioritization list, not an objective universal ranking. Each entry records category, selection rationale, jurisdiction, role dates and source confidence.

Potential categories include heads of state/government, cabinet officials, senior legislators, central-bank and financial-regulator leaders, senior judicial and military office holders, multilateral leaders and selected major corporate decision-makers.

### B. Wealth watchlist — 500 people

A separately maintained snapshot of the 500 highest-ranked people from a configured public wealth-list provider. Preserve provider, rank, estimated net worth, currency, as-of timestamp, retrieval timestamp and source URL. Estimates are not audited balances.

The cohorts can overlap. Deduplicate people by stable IDs while retaining separate cohort membership and ranking histories.

## Target repository layout

- AGENT.md, README.md and TODO.md
- docs/ for architecture, data sources, data model, detection, governance, people/disclosures and roadmap
- schemas/ for shared JSON Schema contracts
- config/ for feed, market and watchlist configuration
- python/ for async collectors, document extraction and research adapters
- rust/ for the workspace and core services
- migrations/ for PostgreSQL schema
- tests/ for fixtures, replay tests and contract tests
- scripts/ for local developer tooling
- data/ for local runtime data; never commit runtime data or credentials

This is the intended layout; most runtime folders do not yet exist.

## Language ownership

| Capability | Python | Rust |
|---|---|---|
| RSS, JSON APIs and news collection | Primary | Event ingestion |
| Reddit, Google News and GDELT | Primary adapters | Event processing |
| Government documents and filings | Download and extraction | Structured records and API |
| People and wealth registries | Research and refresh | Query and serve |
| Exchange WebSockets | Prototypes and fixtures | Primary long-running stream |
| Position reconstruction | Reference model and research | Production engine |
| Anomaly scoring | Model research/backtesting | Deterministic production scoring |
| Storage | Async ingestion clients | Shared storage layer |
| HTTP API | Research clients | Primary API |

Avoid duplicating business rules. The shared schema is authoritative; each production capability has one implementation owner.

## Initial source priorities

1. Hyperliquid public Info and WebSocket APIs, including HIP-3 perpetual DEX discovery and stock-linked markets.
2. Google News RSS and GDELT for global news and events.
3. Official incident feeds including CISA and infrastructure-provider status feeds.
4. Reddit only through a currently permitted authenticated access method. Reddit announced RSS support ended November 13, 2026; do not assume legacy .rss or .json routes remain available or permitted.
5. Official public disclosure systems: US House, US Senate, US OGE, UK Parliament, Canada and later jurisdiction-specific sources.
6. Wikidata for discovery and enrichment; verify current roles against primary official sources.
7. Optional Forbes-derived wealth-list provider with explicit provenance and update timestamps.
8. FINRA, SEC and CFTC public datasets for traditional markets, with their aggregation and publication-delay limitations.

## Critical limitations

- Traditional equity markets do not publish every investor's live short positions. FINRA short-sale volume is not short interest; short-interest reports are delayed aggregate snapshots; SEC Form 13F does not report short positions.
- Hyperliquid market-wide trades can reveal addresses participating in monitored markets, but full account history requires account-level queries for each known address.
- Public prominence or wealth does not imply that a person owns a particular blockchain address.
- Financial disclosure documents often list securities or asset categories but do not disclose brokerage account identifiers or wallet addresses.
- Disclosure systems have different filing delays, coverage, privacy rules, access methods and licenses.
- There is no universal authoritative ranking of the world's 500 most prominent people. The project's selection methodology must be transparent and versioned.

## Safety and research posture

- Wallet attribution requires a direct, cited public documentary link. Heuristic matches never become person-level attribution.
- Preserve original source links, dates, hashes and extracted passages.
- Show contradictory evidence and uncertainty.
- Do not publish allegations or present anomaly scores as proof of misconduct.
- Do not collect private credentials, account numbers or unnecessary personal data.
- Respect source terms, access controls, rate limits, retention requirements and commercial licensing.

## Project documents

- docs/ARCHITECTURE.md
- docs/DATA_MODEL.md
- docs/DATA_SOURCES.md
- docs/DETECTION_AND_CORRELATION.md
- docs/GOVERNANCE_AND_EVIDENCE.md
- docs/PEOPLE_AND_DISCLOSURES.md
- docs/ROADMAP.md
- TODO.md

## License

No project license has been selected yet. Do not add a license until intended reuse and the licensing compatibility of integrated datasets have been reviewed.
