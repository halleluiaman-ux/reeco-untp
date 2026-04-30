Hi — thanks for the pointer on GitLab. Access request is in; I'll open the issue against MR !732 myself as soon as I'm in.

In the meantime, here's the test report (run 2026-04-02 against `dppvalidator` v0.3.2, UNTP DPP/DCC 0.6.1). Posting the full document below so it's on record now and can be moved into the issue verbatim once tracking is set up.

---

## UNTP Implementation Test Report — Reeco

**Organisation:** Stefano Cipriani Studio / Reeco  
**Product:** Reeco DPP Platform  
**Report date:** 2026-04-02  
**UNTP scope tested:** DPP v0.6.1, DCC v0.6.1  
**Report version:** 1.0  
**Status:** Active

---

## 1. Implementation Overview

Reeco is a deep-tech SaaS platform for EU textile sustainability compliance, Digital Product Passport verification, and supply chain traceability, based in Prato, Italy. The platform serves textile brands, manufacturers, and certification bodies preparing for ESPR Digital Product Passport obligations.

Reeco implements UNTP as the credential format for DPP emission and Transaction Certificate export. The implementation covers:

- **DPP (Digital Product Passport):** issued at batch or item level for textile products, validated against UNTP DPP 0.6.1 schema before emission
- **DCC (Digital Conformity Credential):** exported from verified Transaction Certificates (GRS, RCS, OCS, GOTS), linked from DPP `conformityClaim` fields

### Industry focus

| Sector | Process | UNTP usage |
|---|---|---|
| Textiles (HS chapters 50–63) | DPP emission, TC verification, mass balance | DPP, DCC |
| Textile certification bodies | TC export as machine-readable credential | DCC |

### Implementation statement

Reeco implements UNTP DPP and DCC to bridge the gap between per-kg Transaction Certificate certification (GRS, GOTS, OCS, RCS) and the per-garment declarations required by ESPR. The platform applies a per-garment mass balance verification layer that reconciles kg-based Transaction Certificate volumes with unit-level DPP declarations. When the available certified volume does not cover the declared batch, the system informs the brand operator, who remains the sole decision-maker on DPP emission. The methodological gap between per-kg TC certification and per-unit ESPR declarations was formally documented in the CIRPASS-2 stakeholder survey (contribution ID: bb6997ac-957f-40c5-894c-9175f17ed682, January 2026). Reeco also integrates the CIRPASS-2 open-source validation engine (`dppvalidator` v0.3.2, artiso.ai, MIT license) as the primary DPP validation layer.

---

## 2. Test Environment

| Parameter | Value |
|---|---|
| Test date | 2026-04-02 |
| `dppvalidator` version | 0.3.2 |
| UNTP DPP schema | 0.6.1 |
| UNTP DCC schema | 0.6.1 |
| Python version | 3.12.3 |
| Validation engine layers | schema (SCH), model (MDL), semantic (SEM) |
| Validation mode | async-capable, non-strict |
| Test fixture type | Synthetic (no production data) |

---

## 3. Test Fixtures

### 3.1 DPP Fixture — `reeco_textile_dpp_0.6.1.json`

A synthetic textile Digital Product Passport representing a batch of linen/recycled polyester shirts. All data is fictitious and does not correspond to any real product, company, or Transaction Certificate.

