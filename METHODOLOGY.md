# Methodology

The computation rule for the Maraventano Index. This document is the governing specification. It is held constant across editions and amended only by a dated, published revision.

---

## 1. Scope and object of measurement

The Maraventano Index measures change over time in observed purchase-attempt demand in x402 commerce, as recorded at the merchant seat — the server that receives the requests. The underlying record is continuous from April 6, 2026.

The Index is a measurement, not an analysis. It reports a value produced by a fixed rule and makes no claim as to the meaning of that value, the cause of a movement, its persistence, or any course of action. Interpretation is left to the reader.

The quantity measured is observed purchase-attempt demand: a measurable indicator of demand, not a count of distinct buyers and not an assertion of willingness to pay. Automated clients — monitors, probes, indexers, and rankers — are included as attempts (§6).

## 2. Published measures

The publication comprises two measures.

**Total Purchase Attempts** is the gross count of eligible purchase attempts across every paid endpoint in service on a given day. It reflects the full set of endpoints as it exists, and so changes as endpoints are added or withdrawn.

**The Maraventano Index** is the headline measure. It is computed on a like-for-like basis: only endpoints in service across the whole of a comparison contribute to its movement, so the addition or withdrawal of an endpoint cannot, in itself, produce a change in the Index.

## 3. Formula

The daily Index is:

```
Index = (continuing-basket purchase attempts for the day) ÷ 3,812 × 100
```

The continuing basket is the set of paid endpoints holding comparable operating history across the comparison window. The value 3,812 is the fixed baseline (§4). The Index reported for a multi-day edition is the arithmetic mean of the daily Index values over the period.

## 4. Baseline

Index 100 corresponds to 3,812 purchase attempts per day.

The baseline is the continuing basket's mean daily volume over August 4 – September 8, 2026 (36 complete UTC days; unrounded mean 3,811.5, published as 3,812). It is established once and carried forward into every subsequent edition without change. The baseline does not reset. A fixed baseline is what renders successive editions comparable, as a shared base period does for other reference indices.

## 5. Definitions

| Term | Definition |
|---|---|
| **Hit** | An included HTTP request to a monitored x402 host, under the logging regime in force for the period (§7). |
| **Purchase Attempt** | A request to an eligible paid endpoint: an Underground `/buy/` path or a designated paid `/api/` path. Buy and API attempts are counted on the same basis and summed. |
| **Delivery** | An HTTP 200 response on a paid path, including free fulfillment. Delivery is not payment. |
| **Settlement** | An external USDC payment recorded on-chain and independently verifiable. The authoritative record of settlement is the on-chain ledger (§9), not any server-side indicator. |

## 6. Inclusion of traffic, and the definition of noise

Automated clients are included. Probes, crawlers, monitors, health checks, rankers, and indexers are counted as participants in x402 activity: an indexer traversing the catalog or a monitor testing liveness is a genuine x402 interaction observed at the merchant seat. Excluding such traffic would remove the signal the Index is designed to capture.

Noise, which is excluded, comprises only credential scanners and malformed or exploit traffic — requests targeting paths such as `/.env`, `/wp-admin`, `/.git`, `/phpmyadmin`, shell locations, SQL-injection strings, and absolute-URI proxy attempts. Such traffic does not participate in x402 commerce. It is segregated from the published activity and contributes to neither Hits nor Purchase Attempts.

Noise is classified by requested path and request format, not by User-Agent; a self-declared User-Agent cannot establish that a request is, or is not, a scanner.

## 7. Logging regimes

A change to server logging configuration on August 3, 2026 divides the record into two regimes. Both are retained, and the boundary is disclosed on any series that crosses it.

- **Regime 1 (before August 3).** Logs carry no virtual-host field. Eligible commerce requests are attributed by path. Records were written once; no duplicate-line correction applies.
- **Regime 2 (from August 3).** Logs record the virtual host. Concurrent logging directives produce identical adjacent lines, which are collapsed before parsing.

Purchase Attempts are defined identically under both regimes and remain comparable across the full window. Included-Hit totals across the boundary reflect a change in measurement coverage and are not directly comparable; this is noted wherever the two are presented together.

## 8. Roster changes

The set of paid endpoints changes over time. The Index accommodates this under the following rules.

The Index is not divided by endpoint count. Endpoints differ by orders of magnitude in the volume they attract, and weighting them equally would introduce large, spurious movement. The Index is computed on total attempts within the continuing basket; an endpoint drawing no demand contributes nothing, and the total is therefore self-correcting.

A new endpoint enters Total Purchase Attempts on the day it opens. It joins the Index only once it holds comparable operating history, chained into the series so that its arrival does not itself move the Index.

A retired endpoint leaves the continuing basket on its retirement date, disclosed with a dated marker. Where a roster change is large enough to create a discontinuity in the series, the series is chained across the seam so that the change does not register as demand movement.

Every roster change, and every external intervention — an endpoint launch or retirement, a discovery-catalog listing, a coordinated sweep — is recorded as a dated event marker on the series. Markers are retained permanently and are not presented as organic movement. A listing that raises demand is reported as such, with its marker: a documented effect, not an irregularity. As with other reference indices whose constituents change, the constant is the published rule and the disclosed changes, not a fixed membership.

## 9. Settlements

Settlements are established on-chain, independently of the server-request pipeline, and are published as a settlement appendix giving, for each, the public transaction identifier (Base) or signature (Solana) and the UTC timestamp. Each is independently verifiable on a public block explorer.

The dataset carries a row-level `paid` indicator. It is an approximate join, is predominantly false, and is secondary. The authoritative count of settlements is always the on-chain appendix.

## 10. Data protection

Every distributed record is redacted as follows.

- Source IP addresses are removed and replaced by a stable, salted, one-way identifier (`machine`). The raw address is not stored, and the identifier is not reversible. A `machine` denotes one source network address, not one person, agent, or device.
- User-Agent strings are retained as transmitted but are self-declared and may be spoofed; they are truncated to 120 characters.
- Query strings are reduced to an allowlist; all other parameters, including any inadvertently transmitted credentials, are discarded.
- Payment signatures, nonces, authentication material, and API keys are removed.
- Underground product paths are replaced by stable `product_N` labels; the mapping from label to product is not distributed.
- Traffic outside the defined scope is excluded in full.

## 11. Exclusions from the totals

Three classes of traffic are excluded from all published totals:

- **Noise** (§6) — credential scanners and malformed or exploit traffic.
- **Internal traffic** — requests originating from the publisher's own infrastructure.
- **Test and house settlements** — settlements and attempts originating from the publisher's own wallets, used for testing and verification, are removed from both the Index and the published settlement counts. The Index reflects external demand only.

## 12. Integrity and verification

Each distributed dataset is accompanied by an Ed25519-signed integrity manifest (key identifier `underground-attest-2026-07-18`) stating the file's SHA-256 digest, its record count, and the public key. The manifest establishes that the dataset is the Index's own and has not been altered since signing.

Independent confirmation is available at the settlement layer (§9): each settlement is a public on-chain transaction that any party may verify. The Index's standing rests on a published method, a continuous record, and that public settlement layer.

## 13. Limitations

- User-Agent values are self-declared and may be spoofed.
- A `machine` identifier corresponds to a source network address, not to an individual actor.
- Delivery, payment, and settlement are distinct: delivery is not payment, and neither is settlement.
- The Index is an indicator of observed purchase-attempt demand, not a census of buyers.

---

*This specification reflects the method as applied to the editions described in [`DELIVERABLES.md`](DELIVERABLES.md). Any revision is dated and published.*
