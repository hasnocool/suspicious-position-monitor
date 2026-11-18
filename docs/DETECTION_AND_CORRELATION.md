# Detection and Correlation

## 1. Principle

Detect observable unusual activity, not motive. A large short or a trade near an incident is a research lead, not proof of insider trading, manipulation, coordination or wrongdoing.

The engine must explain why an item was flagged and what evidence is missing.

## 2. Position-change classification

Reconstruct signed exposure from ordered fills and reconcile to periodic venue snapshots.

Classification must account for new short from flat, add to existing short, partial close/reduction, full close, flip long-to-short, flip short-to-long, partial fills, aggregated fills, reduce-only executions, liquidation/ADL, subaccounts, transfers, margin changes, duplicate/out-of-order fills and missing history.

A sell is not automatically a new short. If fill history is incomplete, mark the position uncertain and avoid high-confidence alerts.

## 3. Baseline features

Calculate by wallet and asset:
- new short notional and incremental short notional
- notional relative to market volume and open interest
- notional relative to wallet historical median and robust dispersion
- percentile versus other observed wallets
- long/short bias change
- leverage and liquidation distance where reliable
- funding regime and funding payments
- concentration across related securities/sectors
- repeated adds over short windows
- timing versus market hours and known events
- wallet history and data coverage

Use rolling baselines (for example 7/30/90 days) and robust statistics such as median absolute deviation. Avoid one fixed dollar threshold across assets with different liquidity.

## 4. Explainable score

An initial research score may be a weighted sum of normalized features:

score =
  0.25 * size_anomaly
+ 0.20 * incremental_position_change
+ 0.15 * concentration_change
+ 0.15 * leverage_or_liquidation_risk
+ 0.15 * independent_event_relevance
+ 0.10 * historical_wallet_deviation

This is a starting hypothesis, not a validated model. Coefficients must be configurable, versioned and evaluated against historical data. Store every score component, raw value, normalization, baseline window and missing-data flag.

Do not let news relevance manufacture a position anomaly. Market evidence and event evidence are separate score families.

## 5. Event correlation

Correlate anomalies with issuer events, supplier/cloud/payment disruptions, sector and macro events, relevant jurisdictions/regulators, public announcements and official filings.

Use configurable time windows such as 1 hour, 6 hours, 24 hours and 7 days. Store the actual time difference and timestamp basis. Do not assume event occurrence time equals publication time.

Event confidence considers source reliability and independence, original versus syndicated reporting, official confirmation, contradictory reports/corrections, independent publishers and uncertainty in event time. Multiple copies of one press release are not independent corroboration.

## 6. Alert tiers

- Observation: unusual but weakly evidenced activity.
- Watch: statistically unusual position change with adequate fill data.
- High priority: large, confirmed position change with multiple independent supporting signals.
- Research review: possible time/event correlation requiring human examination.

Do not name an associated person unless a verified documentary wallet attribution exists. A watchlist match alone is not attribution.

## 7. Confounders and alternate explanations

Always display plausible alternatives: market-maker inventory/liquidity provision, hedging elsewhere, basis or cross-venue arbitrage, public event-driven research, subaccount reorganization, liquidation/risk reduction, options or ETF hedges, stale/mapped market data, market-wide volatility and funding dislocations.

## 8. Evaluation

Use historical replay and event studies. Measure precision at top N alerts, false-positive rate after analyst review, detection latency, position reconstruction error versus snapshots, feed uptime/completeness, event-cluster accuracy, alert stability and performance by asset/venue/jurisdiction/source.

Prevent look-ahead bias: a backtest may use only information observable at the time, including publication and ingestion timestamps.

## 9. Alert report

Every report should state:
1. What changed, by how much and when.
2. How the position was reconstructed and reconciled.
3. Why it is statistically unusual.
4. Which news/events were known at that time.
5. Whether a documented person/entity link exists and the exact evidence.
6. What is unknown, stale, contradictory or incomplete.
7. Alternate explanations and original source links.

No automated trading or public accusation workflow belongs in this project.
