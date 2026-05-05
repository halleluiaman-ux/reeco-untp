# Reeco — The Structural Gap Between Batch Certification and Per-Unit DPP

## The problem

EU regulations on textile sustainability claims operate at two incompatible levels of granularity.

**Upstream — material certifications:**
Schemes such as the Textile Exchange Global Recycled Standard (GRS), Recycled Claim Standard (RCS), Organic Content Standard (OCS), and Global Organic Textile Standard (GOTS) operate on **mass-balance accounting in kilograms over rolling periods**, typically 90 days. A Transaction Certificate (TC) certifies that a defined quantity of certified material has flowed through a specific tier of the supply chain. The unit of certification is the kilogram-batch, not the finished garment.

**Downstream — Digital Product Passport requirements:**
The EU Ecodesign for Sustainable Products Regulation (ESPR, EU 2024/1781) and, in particular, the textile-specific Delegated Act on the Digital Product Passport, require **per-unit declarations**. Each individual garment placed on the EU market must carry a DPP that asserts, among other attributes, the recycled content percentage and the certifying scheme. The unit of declaration is the single physical product.

Between the two levels there is a **structural mismatch**: a 90-day TC declared in kilograms cannot, by itself, support a per-garment claim. Bridging the two requires a verifiable mass balance method that operates **at the level of the individual production lot** and produces, for each garment, a defensible link back to the TC kilograms consumed.

## What Reeco does

Reeco DPP Platform implements a **proprietary mass balance verification method** that converts batch-level TC kilograms into per-garment claim coverage. The method is applied at DPP issuance time.

Where TC coverage is insufficient for the declared production lot, the platform **does not block DPP emission**. It informs the brand that mass balance does not cover the full declared quantity, and lets the brand decide how to proceed — for example, emitting DPPs with downgraded claims for the uncovered units (a "100% recycled cotton GRS" claim becomes a plain "100% cotton" declaration on those specific units), or pausing emission until additional TC documentation is uploaded. The decision authority remains with the economic operator placing the product on the market. Reeco provides the evidence layer; the commercial decision belongs to the brand.

The cryptographic backbone of each issuance is anchored to a verification ledger entry, whose SHA-256 hash is embedded in the issued DCC under `auditableEvidence.hashDigest`. This enables any auditor — internal, external, or regulatory — to verify that a given DPP was issued against a specific, immutable ledger state.

## Why this matters for ESPR compliance

Brands that emit DPPs without a per-unit mass balance bridge are exposed to two compliance risks:

1. **Greenwashing liability under the EU Green Claims Directive (ECGT, EU 825/2024)** — declaring "100% recycled cotton GRS" on a per-garment DPP without traceable per-unit evidence to back the claim. Sanctions under ECGT can reach 4% of annual turnover.
2. **Audit failure under ESPR market surveillance** — when authorities request the evidence chain linking a specific garment to a specific TC, only per-unit-resolved methods can answer.

Self-declarations and aggregate annual reports do not satisfy per-garment evidentiary requirements. The bridge has to operate at the lot level, automatically, and at scale.

## Adjacent regulatory frameworks

The same per-unit evidence architecture that satisfies ESPR is structurally aligned with three additional regulatory regimes:

- **CSRD** (Corporate Sustainability Reporting Directive) — auditable, granular sustainability data at product level
- **CSDDD** (Corporate Sustainability Due Diligence Directive) — supply chain due diligence with documented evidence chains
- **EU Forced Labour Regulation** (2024/3015) and **US Uyghur Forced Labor Prevention Act (UFLPA)** — origin verification at lot level, supporting rebuttable-presumption rebuttals with forensic-grade traceability

Per-unit mass balance is not a single-regulation feature. It is the architectural primitive that all four regulatory regimes converge on.

## Intellectual property protection

The proprietary mass balance verification method implemented in Reeco DPP Platform is protected under Italian law as a registered software work:

- **Deposit reference:** SIAE `D000029472`
- **Title:** *Sistema Reeco di Verifica Forense v1.0*
- **Deposit date:** 28 April 2026
- **Depositor:** Stefano Cipriani Studio (VAT IT02391280977)

