# Data Sources

This is a source registry plan, not a guarantee that every source is complete, free for commercial use or suitable for automated bulk ingestion. Revalidate endpoint behavior and terms before implementing each adapter.

Every provider configuration must record owner, endpoint, authentication, permitted use, license/terms URL, rate limit, expected latency, coverage, last checked date, retention rules and health status.

## 1. Market and blockchain

| Source | Initial use | Interface / data | Limitations |
|---|---|---|---|
| Hyperliquid | First production connector | Info API and WebSocket: DEX metadata, markets, trades, user fills, clearinghouse state, funding and books | Account endpoints need an address; market-wide trades discover addresses only as fills are observed. |
| dYdX | Later | Public indexer and chain events | Validate current endpoints and position semantics first. |
| Drift | Later | Solana program/indexer data | Protocol-specific account and market reconstruction required. |
| GMX | Later | EVM events and public protocol data | Multiple versions/chains; use versioned adapters. |
| Other EVM protocols | Later | Public RPC logs, subgraphs or documented APIs | RPC alone may not provide a convenient full position ledger. |

Hyperliquid reference:
- Info: https://api.hyperliquid.xyz/info
- WebSocket: wss://api.hyperliquid.xyz/ws
- DEX discovery: perpDexs
- Metadata/context: meta and metaAndAssetCtxs
- Market-wide trades: WebSocket trades subscription per coin
- Positions: clearinghouseState
- Account fills: userFillsByTime and userFills
- Prices/books: allMids, l2Book and activeAssetCtx
- Funding: fundingHistory

Discover HIP-3 assets dynamically. Preserve DEX name as well as symbol because symbols can overlap across DEXes.

## 2. News and incident feeds

| Source | Interface | Use | Notes |
|---|---|---|---|
| Google News | RSS top stories, sections and search | Broad news, company/person/ticker/event queries | Public RSS endpoint patterns; configurable locale/query. Not the personalized Chrome Discover feed. |
| GDELT DOC 2.0 | JSON article search and timelines | Worldwide, multilingual news | Use conservative request pacing and validate current limits. |
| GDELT event files | Periodic event, mention and knowledge-graph files | Structured event trends | Preserve event time separately from dataset update time. |
| CISA | Official alerts/advisories/KEV resources | Cybersecurity incidents and vulnerabilities | Official advisory is not itself proof of market impact. |
| Cloud/status providers | Statuspage-compatible feeds or public status APIs | Cloud, CDN, DNS, SaaS and infrastructure outages | Status histories may be revised. |
| Exchange/protocol status | RSS, JSON or public status APIs | Trading venue downtime and maintenance | Distinguish maintenance from incidents. |
| Company investor relations | Public RSS/HTML releases | Earnings, guidance and material announcements | Prefer canonical issuer release. |
| Government press rooms | RSS, HTML or official APIs | Legislation, sanctions and policy | Country-specific and multilingual. |

Google News top stories example:
https://news.google.com/rss?hl=en-US&gl=US&ceid=US:en

Google News search example:
https://news.google.com/rss/search?q=cyberattack%20OR%20outage&hl=en-US&gl=US&ceid=US:en

Create separate feeds for world, business, technology, cybersecurity, financial infrastructure, relevant issuers and named events. URL-encode query text and store the exact query used.

### Reddit — current access warning

Reddit announced RSS support would end on November 13, 2026. That date has passed for this project baseline (November 18, 2026).

- Do not rely on /r/{subreddit}/.rss or /search.rss in production.
- Legacy .json listing/search URLs may be probed in a disabled compatibility test, but are not a guaranteed or approved durable interface.
- Production must use a currently supported, authenticated access path and follow the current developer/API terms.
- Reddit's current Data API Terms reserve the right to limit or charge for API use; commercial API use requires a separate agreement unless expressly permitted.
- Do not bypass OAuth, rate limits, access controls or content-retention requirements.
- Store links, IDs, timestamps and the minimum licensed data necessary.

