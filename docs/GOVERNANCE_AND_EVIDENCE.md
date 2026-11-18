# Governance, Licensing and Evidence

## 1. Evidence-first outputs

Every claim shown in the interface must be traceable to source records. Distinguish raw observation, normalized fact, source assertion, model-generated extraction, inferred market state, analyst interpretation and unresolved hypothesis. Never display an interpretation as a primary-source fact.

## 2. Wallet attribution safeguards

Wallet-to-person attribution is allowed only through explicit public evidence. Do not deanonymize or guess a wallet owner. A wallet that cannot be attributed remains a pseudonymous address and can still be analyzed.

Verified attribution requires exact address/network/venue, explicit person named by source, URL/publication date/content hash, exact excerpt and page/section, source quality/reviewer, claimed validity period and correction/dispute mechanism.

A social-media mention, same-name match, transaction graph or timing correlation is not sufficient on its own.

## 3. Source license registry

Each source needs a machine-readable policy record:
- license name and URL
- terms URL
- commercial-use status: allowed, prohibited, license_required or unclear
- attribution requirement
- retention/deletion requirement
- access authentication and permitted scope
- rate limit and caching policy
- review date and reviewer

Unknown use is review_required by default.

Examples requiring special treatment:
- US House financial disclosure portal states a statutory restriction on commercial use, with a news/communications-media exception for public dissemination. Do not use those records in a commercial analytics product without legal review.
- OpenSanctions says its PEP collection is free for non-commercial use; commercial use requires a data license.
- Reddit Data API Terms say commercial API use may require a separate agreement and reserve the right to set fees/limits. Reddit RSS support ended November 13, 2026.
- UK Parliament Register of Members' Financial Interests data is offered under the Open Parliament Licence.
- Third-party wealth-list APIs may have a project license, but underlying source data and accuracy must still be assessed.

A public URL does not grant unlimited rights to crawl, retain, redistribute or monetize its content.

## 4. Personal data minimization

- Collect only data required for public-interest research.
- Do not store private credentials, private messages, redacted identifiers or sensitive contact details.
- Respect statutory access declarations.
- Make retention configurable and honor provider deletion requirements.
- Keep wallets pseudonymous until attribution is verified.
- Restrict analyst notes and review decisions to authorized users.

## 5. Publication language

Prefer:
- unusual short-position increase
- large observed short exposure
- time-correlated event
- public disclosure reports
- wallet attribution not established
- requires independent verification

Avoid calling activity insider trading or manipulation unless an authoritative legal finding establishes that. Do not say an official owns a wallet without verified attribution. A risk score is a prioritization mechanism, not a finding of misconduct.

## 6. Corrections and disputes

Every person, role, filing extraction and wallet attribution needs a correction path. Preserve prior versions and correction timestamps. Mark withdrawn/contradicted evidence rather than silently deleting it. Recompute affected reports when source facts change. Keep an audit trail and surface stale source data.

## 7. Security

- Store API credentials in environment variables or an OS secret store; never commit them.
- Separate public config from secrets.
- Use least-privilege database roles.
- Keep credentials and unnecessary personal data out of logs.
- Validate and limit downloaded file sizes/content types.
- Sandbox PDF/HTML parsing workers.
- Protect the local API with authentication when bound beyond loopback.
