# People, Wealth Lists and Financial Disclosures

## 1. Two separate top-500 cohorts

Do not merge public prominence and personal wealth into one score.

### Cohort A: 500 global public-interest figures

This is a transparent project-defined watchlist of current or recent people with major public decision-making roles or documented influence over public markets, policy or critical infrastructure. It is not an objective universal ranking of the world's most famous people.

Suggested categories:
- Heads of state and heads of government.
- Cabinet ministers and senior executive-branch officials.
- Finance, treasury, trade, energy, defense and technology ministers.
- Central-bank governors and senior monetary-policy officials.
- Leaders of securities regulators, competition authorities and financial regulators.
- Speakers, party leaders and senior legislative committee chairs.
- Senior judicial office holders where decisions have material public-market implications.
- Senior leaders of multilateral institutions.
- Selected leaders of state-owned enterprises and systemically important public companies.
- Other decision-makers added through documented editorial review.

Use versioned selection rules and explainable scoring dimensions: public authority, geographic scope, seniority, currentness, market relevance and evidence quality. Do not use social-media popularity as the main measure. Apply country/region and role-category balancing rules to reduce source-coverage bias. Show gaps instead of pretending global completeness.

### Cohort B: 500 wealth-ranked people

Use one or more public wealth-ranking providers to produce a dated top-500 snapshot.

Candidate first provider: komed3/rtb-api, https://github.com/komed3/rtb-api

Its README describes a Forbes real-time billionaire-derived API. It documents a list index, dated/latest list endpoints and profile index; says data is mostly updated monthly; warns of an upcoming API change; and disclaims accuracy/completeness. The repository is MIT-licensed. Discover the available list IDs from its list index at runtime rather than hardcoding an assumed value. Validate the provider schema and eligible row count, then select the first 500 by the provider's own rank.

Keep provider attribution and the accuracy caveat visible. A person's wealth rank does not imply public company holdings, a public wallet or a known trading account.

## 2. Global public-office discovery

1. Ingest official lists of current heads of state/government, ministers, legislators, central-bank leaders, regulators and multilateral leaders.
2. Use Wikidata to discover potential roles, aliases and identifiers.
3. Use OpenSanctions PEP data only if its license permits the deployment; free bulk PEP access is non-commercial, and commercial use requires a license.
4. Verify current roles against official pages/APIs or dated government records.
5. Normalize names, transliterations, native-script names, titles, jurisdiction, identifiers and term dates.
6. Resolve duplicates using stable external IDs first, then human-reviewed matching.
7. Generate a versioned candidate ranking and review the top-500 selection.
8. Preserve all sources and selection decisions.

A person without reliable current-role evidence may remain in the historical directory but must not be marked as a current official.

## 3. Disclosures are not a worldwide unified dataset

There is no single public, machine-readable global database containing every official's assets and transaction history. Coverage, reporting thresholds, filing intervals and access methods vary by jurisdiction.

### United States

**House of Representatives**
- The Clerk's Public Disclosure site provides financial disclosure documents and search.
- The portal displays a statutory restriction against commercial use, except for news/communications media for general-public dissemination, among other restrictions.
- Carry a restricted-use flag and disable this adapter for commercial deployments unless counsel confirms a permitted use.
- Do not collect redacted personal identifiers or attempt to reconstruct them.

**Senate**
- The eFD system provides public financial disclosure reports.
- Senate Ethics states covered reports (with an exception for certain unelected candidates) are available for public inspection for six years.
- Periodic Transaction Reports can contain reported transactions, but the portal is not assumed to be a free structured API.
- Preserve report type, report period, filing date, disclosed transaction date and publication date.

**Executive branch / OGE**
- OGE Form 278e and periodic transaction reports are relevant.
- OGE says it reviews roughly 1,100 of approximately 26,000 executive-branch public disclosure filings; many remaining reports are held by the agencies.
- Reports have a special access process and are generally destroyed after six years.
- Treat OGE as a document/source adapter, not a complete government-wide API.

### United Kingdom

- The Register of Members' Financial Interests is available through a Parliament API under the Open Parliament Licence.
- Members must register changes to registrable interests within 28 days.
- Preserve register edition date because records and corrections can change.

### Canada

- The Conflict of Interest and Ethics Commissioner's public registry is the authoritative starting point for federal MPs and ministers' public declarations.
- Do not assume a public bulk API. Do not treat third-party mirrors as official or similarly licensed.
- House of Commons open data can help validate members, names, constituencies, parties and roles; it is not the financial disclosure register itself.

### Other jurisdictions

Implement country adapters only after identifying:
- official disclosure authority and legal basis
- public access scope
- API, bulk file or document download options
- filing deadlines and actual publication delay
- asset categories and value thresholds
- family/dependent asset rules
- license, automated-access and retention requirements
- official languages and translation quality

Do not extrapolate one country's rules to another.

## 4. Wallet-to-person attribution

A public official or wealthy person's name must never be used to guess wallet ownership. Associate a wallet with a person only where a public source explicitly establishes the relationship.

Acceptable evidence:
- signed/published official disclosure explicitly identifying the address
- court, regulator or government filing explicitly attributing the address
- direct public statement by the person or authorized representative
- reliable investigative reporting that explicitly identifies the address, with preserved evidence and human review

Not sufficient on its own:
- same or similar names
- a wallet trading a stock connected to the person's employer or country
- activity near a public announcement
- wealth estimates, shareholdings or directorships
- social-media speculation or unsourced wallet labels
- transaction graph proximity or common funding source without explicit attribution

Unverified addresses remain in a pseudonymous wallet registry and can still be analyzed as wallets. They are not joined to a person.

Every attribution stores address/network/venue, person/entity ID, evidence URL, document title/date, exact excerpt and page/section, source hash, attribution method, reviewer, confidence, review status, validity dates and correction/dispute history.

## 5. What disclosures can show

Disclosures can establish reported asset interests and transactions subject to reporting rules and publication delays. They generally do not reveal real-time brokerage positions, account credentials or complete trade histories.

Documents may use value bands, report delayed transactions, list indirect ownership or cover only certain asset types. Absence of a filing entry is not evidence that an asset/account does not exist.

Keep these record types separate:
- reported_holding
- reported_transaction
- wallet_attribution
- observed_wallet_activity
- market_position
- analyst_hypothesis

## 6. Rankings and history

Store versioned snapshots rather than overwriting ranks:
- cohort and cohort version
- rank and scoring/methodology version
- provider and provider record ID
- estimated wealth and currency where relevant
- public role and jurisdiction where relevant
- as-of time and retrieval time
- source evidence

A person can leave a cohort without being deleted from the entity directory. Preserve historical membership for reproducible research.
