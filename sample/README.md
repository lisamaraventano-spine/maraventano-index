# Sample — one complete day

`MI-30Day-sample-2026-10-08.jsonl`

One complete UTC day of the observational dataset, taken verbatim from the 30-Day Report (Edition V.1): **October 8, 2026**, the closing day of that edition's window. It carries the same schema and the same redaction as the full dataset. Nothing is synthetic or altered for presentation.

| | |
|---|---|
| Records | 14,257 (one JSON object per observed HTTP request) |
| Settled rows present | 3 — rows carrying the approximate row-level `paid` flag. The **authoritative** Oct 8 count is **5 settlements** (Appendix B); the row-level join is lossy and undercounts (see below). |
| Schema | identical to the full dataset — [`../DELIVERABLES.md` §3](../DELIVERABLES.md#3-the-dataset-file-1-schema) |
| Redaction | identical to the full dataset — [`../METHODOLOGY.md` §10](../METHODOLOGY.md#10-data-protection) |

One day is approximately 1/30 of a monthly edition. It is included so the dataset's structure and contents can be examined directly.

## Reading the records

Field definitions are in [`DELIVERABLES.md` §3](../DELIVERABLES.md#3-the-dataset-file-1-schema).

- Most rows are attempts that did not pay — probes, indexers, monitors, and crawlers reaching the catalog. That activity is counted, not discarded; see [`METHODOLOGY.md` §6](../METHODOLOGY.md#6-what-counts-and-what-is-noise).
- A row with `wallet` set and `paid: true` is a settled request — an on-chain USDC payment. **Three** rows here carry that flag (all `/api/x402-operator-census/lookup`), but the **authoritative** Oct 8 total is **5 settlements** per Appendix B. The row-level `paid` flag is an approximate join and undercounts; the settlement appendix is the source of record (see [`METHODOLOGY.md` §9](../METHODOLOGY.md#9-settlements)).
- `delivered: true` with `paid: false` indicates free fulfillment — the process completed, no payment.

## Caveats

User-Agents are self-declared and spoofable. A `machine` identifier is one source network address, not one person, agent, or device. `delivered` ≠ `paid` ≠ `settled`. Wallet addresses are public on-chain data. See [`METHODOLOGY.md` §13](../METHODOLOGY.md#13-standing-caveats).
