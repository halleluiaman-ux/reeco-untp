# Reeco — UNTP Implementation

[![UNTP Software Register](https://img.shields.io/badge/UN%2FCEFACT-UNTP%20Software%20Register-1f6feb)](https://opensource.unicc.org/un/unece/uncefact/spec-untp/-/merge_requests/732)
[![CIRPASS-2 EWG1](https://img.shields.io/badge/CIRPASS--2-EWG1%20Expert%20Member-2ea44f)](https://cirpass2.eu/expert-working-group-1/)
[![CIRPASS-2 EWG3](https://img.shields.io/badge/CIRPASS--2-EWG3%20Expert%20Member-2ea44f)](https://cirpass2.eu/expert-working-group-3/)
[![JRC Registered Stakeholder](https://img.shields.io/badge/JRC-Registered%20Stakeholder-555)](https://joint-research-centre.ec.europa.eu/)
[![ESPR 2028 Ready](https://img.shields.io/badge/ESPR%202028-Ready-orange)]()

UNTP (UN Transparency Protocol) implementation artefacts for **Reeco®**, the EU textile Digital Product Passport platform.

---

## Test Results

| Credential type | Fixtures | Pass rate |
|---|---|---|
| Digital Product Passport (DPP) v0.6.1 | 6 | 6 / 6 ✅ |
| Digital Conformity Credential (DCC) v0.6.1 | 16 | 16 / 16 ✅ |

- **Test report**: [docs/test-reports/REECO_UNTP_TEST_REPORT_v1_0.md](docs/test-reports/REECO_UNTP_TEST_REPORT_v1_0.md)
- **DPP fixture**: [test-report/fixtures/reeco_textile_dpp_0.6.1.json](test-report/fixtures/reeco_textile_dpp_0.6.1.json)
- **DCC fixture**: [test-report/fixtures/reeco_grs_dcc_0.6.1.json](test-report/fixtures/reeco_grs_dcc_0.6.1.json)

---

## Institutional status

| Asset | Reference |
|---|---|
| **UNTP Software Register entry** | [Merge Request !732 — merged April 2026](https://opensource.unicc.org/un/unece/uncefact/spec-untp/-/merge_requests/732) |
| **UNTP scope** | DPP v0.6.1, DCC v0.6.1 |
| **Sector focus** | Textiles (HS 50–63) |
| **Validator** | `dppvalidator` v0.3.2 (CIRPASS-2 reference validator) |
| **Layers tested** | Schema (SCH), Model (MDL), Semantic (SEM) |
| **Run date** | 2026-04-02 |
| **Report SHA-256** | `7f87856b49e6d1b642b7f57f78f5d6609e77ac5dbfb0a49b48bb5d76e478a0e7` |

---

## Implementation scope

- **UNTP DPP v0.6.1** — per-garment Digital Product Passport aligned with ESPR 2028 requirements
- **UNTP DCC v0.6.1** — Digital Conformity Credential for GRS, RCS, OCS, GOTS Transaction Certificates
- **W3C Verifiable Credentials** — credentials cryptographically signed with Ed25519
- **GS1 Digital Link** — compatible identification scheme, coordinated with EWG3 (GTS) for UPI convergence
- **EPCIS** — conceptual reference for forensic verification event model

The platform bridges the structural mismatch between **batch-level certifications** (kg-based, 90-day periods, e.g. GRS) and **per-unit declarations** required by **EU ESPR** and the textile-specific Delegated Act on Digital Product Passport. The methodological contribution to this gap is documented in the CIRPASS-2 stakeholder consultation (contribution ID `bb6997ac`, January 2026).

---

## Author and institutional credentials

**Stefano Cipriani** — founder, Stefano Cipriani Studio, Prato, Italy.

- CIRPASS-2 Expert Member — Expert Working Groups EWG1, EWG3, EWG5
- JRC Registered Stakeholder — Unit B5, Seville
- UN/CEFACT UNTP Software Register — listed implementer (MR !732)
- ORCID: [0009-0001-3423-9402](https://orcid.org/0009-0001-3423-9402)
- Wikidata: [Q138773743](https://www.wikidata.org/wiki/Q138773743)
- Zenodo DOI: [10.5281/zenodo.19206499](https://doi.org/10.5281/zenodo.19206499)
- Substack: [stefanocipri.substack.com](https://stefanocipri.substack.com)

---

## Legal entity

**Stefano Cipriani Studio**

- Legal Entity Identifier (LEI): [`8156007B2A5194A30603`](https://search.gleif.org/#/record/8156007B2A5194A30603)
- VAT (P.IVA): `IT02391280977`
- ATECO: `62.10.00` — Software production, consulting, and related activities (added 2026-04-09)
- Italian software deposit: SIAE `D000029472` — *Sistema Reeco di Verifica Forense v1.0*
- Registered office: Prato, Tuscany, Italy
- Email: [admin@reeco.eco](mailto:admin@reeco.eco)
- PEC: [stefano.cipriani.studio@pec.it](mailto:stefano.cipriani.studio@pec.it)

---

## Repository contents

```
docs/
└── test-reports/
    └── REECO_UNTP_TEST_REPORT_v1_0.md       # Full conformance test report

test-report/
└── fixtures/
    ├── reeco_textile_dpp_0.6.1.json         # DPP fixture (textile, HS 50–63)
    └── reeco_grs_dcc_0.6.1.json             # DCC fixture (GRS Transaction Certificate)
```

---

## Related public resources

- Reeco platform: [reeco.eco](https://reeco.eco) · [portal.reeco.eco](https://portal.reeco.eco)
- Substack (technical analysis on EU DPP and ESPR): [stefanocipri.substack.com](https://stefanocipri.substack.com)
- Durability Index v1.02 and Repairability Index v3.1: ResearchGate (submitted to JRC Seville, Unit B5)

---

## License and trademarks

Test artefacts and report are released under the terms specified in the repository `LICENSE` file. Trademarks (Reeco®, Stefano Cipriani Studio) and proprietary algorithms (mass balance verification method) are not part of the public release.

---

## Status

**Active** — last updated April 2026.

> *CIRPASS-2 is a European Commission Coordination and Support Action (CSA) that produces recommendations on the Digital Product Passport framework. It is not a certification or standardisation body.*
