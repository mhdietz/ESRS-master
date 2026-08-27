# DMA Mapping Tool — Update Log: topical standards rebuilt from adopted Delegated Act (C(2026) 5010)

**Date:** 28 July 2026
**Basis:** Commission Delegated Regulation C(2026) 5010, adopted 3 July 2026 (revised ESRS). Topical standards (E1–E5, S1–S4, G1) rebuilt from the adopted texts uploaded from the EFRAG Knowledge Hub. This pass follows the cross-cutting rewrite logged in `CHANGELOG_v2_cross-cutting.md`.

## Files updated
- `ESRS_Knowledge_Base_Simplified_v2.md` → **v2.1** (Part 3 topical DR tables, Part 4 sub-topic→DR cross-references, Part 6 rationale templates, Part 7 flags, headers/footer)
- `Gem_Worked_Examples_v2.md` → **v2.1** (E1-2 and E1-8 JSON blocks; `ghg_boundary_approach` default; schema-field completeness)
- `post_omnibus_mapping_matrix_v3.md` → **v3.1** (DR columns, sub-topic descriptors, structural notes)

## What changed and why it matters

1. **Part 3 rebuilt from adopted text.** All ten topical DR tables rewritten from the adopted standards, with disclosure objectives and key content sourced to adopted paragraph and Application-Requirement numbers. The blanket `⚠ VERIFY` banner over Part 3 is removed.

2. **E1-2 always-mandatory claim — RESOLVED (killed).** The draft-era "E1-2 is mandatory regardless of materiality per ESRS 2 Appendix C" is wrong on both counts. There is no Appendix C basis, and adopted E1-2 ("Identification of climate-related risks and scenario analysis", paras 14–17) is a materiality-conditional risk-identification DR whose scenario-analysis element is itself conditional (para 17, "if used"). E1-2 is now treated as a normal materiality-conditional DR throughout KB §1.2, the E1 section, Part 6, and worked Example 3.

3. **E1-8 GHG boundary — RESOLVED (financial control confirmed).** Adopted E1 AR 19 sets the reporting boundary as **financial control** per the GHG Protocol Corporate Accounting and Reporting Standard (2004), with **equity share** or **operational control** as permitted alternatives; ESRS 1 paras 71/72 and AR 36 (joint operations) prevail, and E1-8 is carved out of the ESRS 1 para 91 partial-boundary relief. This **inverts** the carried-forward draft-era caution ("do NOT assert financial control"). New schema default: `ghg_boundary_approach = "Financial control (GHG Protocol 2004)"`.

4. **Four invented DRs deleted.** The standalone "anticipated financial effects" DRs the draft carried as **E2-6, E3-5, E4-6, E5-6 do not exist** in the adopted texts. Adopted DR ranges: E2 → E2-1..E2-5; E3 → E3-1..E3-4; E4 → E4-1..E4-5; E5 → E5-1..E5-5. AFE is consolidated in ESRS 2 SBM-3 (paras 25–27) plus E1-11 (climate only). Removed from Part 3, Part 4.2, Part 5.2 routing modifiers, Part 7, and the matrix.

5. **DR name / content corrections (adopted):**
   - E1-7 "Energy consumption and mix" — no "energy intensity" headline datapoint (removed).
   - E3 renamed "Water"; sub-topic = water use (withdrawal/consumption/discharge/stored); E3-4 = "Water metrics" (was "Water consumption"). "Marine resources" is not an adopted E3 sub-topic — removed (marine handled in E5).
   - E4-1 = "Biodiversity and ecosystems transition plan" (conditional — only if a plan exists and is public); E4-5 = "Metrics related to biodiversity and ecosystems change".
   - E5-5 = "Resource outflows" covering both products (durability/repairability/recyclability) and waste.
   - S1 now carries the **six** adopted sub-topics (was three). S1-8 = "Gender diversity in top management" (was "Diversity metrics"); S1-14 = % employees **entitled** to family-related leave (was "uptake rate by gender").
   - S2 carries the six adopted sub-topics (same as S1).
   - **All six G1 DR names corrected:** G1-1 Policies / G1-2 Actions / G1-3 Targets / G1-4 Metrics (corruption or bribery) / G1-5 Metrics (political influence, incl. lobbying) / G1-6 Metrics (payment practices). Removed the draft-era "EU Transparency Register number" datapoint (not in adopted G1-5) and "average time to pay invoices" as a headline (adopted G1-6 = standard terms in days + % aligned + outstanding proceedings).

6. **Sub-topics collapsed to routing labels.** Per the specialist's decision and the adopted architecture (every standard's obj. para 1 routes sub-topics through ESRS 1 para 30), DRs are set at the **standard level**, not partitioned per sub-topic. The matrix DR column now shows the full adopted standard DR set; in-scope DRs are then decided by the materiality assessment and the para-24 filter. The former per-sub-topic DR partition (draft-era) is replaced.

7. **Sub-topic descriptors replaced.** The matrix's parenthetical keyword-hints are replaced with adopted **objective-paragraph** sub-topic descriptors. GDR-M added to every ESRS 2 reference in the matrix.

