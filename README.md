# Indian Market Sentimeter — Institutional Capital Intelligence

A repository-backed, auditable research layer for institutional capital data relevant to Indian markets. The project is designed to complement—not replace or modify—the existing Sentimeter scanner or the 100-Point Hunter paper test.

## Project status

This repository is currently being initialized. The first implementation should be limited to **Phase 0 (source and repository preparation)** and the foundations for **Phase 1 (free, official, automatable sources)**. No data has been collected yet. No source should be scraped until its official endpoint, format, automation terms, and publication timing have been verified and recorded.

The repository is the intended single store for institutional history. Data should be append-only: corrections are new revisions, unavailable values are not represented as zero, and published/available timestamps—not just observation dates—govern when a value may be used in analysis.

## Guardrails

- Free and official sources only; no login-, CAPTCHA-, or paywall-gated scraping.
- Verify source terms and endpoint behavior before implementing each collector.
- Preserve existing Sentimeter behavior and avoid duplicate collectors.
- Keep cash flows, derivatives notionals, holdings changes, and valuation effects distinct.
- Label estimates and partial global-flow proxies with method and confidence; never present them as confirmed flows.
- Validate records before storage; malformed, stale, or duplicate data must not silently enter the history.
- Do not calculate predictive scores until there is sufficient sourced history and walk-forward tests.

## Initial implementation sequence

1. Inspect and inventory the repository and any existing collector/workflow/data files.
2. Verify Phase 1 source terms, endpoints, formats, and historical coverage; record findings in the source registry.
3. Establish shared record/schema validation and append-only revision storage.
4. Add or repair collectors one source family at a time, with fixtures and tests.
5. Add scheduled workflows only after a manual cloud run demonstrates that the source is reachable and the workflow safely persists validated output.
6. Build analysis and dashboard integration after reliable history exists.

## Phase 1 candidates

Subject to source verification: existing NSE FII/DII cash data and participant-wise derivatives; NSDL/SEBI FPI statistics; AMFI industry-level flows and SIP; quarterly exchange ownership disclosures; FRED macro series; USD/INR; RBI reserves; and CFTC positioning. These sources have different frequencies and lags and should have separate collectors/schedules. Paid global fund-flow products are out of scope.

## Data and interpretation

A change in AUM is not itself a flow. Reported flows and estimated flows must be distinguished. Futures/options notionals belong in a separate dataset from cash invested. Quarterly holdings changes are “changes in disclosed position,” not claims of purchases or sales. Missing values remain unavailable. Every value should carry source, source URL, collection and publication timestamps when known, period, currency, units, confidence, and revision metadata.

## Planned layout

```text
source_registry.json       Verified source inventory and status
schemas/                   Shared record contracts
collectors/                One source-family collector per module
 data/                     Append-only dataset history and freshness status
tests/fixtures/             Small fixed source samples for parser tests
.github/workflows/          Frequency-specific workflows, added after cloud validation
```

The precise layout should be reconciled with any existing repository files before adding collectors. JSON is the initial storage format; any move to SQLite/Parquet is deferred until the Phase 2 volume trade-off is reviewed.

## Limitations

Public disclosures do not provide a complete investor-to-destination money trail. Weekly global fund flows by country are generally licensed; any ETF/ICI-based proxy is partial and must be labelled accordingly. Historical analysis is limited to the period covered by verifiable source data; missing history must not be reconstructed or fabricated.