The deposit was made with the Società Italiana degli Autori ed Editori (SIAE), the Italian collective rights management organisation authorised under Italian Law 633/1941 to register software works. The SIAE software deposit constitutes an act of public faith with certified date (*data certa*), opposable to third parties under Italian Civil Code Art. 2704. It establishes priority of authorship and date of creation for the protected work.

The legitimacy of Stefano Cipriani Studio as a software-producing legal entity is reinforced by the registration of ATECO code `62.10.00` (*Software production, consulting, and related activities*) at the Chamber of Commerce of Prato, effective from 1 January 2026 (filed 9 April 2026). This formal classification supports both intellectual property protection and access to the Italian Patent Box regime.

The trademark **REECO®** is registered in Nice classes 24 and 25 in the European Union, United Kingdom, Canada, USA, Japan, Australia, Turkey, and class 25 in China.

## Institutional anchoring

This methodological contribution was formally documented in the **CIRPASS-2 stakeholder consultation** (contribution ID `bb6997ac`, January 2026). CIRPASS-2 is the European Commission Coordination and Support Action that produces recommendations on the Digital Product Passport framework. CIRPASS-2 is not a certification or standardisation body; it is a recommendation-producing CSA.

Reeco is a listed implementer in the **UN/CEFACT UNTP Software Implementations Register** ([Merge Request !732](https://opensource.unicc.org/un/unece/uncefact/spec-untp/-/merge_requests/732), merged April 2026). Public conformance test results: [Reeco UNTP Test Report v1.0](docs/test-reports/REECO_UNTP_TEST_REPORT_v1_0.md) — DPP 6/6, DCC 16/16 pass against UNTP v0.6.1, validated with `dppvalidator` v0.3.2 (CIRPASS-2 reference validator) across Schema, Model, and Semantic layers.

## Scope of public artefacts

This repository contains:

- Public conformance test fixtures and reports against UN/CEFACT UNTP v0.6.1
- Quarterly re-attestation runs (when established) at [docs/test-reports/quarterly-runs/](docs/test-reports/quarterly-runs/)
- Institutional cross-references: legal entity identifiers, persistent identifiers, regulatory submissions

This repository **does not** contain:

- The proprietary mass balance verification method (trade secret of Stefano Cipriani Studio, deposited at SIAE under reference `D000029472` — *Sistema Reeco di Verifica Forense v1.0*)
- The hardware specifications of the verification ledger infrastructure
- Brand-specific implementation data, datasets, or audit trails

The methodological intent is documented; the implementation is protected.

## Related documents and identifiers

- UNTP Software Implementations Register entry: [Merge Request !732](https://opensource.unicc.org/un/unece/uncefact/spec-untp/-/merge_requests/732)
- Reeco UNTP Test Report v1.0: [docs/test-reports/REECO_UNTP_TEST_REPORT_v1_0.md](docs/test-reports/REECO_UNTP_TEST_REPORT_v1_0.md)
- Author Zenodo deposits:
  - [Algorithmic Material Exhaustion Control in Textile Digital Product Passport Verification](https://doi.org/10.5281/zenodo.19206500) (working paper, March 2026)
  - [Durability Index — A Standardised Framework for the Assessment of Physical Durability of Textile Apparel](https://doi.org/10.5281/zenodo.20034567) (standard, May 2026)
- Wikidata items:
  - [Reeco (Q138773743)](https://www.wikidata.org/wiki/Q138773743)
  - [Stefano Cipriani Studio (Q139669450)](https://www.wikidata.org/wiki/Q139669450)
- ORCID: [0009-0001-3423-9402](https://orcid.org/0009-0001-3423-9402)
- GLEIF Legal Entity Identifier: [8156007B2A5194A30603](https://search.gleif.org/#/record/8156007B2A5194A30603)

---

*Stefano Cipriani Studio — Prato, Tuscany, Italy*
*VAT IT02391280977 · ATECO 62.10.00 · SIAE D000029472 · LEI 8156007B2A5194A30603*
*PEC: stefano.cipriani.studio@pec.it · Email: admin@reeco.eco*
