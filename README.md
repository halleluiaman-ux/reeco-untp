# Reeco — UNTP Implementation

[![UNTP Software Register](https://img.shields.io/badge/UN%2FCEFACT-UNTP%20Software%20Register-004990)](https://opensource.unicc.org/un/unece/uncefact/spec-untp/-/merge_requests/732)
[![CIRPASS-2](https://img.shields.io/badge/CIRPASS--2-Expert%20Member%20EWG1%C2%B73%C2%B75-6A9B35)](https://cirpass2.eu)
[![JRC Registered Stakeholder](https://img.shields.io/badge/JRC-Registered%20Stakeholder-ffa500)](https://joint-research-centre.ec.europa.eu)
[![ESPR 2028 Ready](https://img.shields.io/badge/ESPR-2028%20Ready-3BC71A)](https://reeco.eco/espr-compliance/)

UNTP (UN Transparency Protocol) implementation artifacts for **Reeco®**, the EU textile Digital Product Passport platform.

## Test Results

| Credential type | Fixtures | Pass rate |
|---|---|---|
| Digital Product Passport (DPP) v0.6.1 | 6 | **6 / 6** ✅ |
| Digital Conformity Credential (DCC) v0.6.1 | 16 | **16 / 16** ✅ |

- **Test Report:** [`REECO_UNTP_TEST_REPORT_v1.0.md`](test-report/REECO_UNTP_TEST_REPORT_v1.0.md)
- **DPP fixture:** [`reeco_textile_dpp_0.6.1.json`](test-report/fixtures/reeco_textile_dpp_0.6.1.json)
- **DCC fixture:** [`reeco_grs_dcc_0.6.1.json`](test-report/fixtures/reeco_grs_dcc_0.6.1.json)

## Implementation scope

- **UNTP DPP** v0.6.1 — per-garment Digital Product Passport aligned with ESPR 2028 requirements
- **UNTP DCC** v0.6.1 — Digital Conformity Credential for GRS, RCS, OCS Transaction Certificates
- **W3C Verifiable Credentials** — all credentials cryptographically signed with Ed25519
- **GS1 Digital Link** — compatible identification scheme, coordinated with EWG3 (GTS) for UPI convergence
- **EPCIS** — conceptual reference for forensic verification event model

## UN/CEFACT Software Register

Reeco is listed in the UN/CEFACT UNTP Software Register via [MR !732](https://opensource.unicc.org/un/unece/uncefact/spec-untp/-/merge_requests/732) (branch `feat/reeco-register`, approved April 2026).

## Organisation

**Stefano Cipriani Studio** · Reeco®
Prato, Italy · P.IVA IT02391280977

- **Website:** [reeco.eco](https://reeco.eco)
- **Contact:** admin@reeco.eco
- **PEC:** stefano.cipriani.studio@pec.it
- **ORCID:** [0009-0001-3423-9402](https://orcid.org/0009-0001-3423-9402)
- **Zenodo DOI:** [10.5281/zenodo.19206499](https://doi.org/10.5281/zenodo.19206499)

## Institutional role

- CIRPASS-2 Expert Member — Expert Working Groups **EWG1, EWG3, EWG5**
- JRC Registered Stakeholder — Unit B5
- UN/CEFACT UNTP Software Register — listed implementer

## Status

**Active** — Last updated April 2026.

---

*CIRPASS-2 is a European Commission Coordination and Support Action that produces recommendations on the Digital Product Passport framework. It is not a certification or standardisation body.*