8. **Phase-in framing corrected throughout topical sections.** Draft-era "prior to FY2027" replaced with the ESRS 1 para 127 first-N-reporting-years framing for "Other undertakings" (Wave 2 / Trimble). (The Wave-one para 125/126 calendar cutoffs, which legitimately use FY2027/FY2028/FY2030, are retained in §1.3.)

9. **Worked-examples schema completeness.** Fixed a pre-existing v2.0 gap: six DR-decision blocks were missing the required `retention_basis` and `commercial_prejudice_omission` fields (format rules 14–15). Because the Gem pattern-matches against these examples, the gap would have propagated as missing fields in output. All twelve DR-decision blocks now carry both fields; all seven JSON blocks parse cleanly.

## Provenance / process note (recorded at specialist's request)
During the rebuild, an **unattributed file** (`adopted_DR_inventory.md`) was found already present in the working directory before any file was written — a correction catalog matching this task, author unknown, timestamped immediately before the pass began. Its provenance could not be established. It was treated as untrusted and deleted, and the rebuild was performed **solely from the ten adopted EFRAG Knowledge Hub PDFs** uploaded by the specialist, with each correction verified against the source paragraphs. Where independent reading of the adopted text happened to agree with that file, the source of record remains the adopted standard, not the file.

## ⚠ Still open / residual
- **Entry into force.** Confirm OJ publication / post-scrutiny entry-into-force before any live filing.
- **Trimble 14-IRO re-run.** The original run is draft-basis (KB v1.0) and must be re-run on **v2.1** — labelled draft-basis in the system of record, not silently overwritten — before assurance reliance. Good opportunity to fold in this week's stakeholder-interview perspectives.

## Addendum — 28 July 2026: ESRS 1 Appendix A / para 127 reconciliation
Source: adopted ESRS 1 General Requirements text (EFRAG Knowledge Hub PDF), supplied directly by the specialist, including the full text of paras 121–127 and Appendix A. Files updated: `ESRS_Knowledge_Base_Simplified_v2.md` (§1.3, Part 3 header, E2/E4/S1/S2/S4 sub-topic lines, E2-5 DR row, Part 7 table, footer), `post_omnibus_mapping_matrix_v3.md` (header note).

- **E2-5 SoC first-year quantitative window — ✅ RESOLVED.** ESRS 1 para 127(d): for "Other undertakings" (Wave 2 / Trimble), quantitative substances-of-concern information under E2-5 may be omitted for the **first three financial years of reporting**. This closes the item flagged above.
- **E1-11 internal paragraph numbers — ✅ RESOLVED.** Confirmed by direct cross-check: para 127(b)/(c) (and 125/126) cite **E1-11 paragraphs 39(a)(b) and 40(a)(b)** exactly as already used in the E1 topical rebuild. No discrepancy.
- **Sub-topic label reconciliation — ✅ RESOLVED, all ten standards.** Cross-checked verbatim against ESRS 1 Appendix A: E1, E2, E3, E5, S1, S2, S3, S4, G1 confirmed (E2's label order aligned to Appendix A; S1/S2 labels expanded to Appendix A's fuller wording — "and social protection" added to Working conditions, "information and consultation rights of workers, including through works councils" added to Social dialogue). **E4 also closed:** Appendix A's non-binding summary reads "the extent and condition of terrestrial **and marine** ecosystems" — no "freshwater" — but the specialist confirmed (28 July 2026) that the KB's "freshwater" was sourced directly from E4's own adopted para 6 text in the topical rebuild pass, not inferred. Per the standing rule that Appendix A is explicitly non-binding guidance (para 10, Appendix A intro) and cannot override a topical standard's own operative paragraph, the KB label stands unchanged.
- **Wave-one para 125/126 calendar cutoffs — confirmed.** FY2027 (E4/S2/S3/S4), FY2028 (AFE), FY2030 (quantitative AFE, SoC) all confirmed verbatim for both the above- and below-threshold Wave-one sub-populations, including the E1-11 39(a)(b)/40(a)(b) exception on each. (Low priority — Trimble is Wave 2 — but no longer just "lightly checked.")
- **E4 "freshwater" — ✅ CLOSED.** Specialist confirmed (28 July 2026) that "freshwater" in the E4 ecosystems sub-topic label was sourced directly from E4's own adopted para 6 text during the topical rebuild, not inferred. Per the standing rule that ESRS 1 Appendix A is non-binding guidance and cannot override a topical standard's own operative paragraph, the label stands unchanged; Appendix A's shorter summary is treated as an imprecise restatement.
- **S1/S2 Appendix A footnote (*) — ✅ SOURCED AND CLOSED.** Text supplied by the specialist: the footnote clarifies that although S1 and S2 sub-topics are aligned, the *depth and granularity* of the materiality assessment for value-chain workers (S2) may legitimately differ from own workforce (S1), depending on the type/quality of data available — especially for upstream/downstream impacts and risks. This is a **methodology note, not a labelling correction**, and has been added to the S2 section (`### S2 — Workers in the Value Chain`) as a new "Materiality-assessment depth note," with an engine implication: S2 materiality write-ups are not expected to match S1's in rigor, and a shallower value-chain assessment (consistent with ESRS 1 para 33) does not itself indicate an incomplete DMA.

**Result: all ESRS 1 open items from the 28 July 2026 audit are now closed except entry-into-force / OJ publication status.**