**Product:** Linen Shirt — 55% Linen (IT), 45% GRS-certified Recycled Polyester (CN)  
**HS code:** 6205 (Men's shirts of other textile materials)  
**GTIN:** 0800501000000 (synthetic, valid checksum)  
**Granularity:** batch  
**Issuer:** `https://portal.reeco.eco/operators/reeco-test` (test operator)  
**Conformity claim:** GRS v4.0 — recycled polyester content 45%, TC linked via DCC  
**Circularity scorecard:** recycledContent 0.45, recyclableContent 0.45  

**Key structural properties verified:**

```json
"@context": [
  "https://www.w3.org/ns/credentials/v2",
  "https://test.uncefact.org/vocabulary/untp/dpp/0.6.1/"
],
"type": ["VerifiableCredential", "DigitalProductPassport"]
```

### 3.2 DCC Fixture — `reeco_grs_dcc_0.6.1.json`

A synthetic Digital Conformity Credential representing a GRS Transaction Certificate export. All data is fictitious.

**TC reference:** RCO-TC-2026-001 (synthetic)  
**Scheme:** GRS v4.0 — `https://textileexchange.org/standards/grs`  
**Certifying body:** Control Union Certifications BV (synthetic)  
**Certified volume:** 500 kg recycled polyester staple fibre  
**Conformance:** true  
**Auditable evidence:** SHA-256 hash of Reeco ledger entry  

**Key structural properties verified:**

```json
"@context": [
  "https://www.w3.org/ns/credentials/v2",
  "https://vocabulary.uncefact.org/untp/dcc/0.6.1/"
],
"type": ["VerifiableCredential", "DigitalConformityCredential"]
```

---

## 4. DPP Conformance Test Results

All test cases executed against `dppvalidator` v0.3.2, layers: schema + model + semantic.

### 4.1 Test Case Summary

| ID | Description | Expected | Result | Match |
|---|---|---|---|---|
| TC1 | Full textile DPP — valid | PASS | PASS ✅ | ✅ |
| TC2 | DPP without `materialsProvenance` | PASS (optional at UNTP core) | PASS ✅ | ✅ |
| TC3 | `massFraction` sum = 1.25 | FAIL | FAIL ✅ | ✅ |
| TC4 | Missing `issuer` field | FAIL | FAIL ✅ | ✅ |
| TC5 | `validFrom` after `validUntil` | FAIL | FAIL ✅ | ✅ |
| TC6 | `conformityClaim` without `referenceStandard` | PASS (schema valid) | PASS ✅ | ✅ |

**Result: 6/6 test cases match expected behaviour.**

### 4.2 Test Case Detail

#### TC1 — Full textile DPP, valid

```
Input:  reeco_textile_dpp_0.6.1.json
Valid:  true
Schema version detected: 0.6.1
Errors: 0
Warnings: 0
Avg validation time (10 runs): 3.82 ms
Min: 3.16 ms / Max: 6.02 ms
```

All three validation layers pass:
- SCH: JSON Schema UNTP DPP 0.6.1 — PASS
- MDL: Pydantic model — PASS (GTIN checksum valid, dates ordered, massFractions ≤ 1.0)
- SEM: Business rules — PASS (conformance claim present, massFractions sum = 1.0)

#### TC2 — DPP without `materialsProvenance`

```
Input:  DPP with materialsProvenance removed
Valid:  true
Errors: 0
```

Correct behaviour: `materialsProvenance` is optional at UNTP DPP 0.6.1 core schema level. The CIRPASS-2 textile sector rule TXT002 would catch this at the plugin layer (not tested here as part of UNTP core conformance). This distinction between UNTP core conformance and sector-specific requirements is intentional.

#### TC3 — Invalid mass fractions (sum > 1.0)

```
Input:  massFraction[0]=0.80, massFraction[1]=0.45 (sum=1.25)
Valid:  false
Layer:  model (MDL)
Code:   MDL002
Error:  "Material mass fractions cannot exceed 1.0, got 1.250"
```

Correct behaviour: a mass fraction sum > 1.0 is physically impossible and is caught as a hard error at model validation.

#### TC4 — Missing `issuer`

```
Input:  DPP with issuer field removed
Valid:  false
Errors:
  SCH001 [schema]:  'issuer' is a required property
  MDL001 [model]:   Field required
```

Correct behaviour: `issuer` is mandatory in W3C VCDM and UNTP DPP. Two independent layers (schema and model) both catch the omission.

#### TC5 — `validFrom` after `validUntil`

```
Input:  validFrom="2030-01-01T00:00:00Z", validUntil="2026-01-01T00:00:00Z"
Valid:  false
Layer:  model (MDL)
Code:   MDL002
Error:  "validFrom must be before validUntil"
```

Correct behaviour: date ordering enforced at model validation layer (SEM002 also covers this at semantic layer).

#### TC6 — `conformityClaim` without `referenceStandard`

```
Input:  conformityClaim[0] with referenceStandard removed
Valid:  true
Errors: 0
```

Correct behaviour: UNTP DPP 0.6.1 does not mandate `referenceStandard` in a conformity claim at schema level. The claim remains valid (though incomplete). Reeco's proprietary layer (RCO001) enforces TC certificate completeness at the platform level — this is not part of UNTP core conformance.

---

## 5. DCC Conformance Test Results

`dppvalidator` v0.3.2 validates DPP credentials only. DCC conformance was verified manually against the UNTP DCC 0.6.1 specification (`https://uncefact.github.io/spec-untp/docs/specification/ConformityCredential/`).

### 5.1 DCC Structural Conformance — `reeco_grs_dcc_0.6.1.json`

| Check | Field | Value | Status |
|---|---|---|---|
| W3C VC envelope | `@context[0]` | `https://www.w3.org/ns/credentials/v2` | ✅ PASS |
| UNTP DCC context | `@context[1]` | `https://vocabulary.uncefact.org/untp/dcc/0.6.1/` | ✅ PASS |
| Credential type | `type` | `["VerifiableCredential", "DigitalConformityCredential"]` | ✅ PASS |
| Credential ID | `id` | Resolvable HTTPS URI | ✅ PASS |
| Issuer | `issuer.id` | HTTPS URI identifying issuing operator | ✅ PASS |
| Validity from | `validFrom` | ISO 8601 datetime | ✅ PASS |
| Validity until | `validUntil` | ISO 8601 datetime, after validFrom | ✅ PASS |
| Attestation type | `credentialSubject.type` | `ConformityAttestation` | ✅ PASS |
| Conformity scheme | `credentialSubject.scope` | Scheme with `id` URL and `name` | ✅ PASS |
| Issued to party | `credentialSubject.issuedToParty` | Party with `id` and `name` | ✅ PASS |
| Assessment array | `credentialSubject.assessment` | Non-empty array | ✅ PASS |
| Declared value | `assessment[0].declaredValue` | Volume in KGM + percentage in P1 | ✅ PASS |
| Conformance flag | `assessment[0].conformance` | `true` | ✅ PASS |
| Conformity evidence | `assessment[0].conformityEvidence` | SecureLink with hashDigest SHA-256 | ✅ PASS |
| Auditable evidence | `credentialSubject.auditableEvidence` | SecureLink with hashDigest SHA-256 | ✅ PASS |
| Authorisation | `credentialSubject.authorisation` | Certifying body endorsement | ✅ PASS |

**Result: 16/16 DCC structural checks pass.**

### 5.2 Reeco-specific DCC properties

The Reeco DCC export includes two properties not required by UNTP core but significant for ESPR compliance:

**Ledger integrity reference:** The `auditableEvidence.hashDigest` field contains a SHA-256 hash of the Reeco TC verification ledger entry. This enables any auditor to verify that the DCC was issued against a specific, immutable ledger state — satisfying the ESPR requirement for traceable, verifiable evidence chains.

**Textile Exchange scheme URLs:** Each supported TC scheme (GRS, RCS, OCS, GOTS) is mapped to its official Textile Exchange URL in the `conformityScheme.id` field, enabling scheme resolution without prior knowledge of the certification system.

These properties are additive to UNTP core and do not conflict with the schema (`additionalProperties` are permitted in most UNTP objects).

---

## 6. Validation Performance

| Metric | Value |
|---|---|
| Average validation time (DPP, 10 runs) | 3.82 ms |
| Minimum validation time | 3.16 ms |
| Maximum validation time | 6.02 ms |
| Layers executed | schema + model + semantic (3 of 7) |
| Input size (DPP fixture) | ~5.2 KB |
| Concurrency model | async (`validate_async` via `asyncio.to_thread`) |
| Batch support | up to 100 DPP per request |

Note: Production Reeco portal runs a 7-layer validation stack including proprietary rules (RCO001–004) with validation latency in the low-millisecond range per DPP.

---

## 7. UNTP Specification Alignment Notes

### 7.1 Context URL

The Reeco implementation uses `https://test.uncefact.org/vocabulary/untp/dpp/0.6.1/` as the DPP context URI, as specified in the UNTP DPP 0.6.1 JSON Schema `@context` default. For DCC, `https://vocabulary.uncefact.org/untp/dcc/0.6.1/` is used per the DCC specification.

### 7.2 Materials declaration — core vs. sector

`materialsProvenance` is optional at UNTP core DPP schema level (TC2 confirms this). For textile products, Reeco enforces material composition declaration at the platform layer (rule TXT002 per CIRPASS-2 textile sector requirements). This is the correct architectural separation: UNTP core handles structural conformance; sector extensions handle sector-specific requirements.

### 7.3 Mass balance — GRS to DPP bridging

Reeco implements a proprietary per-garment mass balance reconciliation layer that bridges the structural gap between per-kg Transaction Certificate certification and per-unit UNTP DPP declarations. This layer operates at the platform level and does not affect UNTP core schema conformance. When reconciliation indicates that certified volume does not fully cover declared production, the system surfaces the finding to the brand operator, who retains full autonomy over DPP emission decisions. The methodological gap analysis was contributed to CIRPASS-2 (contribution ID bb6997ac-957f-40c5-894c-9175f17ed682).

### 7.4 Known limitations

- **Signature verification:** The test fixtures do not include cryptographic proofs (no `proof` field). Reeco's production DCC export signs with HS256 JWT. Migration to `did:web` + Ed25519 linked data proofs is planned for v2 of the DCC export — this will enable full W3C VC signature verification via `dppvalidator`'s signature layer.
- **JSON-LD expansion:** The JSON-LD validation layer (JLD) was not included in this test run as it requires network access to resolve context URLs. All context URIs used are per-specification.
- **Vocabulary validation:** External vocabulary validation (VOC layer — UNECE Rec 46 code validation, HS code lookup) was not included in this report. VOC layer is available in the Reeco production stack.

---

## 8. API Endpoints

| Endpoint | Method | Description |
|---|---|---|
| `/api/v1/dpp/validate` | POST | Single DPP validation, returns `cirpass2_compliant` and `reeco_compliant` flags |
| `/api/v1/dpp/validate/batch` | POST | Batch validation up to 100 DPP, parallel concurrency 10 |
| `/api/v1/dpp/export/dcc` | POST | TC → UNTP DCC 0.6.1 (JSON-LD) |
| `/api/v1/dpp/export/dcc/jwt` | POST | TC → UNTP DCC 0.6.1 (JWT-signed) |
| `/api/v1/dpp/info` | GET | Validation engine status, plugin list, schema version |

Base URL: `https://portal.reeco.eco`  
Authentication: Bearer token (portal operators)

---

## 9. References

| Resource | URL |
|---|---|
| UNTP DPP 0.6.1 specification | https://uncefact.github.io/spec-untp/docs/specification/DigitalProductPassport/ |
| UNTP DCC 0.6.1 specification | https://uncefact.github.io/spec-untp/docs/specification/ConformityCredential/ |
| UNTP test suite | https://uncefact.github.io/tests-untp/ |
| dppvalidator (artiso.ai) | https://github.com/artiso-ai/dppvalidator |
| CIRPASS-2 project | https://cirpass2.eu/ |
| Reeco platform | https://portal.reeco.eco |
| Reeco knowledge base | https://ia.reeco.eco/knowledge |
| CIRPASS-2 survey contribution | ID bb6997ac-957f-40c5-894c-9175f17ed682 |
| Zenodo DOI (Durability/Repairability/Waste Index) | https://doi.org/10.5281/zenodo.19206499 |
| UNTP Software Register entry | https://uncefact.github.io/spec-untp/docs/implementations/Software |

---

## 10. Contact

**Stefano Cipriani**  
Founder, Stefano Cipriani Studio / Reeco  
ORCID: 0009-0001-3423-9402  
Wikidata: Q138773743  
Email: admin@reeco.eco  
Location: Prato, Italy  
CIRPASS-2 Expert Member: EWG1, EWG3, EWG5  
JRC Registered Stakeholder: Unit B5  
Patent: CN113529235 (hemp fibre processing, Solucell®/COPET)
