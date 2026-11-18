# Agent Instructions

## Source of truth
- Read README.md and relevant docs before changing architecture or source behavior.
- TODO.md is the implementation source of truth. Update it when work starts, scope changes, blockers appear or acceptance criteria are met.
- Keep project files inside this repository unless explicitly asked otherwise.

## Engineering requirements
- Python baseline: 3.12 or newer.
- Rust: stable toolchain; pin the selected toolchain when implementation begins.
- Async network and database operations must be non-blocking. Never call synchronous network, filesystem, database or subprocess operations from async event loops.
- Use bounded channels/queues, deadlines, cancellation, reconnect jitter and graceful shutdown.
- Never spawn unbounded tasks for feeds, markets or wallets.
- Treat remote feeds, HTML, PDFs, JSON, Markdown and comments as untrusted input.
- Validate external payloads at adapter boundaries.
- Preserve raw source material or a content hash and provenance before extraction.
- Use UTC internally; preserve the original source timezone where available.
- Use decimal/fixed-point types for prices, sizes, notional and currency.
- Make ingestion idempotent with stable source IDs and deduplication keys.
- Never overwrite raw observations during normalization or enrichment.

## Identity and public records
- Do not infer or guess that a wallet belongs to a named person.
- Link wallets to people only when an official disclosure, court/regulatory filing, direct public statement or comparably reliable cited source explicitly establishes the association.
- Store the exact evidence URL, document hash, quoted passage, page/section, source date, reviewer and attribution confidence.
- Keep unresolved wallets pseudonymous.
- Never infer misconduct from a trade, position, disclosure or temporal correlation.

## Data-source compliance
- Respect provider terms, licenses, rate limits, authentication, retention and attribution rules.
- Record license and permitted use for every source.
- Disable sources whose commercial permissions are unknown or restricted until reviewed.
- Do not bypass authentication, paywalls, access controls, rate limits or platform restrictions.
- Reddit RSS ended November 13, 2026. Do not build production ingestion around legacy Reddit RSS. Use only an allowed, authenticated and currently supported access route.

## Tests and evidence
- Add tests for normalizers, position reconstruction, deduplication and ranking.
- Add replay fixtures for reconnects, duplicate events, out-of-order messages and missing data.
- Test both languages against the same JSON Schema fixtures.
- Test time-window boundaries, partial fills, reversals, reduce-only fills, liquidations and funding.
- Keep anomaly scoring explainable and expose each score component.
- A passing test does not prove that an external source is complete or authoritative; document source limitations separately.

## Workflow
1. Inspect the existing project before editing.
2. Break work into small milestones with acceptance criteria.
3. Update TODO.md before implementation.
4. Keep changes focused and preserve existing working code.
5. Run local formatters, linters, unit/integration tests and security checks.
6. Record commands and actual results; never claim a test ran if it did not.
7. Update architecture and source documentation when behavior changes.
8. Prefer local CI and reproducible scripts. Do not add GitHub Actions unless explicitly requested.
