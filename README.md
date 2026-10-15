# Maraventano Index

**An independent, signed measurement of change in purchase-attempt demand on [x402](https://x402.org), over time, observed from the merchant seat.**

The Maraventano Index records HTTP request activity against a set of live x402 commerce endpoints and publishes a single value tracking how comparable purchase-attempt demand moves over time. It is a measurement, produced by a fixed, published rule — not an analysis, a forecast, or a claim as to the identity of any actor. The underlying record is continuous from April 6, 2026.

- Site: **https://maraventanoindex.com**
- Contact: **research@maraventanoindex.com**

This repository is the public record of how the Index is computed and what the published products contain.

## Contents

| Path | Description |
|---|---|
| [`METHODOLOGY.md`](METHODOLOGY.md) | The computation rule — definitions, formula, the fixed baseline, like-for-like treatment, the two logging regimes, noise classification, data protection, and exclusions. |
| [`DELIVERABLES.md`](DELIVERABLES.md) | The structure of each published product — files, record counts, formats, and integrity. |
| [`sample/`](sample/) | One complete day of the observational dataset (October 8, 2026), identical in schema and redaction to the full editions. |

## Not included in this repository

- The full datasets (only the single-day sample in [`sample/`](sample/)).
- The report documents.
- Any signing key, token, or credential.

The full editions are provided to purchasers; their structure is documented in [`DELIVERABLES.md`](DELIVERABLES.md).

## Record and vantage

The merchant seat has operated continuously since April 6, 2026 — an unbroken, longitudinal record. It cannot be reconstructed after the fact: request activity not observed as it occurred is not recoverable, and the archive is therefore not reproducible by a later entrant. From that seat the Index observes the full request stream — every purchase attempt, not only the payments that settle.

Each dataset is accompanied by an Ed25519-signed manifest confirming that the file is authentic and unaltered. The settlements the Index reports are matters of public on-chain record, independently verifiable by any party. The method, and the basis of each guarantee, are set out in [`METHODOLOGY.md`](METHODOLOGY.md).

---

Maraventano Index · maraventanoindex.com
