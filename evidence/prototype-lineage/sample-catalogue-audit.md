# Sample Product Catalogue Data Quality Audit

_Portfolio demonstration using fictional construction-product records. No client or Transmat data is included._

## Executive Summary

Ten sample rows produced **13 logged issues**: one likely duplicate pair, three missing required values, four normalization problems, two price anomalies and three records requiring business confirmation. Eight issues can be corrected deterministically; five need evidence or owner judgment.

## Before / After Extract

| SKU | Original product | Original unit | Original price | Normalized product | Normalized unit | Price | Status |
|---|---|---|---:|---|---|---:|---|
| CEM-001 | Cement grey 25 KG | bag | 14.50 | Grey Cement, 25 kg | BAG | 14.50 | Cleaned |
| cem001 | Cement gray 25kg | pcs | 14,50 | Grey Cement, 25 kg | BAG | 14.50 | Probable duplicate—confirm merge |
| INS-014 | EPS 5см 10м2 | pack | 52.00 | EPS Insulation, 50 mm, 10 m² | PACK | 52.00 | Cleaned |
| INS-015 | EPS 50mm | pack | 5.20 | EPS Insulation, 50 mm | PACK | 5.20 | Confirm pack coverage / price basis |
| ADH-008 | Tile glue flex 25kg | — | 22.90 | Flexible Tile Adhesive, 25 kg | — | 22.90 | Missing unit |

## Issue Log Extract

| Issue ID | SKU | Type | Evidence | Proposed action | Confidence |
|---|---|---|---|---|---|
| DQ-001 | CEM-001 / cem001 | Duplicate candidate | Normalized SKU and equivalent name/weight; unit conflicts | Confirm same product, then merge and retain canonical SKU | High candidate; client decision required |
| DQ-002 | INS-014 | Unit normalization | Cyrillic `см`, compact area text | Normalize 5 cm to 50 mm and 10м2 to 10 m² | High |
| DQ-003 | INS-015 | Price anomaly | Tenfold difference from adjacent 50 mm EPS record | Verify whether price is per sheet vs per pack | Medium; do not auto-correct |
| DQ-004 | ADH-008 | Missing required value | Unit empty | Obtain selling unit from supplier/master record | Certain issue; value unknown |
| DQ-005 | CEM-001 | Spelling/category standard | “grey/gray” variants | Use catalogue standard “Grey”; preserve original in audit trail | High |

## Rules Applied

- Preserve original SKU in a separate audit column.
- Uppercase canonical SKUs; remove no punctuation unless the client rule permits it.
- Normalize mass to `kg`, length to `mm`, area to `m²`; never convert a selling unit without evidence.
- Flag price deviations; do not infer a corrected price from neighboring products.
- Separate deterministic corrections from business decisions.

## Client Decisions Required

1. Are `CEM-001` and `cem001` the same sellable item and barcode?
2. Is `INS-015` priced per sheet, per square metre or per pack?
3. What is the approved selling unit for `ADH-008`?

## Delivery QA

- Record count reconciled before and after cleanup.
- No identifier deleted.
- All changed fields trace to an issue ID.
- Ambiguous values remain flagged rather than invented.

