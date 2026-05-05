# Quarterly UNTP Conformance Test Runs

This folder contains time-stamped re-executions of the UNTP test suite (`dppvalidator`) against the Reeco DPP Platform issuer endpoint.

## Cadence

Quarterly — at the end of each calendar quarter (Q1, Q2, Q3, Q4).

## Scope

Each run executes the same test set as the v1.0 baseline report:

- DPP fixtures: 6 test cases against UNTP DPP v0.6.1
- DCC fixtures: 16 structural checks against UNTP DCC v0.6.1
- Validator: `dppvalidator` v0.3.x (CIRPASS-2 reference) — version pinned per run
- Layers: SCH (Schema) + MDL (Model) + SEM (Semantic)

## File naming convention

```
REECO_UNTP_TEST_REPORT_<YYYY>_Q<N>.md
```

Example: `REECO_UNTP_TEST_REPORT_2026_Q3.md`

## Report structure

Each quarterly report includes:

- Run date (ISO 8601)
- `dppvalidator` version
- UNTP spec versions tested
- Pass/fail counts per credential type
- Performance metrics (avg validation latency)
- Diff vs v1.0 baseline (regressions, new test cases, spec updates)
- SHA-256 of the report file
- Cross-references to baseline report and fixtures

## Purpose

The baseline test report ([REECO_UNTP_TEST_REPORT_v1_0.md](../REECO_UNTP_TEST_REPORT_v1_0.md), run 2026-04-02) is a point-in-time validation of the Reeco implementation listed in the UN/CEFACT UNTP Software Implementations Register ([Merge Request !732](https://opensource.unicc.org/un/unece/uncefact/spec-untp/-/merge_requests/732)).

Quarterly re-runs convert the baseline into a continuous evidence chain over time. They demonstrate that the listed implementation remains operationally conformant after each calendar quarter, addressing the structural gap of self-declaration registries — where the listing alone does not guarantee continued technical conformance.

## Index of runs

| Quarter | Date | Validator | DPP | DCC | Report |
|---|---|---|---|---|---|
| Baseline | 2026-04-02 | dppvalidator v0.3.2 | 6/6 | 16/16 | [v1.0](../REECO_UNTP_TEST_REPORT_v1_0.md) |
| 2026 Q2 | TBD | TBD | TBD | TBD | TBD |
| 2026 Q3 | TBD | TBD | TBD | TBD | TBD |
| 2026 Q4 | TBD | TBD | TBD | TBD | TBD |

> Reports are added to this index after each quarterly execution.