Suggested communities should be configuration, not hardcoded dependencies. Candidate categories include cybersecurity, technology, finance, investing, cloud infrastructure, geopolitics and issuer communities. Monitor posts and comments only within permitted API scopes.

## 3. Traditional markets and public companies

| Source | Interface | Data | Important limitation |
|---|---|---|---|
| FINRA short-sale volume | Public daily files | Aggregate short-sale volume by security | Not an open short-position balance. |
| FINRA/exchange short interest | Public reports/downloads | Aggregate short positions at reporting snapshots | Periodic and delayed; no individual trader identity. |
| SEC EDGAR | data.sec.gov JSON APIs, filing indexes and documents | Company submissions, filings, XBRL facts | Follow SEC access policy and declared User-Agent. |
| SEC fails-to-deliver | Public data files | Settlement failures by security/date | Not proof of naked short selling or manipulation. |
| CFTC Commitments of Traders | Public datasets | Aggregate futures positioning | Weekly snapshot; not named traders. |
| Company IR/exchange | Public releases and market data where available | Earnings, issuer announcements, market context | Licenses and delays vary. |

Never label FINRA daily short-sale volume as short interest. SEC Form 13F does not report short positions.

## 4. Government disclosure systems

| Source | Access route | Use and limitation |
|---|---|---|
| US House Clerk | Financial Disclosure Reports database | Public documents/search; portal displays statutory restrictions including a prohibition on commercial use other than news/communications media for public dissemination. Require legal review before commercial use. |
| US Senate eFD | Public Financial Disclosure database | Member/candidate reports and periodic transaction reports; public inspection period is six years for covered filings. Not assumed to be a structured API. |
| US OGE | Public financial disclosure document access | OGE directly holds only some filer reports; many are held by agencies. Special access and retention rules apply. |
| UK Parliament | Register of Members' Financial Interests API | Structured data under the Open Parliament Licence; preserve register edition/date. |
| Canada Conflict of Interest and Ethics Commissioner | Public registry | No general official bulk API identified in this research. Prefer official pages or authorized downloads; do not assume third-party mirrors are licensed. |
| Other jurisdictions | Country-specific registries | Add only after reviewing legal access, source terms, language, update cadence and disclosure coverage. |

Financial disclosures may show assets, transactions, income or interests, but usually not live brokerage positions or wallet addresses. Store report period, transaction date, filing date and public release date separately.

## 5. People and wealth sources

| Source | Use | License/reliability |
|---|---|---|
| Wikidata Query Service | Discover office holders, roles, identifiers and aliases | Discovery/enrichment only; verify current roles against official sources. |
| OpenSanctions PEP collection | Optional global PEP enrichment | Free for non-commercial use; commercial use requires a data license. Disable in commercial deployments without permission. |
| Official government/parliament sources | Validate current positions and disclosures | Preferred primary evidence; rules differ by jurisdiction. |
| Forbes Real-Time Billionaires page | Public wealth-list reference | Do not assume a general-purpose official free API. Wealth estimates are not audited balances. |
| komed3/rtb-api | Optional machine-readable Forbes-derived data | Its repository is MIT-licensed; its README says data is mostly updated monthly, disclaims accuracy/completeness and announces an API change. Discover paths from list/_index and validate current schema at runtime. |

The wealth provider is replaceable. Do not make the people schema dependent on one provider.

## 6. Access and source policy

- Per-provider concurrency and token-bucket limits.
- ETags / If-Modified-Since where supported.
- Descriptive User-Agent and contact information where required.
- Respect Retry-After and throttling.
- No identity rotation, proxy rotation or limit evasion.
- Persist source failures and lag; visibly mark stale data.
- Unknown licensing defaults to review_required, not unrestricted.

## 7. Source registry fields

Each source record should contain: source_id, provider, category, endpoint, access_method, authentication, license_url, terms_url, permitted_use, rate_limit_policy, enabled and last_verified_at.
