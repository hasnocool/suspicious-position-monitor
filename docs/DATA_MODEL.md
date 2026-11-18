# Shared Data Model

The initial wire format is JSON validated by versioned JSON Schemas in schemas/. Financial numeric values are decimal strings. Timestamps are UTC RFC 3339. All source-derived facts retain provenance.

## 1. Common event envelope

Fields:
- schema_version
- event_id
- event_type
- source_id
- source_record_id
- occurred_at
- observed_at
- ingested_at
- payload

Do not collapse occurrence time, source publication time, first-seen time and ingestion time into one timestamp.

## 2. Core records

### MarketFill
Venue, network, DEX and market symbol; transaction hash and trade ID; wallet address where publicly exposed; maker/taker or buyer/seller role if provided; side, direction, price, size, notional and fee; fill/source timestamps; starting position if available; raw source reference and deduplication key.

### PositionSnapshot
Wallet, venue, market and DEX; signed position size (negative = short where venue convention supports it); entry/mark price, notional, leverage, margin used, liquidation price, unrealized P&L and funding state; snapshot timestamp and reconciliation status.

### PositionChange
Prior signed size, new signed size, signed delta and delta notional; classification (open/add/reduce/close short, open/add/reduce/close long, flip); supporting fills and snapshots; confidence and missing-data flags.

### Wallet
Chain/venue and address; first/last observed; observed markets; account-type hints where exposed; attribution state (unattributed, candidate, verified, disputed, retired). Do not store a person name here unless a separate verified attribution record joins it.

### Person / Organization
Stable internal ID; canonical name and aliases; native-script names; entity type; country/jurisdiction; public IDs such as Wikidata or government member ID; source facts with validity intervals; watchlist memberships.

### PublicRole
Person ID, position title, organization, jurisdiction, start/end dates, current/historical status, source URL, source ID and last-verified timestamp.

### DisclosureFiling
Filer/person ID only where source establishes identity; jurisdiction and filing authority; filing/report type; report period, disclosed transaction date, filing/publication dates; original URL, content hash, format and extraction state; license classification.

### DisclosedInterest / ReportedTransaction
Person ID or unresolved filer ID; asset/instrument and issuer/ticker where disclosed; asset category and value range; transaction type/date; source filing and exact page/section/row locator. Never fabricate a precise value from a value band.

### WalletAttribution
Wallet ID and person/entity ID; explicit source evidence and excerpt; source hash and locator; method, reviewer, confidence and review state; valid-from/to dates and dispute history.

### NewsItem
Source and item ID, URL/canonical URL, title, summary/excerpt, language, author if supplied, published/first-seen/retrieved timestamps, content hash, permitted-retention state and extracted entities/tickers with confidence.

### Event
Category, title, normalized summary, first-seen/event-occurred/published timestamps, geography and sector, affected organizations/people/securities, severity and confidence, confirmation count, linked news items, lifecycle state (emerging, confirmed, corrected, resolved, rejected).

### Anomaly / Alert
Wallet/venue/market and observed change; score and exposed components; baseline window/sample size; event correlations and measured time deltas; missing-data/confounder flags; evidence links, analyst state and notes. Do not include a guilt or intent field.

## 3. Evidence semantics

A person record must not contain inferred blockchain addresses. The relationship is represented only through WalletAttribution with supporting evidence and a human-review state.

Evidence categories:
- primary_official
- primary_regulatory
- primary_court
- direct_public_statement
- reputable_secondary
- unverified
- contradicted
- withdrawn

Only reviewed evidence classes can create a verified wallet attribution. Unverified evidence may be retained as a research lead without linking the person and wallet in public views.

## 4. Numeric and time rules

- Decimal strings in JSON; decimal/numeric database columns.
- Never store money in binary floating point.
- Preserve original currency and converted reporting currency separately.
- Store FX source, rate and timestamp for conversions.
- Store original source timezone where available, plus normalized UTC.
- Use event-time processing with late-arrival tolerance.
- Never replace source publication time with collector retrieval time.
