# Deliverables

The contents of each published product, file by file, with record counts, formats, and integrity. A one-day sample of the dataset is provided in [`sample/`](sample/).

Each product is delivered as a single bundle, available on either payment rail — x402 (USDC on Base or Solana) or Stripe. On confirmed settlement the purchaser receives the bundle described below.

Each bundle is a self-contained archive comprising the report (PDF), the observational dataset behind it (newline-delimited JSON, one record per observed HTTP request), an Ed25519-signed integrity manifest, an on-chain settlement appendix, a data dictionary, and a README. The dataset schema is identical across products and is given once in [§3](#3-the-dataset-file-1-schema).

---

## 1. Founding Report — one-time

| | |
|---|---|
| **Window** | April 6 – September 8, 2026 (156 UTC calendar days, from go-live) |
| **Scope** | A single x402 virtual host, through which both the Underground Cultural District (`/buy/`) and the Maraventano Agent Protocol (`/api/`) services operated during the period |
| **Format** | PDF report and JSON Lines dataset |

**Bundle — 7 files:**

| File | Description |
|---|---|
| `MI-Founding-Report.pdf` | The report: methodology, measures, the historical Index reference, Appendix A (daily observations), Appendix B (settlements). |
| `MI-Founding-FILE1-activity.jsonl` | The observational dataset — **1,096,694** request-level records, one JSON object per line (approximately 270 MB). |
| `FOUNDING-APPENDIX-B.md` | The on-chain settlement ledger — **133** public transfers (87 Base, 46 Solana), each verifiable on a block explorer. |
| `FOUNDING-DATA-DICTIONARY.md` | Field reference for the dataset. |
| `manifest-signed.json` | Ed25519-signed integrity manifest: the dataset's SHA-256 digest, record count, and public key. |
| `README.txt` | Contents and verification steps. |
| `LICENSE.txt` | License and distribution terms (institutional use). |

Dataset integrity — the SHA-256 digest of File 1, as recorded in the signed manifest:
```
b5f3324c38fa201c36c4b9a1b92d7466fa9c280309086a2d799b18095b3ce076
```

## 2. 30-Day Report — monthly

| | |
|---|---|
| **Window** | A complete 30-day UTC period (Edition V.1: September 9 – October 8, 2026) |
| **Scope** | Four x402 virtual hosts in service across the window |
| **Format** | PDF report and JSON Lines dataset |

**Bundle — 7 files:**

| File | Description |
|---|---|
| `MI-30Day-Report-V1.pdf` | The report: methodology, the three measures, the Index, Appendix A (daily table), Appendix B (settlements). |
| `MI-30day-FILE1-activity.jsonl` | The observational dataset — **306,799** request-level records, one JSON object per line (approximately 72 MB). |
| `APPENDIX-B-settlements.md` | The on-chain settlement ledger — **49** public transfers (24 Base, 25 Solana). |
| `DATA-DICTIONARY.md` | Field reference for the dataset. |
| `manifest-signed.json` | Ed25519-signed integrity manifest. |
| `README.txt` | Contents and verification steps. |
| `LICENSE.txt` | License and distribution terms (institutional use). |

Dataset integrity — the SHA-256 digest of File 1, as recorded in the signed manifest:
```
428b354e92b2e723f3ea8710142c2c3e1d60fb8255e7c632a7c5ebd55eaee26d
```

## 3. The dataset (File 1) schema

Both datasets share a single schema: newline-delimited JSON, one object per observed HTTP request, after deduplication and redaction (see [`METHODOLOGY.md`](METHODOLOGY.md) §7, §10). The fields present in every record follow — the structure, not the data.

| Field | Type | Definition |
|---|---|---|
| `ts` | string | Request timestamp, ISO-8601 UTC, to the second. |
| `ua` | string | User-Agent as transmitted — self-declared, may be spoofed, truncated to 120 characters. |
| `resource` | string | The requested resource. Underground products appear as stable `product_N` labels (the mapping is not distributed); MI and MAP API endpoints retain their names. |
| `method` | string | HTTP method. |
| `outcome` | string | HTTP status code (`200`, `402`, `404`, …). `402` denotes the x402 payment challenge. |
| `delivered` | boolean | An HTTP 200 on a paid path — the process completed, including free fulfillment. Fulfillment, not payment. |
| `paid` | boolean | An approximate row-level payment join. Secondary; the authoritative count is the settlement appendix. |
| `machine` | string | Pseudonymous source identifier: `m-` followed by the first 8 hexadecimal characters of `SHA-256(salt + source IP)`. One source address, not one actor. |
| `asn` / `net` / `region` | null | Reserved for later editions; not populated. |
| `wallet` | string / null | Payer wallet (lowercased) on settled rows; public on-chain data. House wallets are excluded. |

## 4. Integrity and verification

- The signed manifest (`manifest-signed.json`) records the dataset's SHA-256 digest, its record count, the settlement summary, and the public key (`key_id: underground-attest-2026-07-18`). A purchaser may recompute the digest and verify the Ed25519 signature to confirm the file is authentic and unaltered.
- The settlement appendix lists public transaction identifiers; each may be opened on `basescan.org` or `solscan.io` to confirm the corresponding on-chain payment.

See [`METHODOLOGY.md`](METHODOLOGY.md) §12 for the integrity and verification model.

## 5. Sample

One complete UTC day of the 30-Day dataset — **October 8, 2026, the closing day of that edition** — is provided in [`sample/`](sample/) (14,257 records; 3 rows carrying the row-level `paid` flag, against an authoritative 5 settlements for the day per Appendix B), identical in schema and redaction to the full dataset.

---

*Record counts, windows, and digests above describe the editions as published. Datasets, report documents, and signing keys are provided to purchasers and are not part of this repository.*
