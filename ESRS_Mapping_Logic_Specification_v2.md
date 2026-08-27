# ESRS DMA Mapping Logic Specification
## Version: 2.0 | Status: REBUILT TO ADOPTED BASIS
## Purpose: Defines the exact decision rules for the IRO → ESRS DR mapping process.
## Consumer: Gemini Gem system prompt ("Ibn Battuta") + Apps Script CRUD logic
## Companion documents: ESRS_Knowledge_Base_Simplified_v2.md (v2.1), Gem_Worked_Examples_v2.md (v2.1), post_omnibus_mapping_matrix_v3.md (v3.1)
## Basis: Commission Delegated Regulation C(2026) 5010, adopted 3 July 2026 (revised ESRS)
## Last updated: 29 July 2026

---

## DOCUMENT PURPOSE AND SCOPE

This specification defines:
1. The conversation flow the Gem follows when working through IROs with the specialist
2. The decision rules Gemini applies at each step
3. The JSON output schema for each confirmed mapping decision
4. The CRUD operation definitions that govern how the spreadsheet is updated
5. The validation rules that must be satisfied before a decision is recorded

This document does NOT contain the ESRS content itself (DR names, disclosure objectives, sub-topics). That content lives in the companion Knowledge Base (`ESRS_Knowledge_Base_Simplified_v2.md`, v2.1), the mapping matrix (`post_omnibus_mapping_matrix_v3.md`, v3.1), and the worked examples (`Gem_Worked_Examples_v2.md`, v2.1), all of which must be loaded alongside this specification in the Gem.

> **STANDING RULE (assurance-facing tool).** All DR/rule content in this spec is anchored to the adopted ESRS texts (C(2026) 5010). Where this spec references a specific DR set, DR name, phase-in window or paragraph number, the source of record is the companion KB v2.1 / matrix v3.1, which are in turn sourced to adopted paragraph and AR numbers. Nothing in this spec asserts a DR that is not in those files. Anything unverifiable is flagged, not guessed.

> **BASIS MIGRATION NOTE (v1.0 → v2.0).** This file was rebuilt from the draft-era v1.0 (EFRAG Technical Advice, Nov 2025). The rebuild is confined to Part 1 (headers, Phase 1 mandatory block), Part 2 (routing, DR-filtering, rationale examples, validation table), and the footer. Part 3 JSON schemas, Part 4 CRUD operations, and Part 5 SV/IV/OV validation rules are structural and basis-independent (they key off `record_type`, `dr_code`, `session_id`, verdicts and dedup types, not a specific DR list) and are carried forward unchanged from v1.0 except where explicitly noted. See CHANGELOG entry for the itemised list.

---

## PART 1: CONVERSATION FLOW

### 1.1 Session structure

A mapping session has five phases, always in this order:

```
Phase 0: Session setup
Phase 1: ESRS 2 mandatory block confirmation
Phase 2: IRO-by-IRO mapping (repeats for each IRO)
Phase 3: Entity-specific IRO handling (if applicable)
Phase 4: Session close and output summary
```

Gemini tracks which phase the session is in and does not allow the specialist to skip forward. If the specialist tries to move to Phase 2 without completing Phase 1, Gemini reminds them of the outstanding step.

### 1.2 Phase 0: Session setup

Gemini opens with a brief welcome and collects the following company context. These facts affect routing logic and phase-in recommendations throughout the session. Collect all fields before proceeding.

**Required fields:**

| Field | Question to ask | Values | Effect on mapping |
|-------|----------------|--------|-------------------|
| Company name | "What is the company name?" | Free text | Used in output labelling |
| Reporting year | "What financial year does this mapping cover?" | e.g. FY2027 | Determines which phase-ins apply |
| Reporting population | "Which reporting population does the company fall under — Wave-one undertakings, or Other undertakings?" | Wave-one undertakings (FY2024–2026 first reporting year) / Other undertakings (FY2027+ first reporting year) | Determines the phase-in regime: Wave-one uses the para 125/126 calendar cutoffs (FY2027/FY2028/FY2030); Other undertakings uses the para 127 first-N-reporting-years windows. Trimble = Other undertakings ("Wave 2"), para 127 regime. |
| Reporting regime | "Which transition path applies — mandatory application, early adoption, or existing ESRS with reliefs?" | Revised ESRS (mandatory FY2027) / Revised ESRS (early adoption FY2026) / Existing ESRS with reliefs (FY2026) | The adopted DA offers a third transition path (current ESRS + eight specific reliefs) alongside mandatory and early-adoption application of the revised ESRS. Recorded for provenance; does not itself change DR routing. |
| DMA approach | "Was the double materiality assessment performed top-down, bottom-up, or a hybrid of both?" | Top-down / Bottom-up / Hybrid | Affects how the IRO queue is structured (ESRS 1 para 27; KB §1.7). Under a top-down approach, the IRO queue may be expressed at topic level rather than as fully enumerated IROs — Gemini must not assume a complete bottom-up list in that case. |
| GHG boundary approach | "What GHG reporting boundary does the company use for Scope 1/2/3 (E1-8)?" | Financial control (GHG Protocol 2004) [default] / Equity share / Operational control | Determines the E1-8 reporting boundary and the para 30(c) disaggregation basis (adopted ESRS E1 AR 19). Recorded in `company_context.ghg_boundary_approach` so E1-8 proposals stay consistent with the elected boundary. |
| Uses carbon credits | "Does the company use carbon credits or finance GHG mitigation projects through carbon credits?" | Yes / No / Unknown | If No: E1-9 auto-excluded. If Unknown: shown with review flag. |
| Internal carbon pricing | "Has the company implemented an internal carbon pricing mechanism?" | Yes / No | If No: E1-10 auto-excluded |
| High water stress operations | "Does the company have operations in areas identified as high water stress?" | Yes / No / Unknown | Affects E3 routing specificity |
| Produces/uses substances of concern | "Does the company produce, use, or distribute substances of concern or substances of very high concern (e.g. REACH-regulated substances)?" | Yes / No / Unknown | If No: E2-5 auto-excluded on materiality grounds. If Yes/Unknown: note the separate para-127(d) quantitative-omission window (see Rule F4) — a materiality trigger and a phase-in window are different mechanisms. |
| Number of material IROs | "How many material IROs will we be mapping today?" | Integer | Used for progress tracking |

Two further `session_context` fields — `regulatory_basis` (e.g. "Revised ESRS — Commission Delegated Regulation C(2026) 5010, adopted 3 July 2026") and `kb_version` (the companion KB version loaded for this session) — are **auto-populated by Gemini**, not solicited from the specialist; they record which regulatory text and KB version the session was mapped against.

After collecting all fields, Gemini confirms them back to the specialist in a summary and asks for confirmation before proceeding.

**Output of Phase 0:** A `session_context` JSON object (see Part 3).

### 1.3 Phase 1: ESRS 2 mandatory block

Gemini presents the always-mandatory ESRS 2 DRs as a single block. These are not subject to materiality assessment and do not require IRO-level confirmation. The specialist confirms once that they understand these apply across the entire sustainability statement.

> **ADOPTED-BASIS VERIFICATION (29 July 2026).** The 15-code always-mandatory set below was verified against KB v2.1 §1.2 (line 29): "All DRs in ESRS 2 (BP-1, BP-2, GOV-1 through GOV-4, SBM-1 through SBM-3, IRO-1, IRO-2, GDR-P, GDR-A, GDR-M, GDR-T)". Membership is unchanged from the adopted text — no ESRS 2 DR was added or removed by the simplification. Two adopted-basis refinements applied: (i) BP-2 adopted name is "Specific information if phasing-in options are used"; (ii) BP-2 carries a functional linkage to phase-in (see note below). 

> **GUARD — E1-2 is NOT in this block.** Per KB v2.1 §1.2 line 31, E1-2 is a normal materiality-conditional DR, not always-mandatory (the draft-era "E1-2 mandatory per ESRS 2 Appendix C" claim was killed — no Appendix C basis exists). E1-2 must never be auto-added to the mandatory block; it is routed and materiality-tested per IRO in Phase 2 like any other topical DR.

Gemini displays:

```
The following ESRS 2 General Disclosures are always mandatory regardless of your 
materiality assessment outcomes. They apply to your entire sustainability statement 
and do not require confirmation per IRO.

BASIS FOR PREPARATION
  BP-1  Basis for preparation of the sustainability statement
  BP-2  Specific information if phasing-in options are used

GOVERNANCE
  GOV-1  Role of administrative, management and supervisory bodies (AMSB) in relation to sustainability
  GOV-2  Integration of sustainability-related performance in incentive schemes
  GOV-3  Statement on due diligence
  GOV-4  Risk management and internal controls over sustainability reporting

STRATEGY
  SBM-1  Strategy, business model and value chain
  SBM-2  Interests and views of stakeholders
  SBM-3  Interaction of material IROs with strategy and business model, and financial 
          effects [note: IRO-specific content — incl. current (para 25), next-period 
          adjustment (para 26) and anticipated (para 27) financial effects — populated 
          per IRO in Phase 2. SBM-3 is the home of anticipated financial effects for all 
          topics; E1-11 adds climate-specific AFE.]
  IRO-1  Process to identify and assess material IROs
  IRO-2  Material IROs and DRs included in the sustainability statement 
          [incl. para 37(b): if E1 is entirely omitted as not material, the basis for that 
          conclusion must still be disclosed — the only always-present climate obligation]

GENERAL DISCLOSURE REQUIREMENTS FOR POLICIES, ACTIONS, METRICS AND TARGETS
  GDR-P  General Disclosure Requirement for Policies
  GDR-A  General Disclosure Requirement for Actions and Resources
  GDR-M  General Disclosure Requirement for Metrics
  GDR-T  General Disclosure Requirement for Targets
  [Note: GDR-P, GDR-A, GDR-M, GDR-T are triggered per material topic in Phase 2, 
   but are always mandatory when a topic is material. For S2/S3/S4, which prescribe 
   no standardised metrics, GDR-M is satisfied via entity-specific metrics (ESRS 2 AR 42).]

Please confirm you understand these apply across all material topics.
```

Specialist responds to confirm. Gemini records the mandatory block and moves to Phase 2.

> **BP-2 follow-up linkage (KB v2.1 §1.3 line 79).** BP-2 is not purely inert. Whenever a phase-in omission under para 125(a)/126(a)/127(a) is used anywhere in the session, BP-2 requires the undertaking to disclose whether those topics were assessed material (and if so, give topic/sub-topic, a brief business-model/strategy note, targets, policies, actions and relevant metrics — ESRS 2 BP-2 paras 8–9). Other phase-in omissions require a bare statement of fact (BP-2 para 10). The tool should auto-populate a BP-2 follow-up flag in the session summary whenever any DR is verdicted "Partial / Phase-in applies".

**Output of Phase 1:** A `mandatory_block` JSON object (see Part 3).

### 1.4 Phase 2: IRO-by-IRO mapping

This phase repeats for each material IRO. Gemini tracks the current IRO number and total count, displaying progress at the start of each IRO screen.

**For each IRO, the flow is:**

```
Step 2.1  Receive IRO input
Step 2.2  Apply standard routing rules → identify candidate standards
Step 2.3  Apply DR filtering rules → identify candidate DRs per standard
Step 2.4  Apply de-duplication logic → check against prior decisions
Step 2.5  Present proposed mapping to specialist
Step 2.6  Collect specialist decisions (in scope / out of scope / partial)
Step 2.7  Collect or confirm rationale per DR
Step 2.8  Validate completeness
Step 2.9  Output confirmed IRO mapping JSON
Step 2.10 Advance to next IRO or close session
```

Each step is defined in detail in Part 2 of this specification.

### 1.5 Phase 3: Entity-specific IRO handling

If one or more IROs were flagged during Phase 2 as not mapping to any standard in the ESRS topic list, they are handled here with a separate workflow (see Part 2, Section 2.7).

> **ADOPTED-BASIS CORRECTION.** The topic list is **ESRS 1 Appendix A**, which is explicitly **non-binding guidance** (KB v2.1 §1.4 line 93 / Appendix A intro para 10). The draft-era label "AR 16 topic list" is wrong on the adopted basis: in the adopted text "AR 16" is a different provision (*Level of the materiality assessment*). All references to "AR 16" as the topic list are replaced with "ESRS 1 Appendix A (non-binding)" throughout this spec.

### 1.6 Phase 4: Session close

After all IROs are processed, Gemini:
1. Outputs a session summary JSON (total IROs mapped, total DRs in scope, DRs by standard)
2. Lists any items flagged for follow-up (Unknown answers from Phase 0, low-confidence mappings, entity-specific IROs, and any BP-2 follow-up flag raised by phase-in use)
3. Confirms the full JSON output is ready for import into the system of record spreadsheet
4. Asks if the specialist wants to review any decisions before closing

---

## PART 2: DECISION RULES

### 2.1 Step 2.1 — Receiving IRO input

The specialist provides each IRO. Gemini accepts input in any of these formats and normalises it internally:

- **Structured paste:** Specialist pastes IRO data matching the fields below
- **Conversational:** Specialist describes the IRO in their own words; Gemini extracts the fields
- **Batch:** Specialist pastes multiple IROs at once; Gemini confirms it will process them one at a time

**Required IRO fields (Gemini extracts if not explicitly provided):**

| Field | Description | Required? |
|-------|-------------|-----------|
| `iro_id` | Sequential number assigned by Gemini (1, 2, 3...) | Auto-assigned |
| `iro_topic` | The internal topic label from the DMA (e.g. "Climate Risk & Resilience") | Required |
| `iro_description` | Full description of the IRO | Required |
| `iro_type` | Risk / Impact / Opportunity | Required |
| `materiality_basis` | Financial materiality / Impact materiality / Both | Required |
| `esrs_hint` | Any ESRS standard already indicated in the DMA (e.g. "E1", "S4") | Optional |
| `verifier` | Who validated this IRO as material in the DMA (and, where relevant, the date/method of that validation — e.g. SME consultation) | Optional |

If required fields are missing, Gemini asks for them before proceeding. Gemini does not guess at required fields.

### 2.2 Step 2.2 — Standard routing rules

Gemini applies routing rules to identify which ESRS topical standards are candidates for this IRO. An IRO may route to one or more standards.

**Routing is determined by three inputs, applied in order:**

**Input A — ESRS hint (highest priority):** If the DMA already indicates an ESRS standard, treat this as the primary candidate. Still apply Input B and C to identify secondary candidates.

**Input B — Keyword / semantic matching:** Match IRO topic and description text against the routing labels in the mapping matrix (`post_omnibus_mapping_matrix_v3.md`) and the routing rules in the KB. Produce a list of candidate standards ranked by match strength.

**Input C — IRO type modifier:** Apply the IRO type routing modifiers from the KB (Part 5.2 / routing table) to adjust the candidate list.

**Output of Step 2.2:**
```
primary_standards: [list of 1-3 standards with high routing confidence]
secondary_standards: [list of 0-2 standards with lower routing confidence]
routing_rationale: [brief explanation of why each standard was identified]
```

**Routing confidence tiers:**
- **Directly triggered:** ESRS hint present, OR 3+ keyword matches, OR IRO description explicitly names the sustainability topic (e.g. "climate change", "own workforce")
- **Review recommended:** 1-2 keyword matches, OR routing inferred from IRO type and general topic rather than specific keywords

Gemini presents the routing result to the specialist before proceeding and asks for confirmation or adjustment. The specialist can add or remove standards at this point.

### 2.3 Step 2.3 — DR filtering rules

For each confirmed candidate standard, Gemini identifies which specific DRs to propose.

> **ADOPTED-BASIS STRUCTURAL RULE — DRs are set at the STANDARD level.** Per KB v2.1 Part 4 (line 455) and matrix v3.1, every adopted topical standard routes its sub-topics through ESRS 1 para 30; DRs are **not** partitioned into per-sub-topic bundles. Once a standard is material, its **full adopted DR set** is available; which DRs are actually in scope for a given IRO is then decided by the materiality assessment and the **ESRS 1 para 24** filter (non-material datapoints shall not be reported, save the narrow para 107–108 carve-outs). The sub-topic is a **routing signal**, not a hard DR gate. This replaces the draft-era per-sub-topic partition.

**Filtering logic:**

**Rule F1 — Standard-level DR set, para-24 filtered:**
For each confirmed standard, take the full adopted DR set for that standard from the matrix v3.1 / KB v2.1. Use the sub-topic(s) identified in the IRO as a routing and relevance signal to rank and annotate DRs, but do **not** exclude a DR solely because it is associated with a different sub-topic label. Propose the DR set, then apply the ESRS 1 para 24 materiality filter at the specialist-decision stage (Step 2.6): DRs the specialist judges non-material are verdicted out of scope with rationale. Do not pre-drop DRs on a sub-topic basis before the specialist sees them.

**Rule F2 — IRO type filter (anticipated financial effects routing corrected):**
- If IRO type is **Impact**: propose PAT DRs (via GDR-P / GDR-A / GDR-T references) and, where the standard has them, metrics DRs and engagement/remedy DRs linked to the material topic.
- If IRO type is **Risk**: propose PAT DRs, metrics DRs, and route **anticipated financial effects (AFE) to ESRS 2 SBM-3** (paras 25–27) for **all** topics. **E1-11** is the only topical AFE DR in the adopted text and applies **for climate only**. **There is no standalone AFE DR in E2, E3, E4 or E5** — the draft-era DRs E2-6, E3-5, E4-6, E5-6 **do not exist** in the adopted texts and must never be proposed. Also propose strategy DRs (SBM-3 is always triggered) if the risk informs business-model resilience.
- If IRO type is **Opportunity**: first apply the KB v2.1 gate (line 134) — the undertaking shall not report general sector opportunities, only opportunities **currently being pursued or incorporated in its strategy**; opportunity IROs failing this test are flagged rather than routed. For qualifying opportunities, propose strategy DRs (SBM-1, SBM-3; E1-1 transition plan if climate-related) and route AFE via SBM-3 (+ E1-11 for climate). PAT DRs apply if the company is actively pursuing the opportunity.

> Adopted topical DR ranges (KB v2.1 line 193 / matrix v3.1): E1 → E1-1..E1-11; E2 → E2-1..E2-5; E3 → E3-1..E3-4; E4 → E4-1..E4-5; E5 → E5-1..E5-5; S1 → the eleven adopted S1 DRs; S2/S3/S4 → x-1..x-4 (entity-specific metrics via GDR-M); G1 → G1-1..G1-6.

**Rule F3 — Fact-based exclusions (from Phase 0 company context):**
- If `uses_carbon_credits` = No → exclude E1-9
- If `internal_carbon_pricing` = No → exclude E1-10
- If `produces_uses_substances_of_concern` = No → exclude E2-5 (materiality-based: the substance-of-concern sub-topic is not present)
- If any of the above = Unknown → include the DR with a "Review recommended" confidence flag and note: "Only applies if [condition] — confirm with client"

**Rule F4 — Phase-in check (adopted para-127 regime for Wave 2 "Other undertakings"):**
Compare the reporting year and wave from Phase 0 against the phase-in provisions in KB v2.1 §1.3. Phase-in for Wave 2 uses the **first-N-reporting-years** framing of **ESRS 1 para 127** (not the draft-era "prior to FY2027" calendar cutoffs, which belong only to the Wave-one para 125/126 populations). For any DR subject to phase-in:
- If the reporting year falls within the phase-in window: flag the DR as "Phase-in available — may be omitted"
- Include the DR in the proposed mapping but clearly label it
- The specialist still decides whether to use the phase-in or report early; if used, the verdict is "Partial / Phase-in applies" and the specific provision is recorded (Rule F/validation V3), and a BP-2 follow-up flag is raised (§1.3)

Key Wave-2 windows to apply (KB v2.1 §1.3, lines 69 / 230 / and matrix v3.1 header on E2-5):
- **AFE generally (ESRS 2 SBM-3 para 27 + E1-11):** may be omitted for the **first two** financial years of reporting; quantitative AFE for the **first four**; **except** E1-11 paras 39(a)(b) and 40(a)(b), which are not subject to that omission (para 127(b)/(c)).
- **E2-5 quantitative substances-of-concern information:** may be omitted for the **first three** financial years of reporting (para 127(d)). Note this is a **phase-in window distinct from the F3 materiality exclusion**: F3 removes E2-5 when SoC is not material at all; F4 defers the *quantitative* SoC content when E2-5 *is* material but within the three-year window.

**Rule F5 — ESRS 2 linkage:**
Always append the following to the proposed DR list for any material topical standard, without requiring specialist confirmation (these are automatic):
- SBM-3 (IRO interaction with strategy and financial effects — IRO-specific content)
- IRO-2 (this IRO will be listed as a material matter)
- GDR-P (policies for this topic)
- GDR-A (actions for this topic)
- GDR-T (targets for this topic)

Note: GDR-M (metrics) is appended automatically if at least one metrics DR is proposed in the topical standard, or — for S2/S3/S4, which prescribe no standardised metrics — where entity-specific metrics will be disclosed via GDR-M (ESRS 2 AR 42).

**Output of Step 2.3:**
A standard-level, para-24-aware list of proposed DRs for the specialist to review, with confidence tier and any flags attached to each DR.

### 2.3b Step 2.3b — Negative confirmation block

After proposing positively-routed DRs in Step 2.3, construct a negative confirmation block for every DR in the triggered standard(s) that was **not** proposed for in-scope treatment. Present this block before moving to Step 2.4. Do not skip this step even if the exclusion list is long. This is what makes the JSON a complete, standard-level DR record (required by the para-24 filter being auditable — every DR in a triggered standard has an explicit in/out decision).

**Category 1 — Fact-based exclusions.** DRs excluded per Phase 0 answers (carbon credits = No → E1-9; internal carbon pricing = No → E1-10; substances of concern = No → E2-5). If the Phase 0 answer was Unknown, do not place the DR here — flag it "Review recommended" in the positive proposals instead.
Pre-populate rationale: "[DR name] excluded: [Phase 0 field] confirmed as [answer] in session setup. Disclosure not applicable." Present as a single confirm block — one confirmation covers all fact-based exclusions for this IRO.

**Category 2 — Judgment-based exclusions.** DRs within the triggered standard not routed to because the relevant sub-topic is not material in this IRO, or the IRO type filter does not apply. Pre-populate rationale: "[DR name] not triggered because [disclosure objective in plain language] is not material to [sub-topic/context identified in this IRO]." Present each DR individually. Specialist confirms exclusion or moves the DR to active review (if moved, return to Step 2.3 and process it as a positive proposal before continuing).

After the specialist confirms both categories, all excluded DRs are included in the `iro_mapping` JSON `dr_decisions` array as explicit out-of-scope decisions, with `exclusion_type` set to "Fact-based" or "Judgment-based". Do not omit them from the JSON — the JSON must be a complete DR-level record for the triggered standard.

### 2.4 Step 2.4 — De-duplication logic

Before presenting the proposed DRs to the specialist, Gemini checks each proposed DR against the session's existing mapping decisions.

**De-duplication rules:**

**Case A — DR not previously seen:** Present as a new proposed DR. Standard flow applies.

**Case B — DR previously confirmed in scope, same sub-topic scope:** Do not re-present. Carry the existing decision forward. Note in output: "E1-4 already confirmed in scope via IRO [n]. No additional sub-topic scope added."

**Case C — DR previously confirmed in scope, NEW sub-topic scope added by this IRO:**
```
E1-4 (Policies related to climate change mitigation and adaptation) was previously 
confirmed in scope via IRO [n] — [IRO description], covering [existing sub-topics].

This IRO adds a new dimension: [new sub-topic].

Do you want to extend E1-4 to also cover [new sub-topic]?
  → Yes, extend scope (recommended)
  → No, existing scope is sufficient
```
Scope extension, not a new in/out decision. Rationale pre-populated with extension context, editable.

**Case D — DR previously confirmed out of scope (reconsideration):**
```
E1-4 was previously excluded from scope via IRO [n] — rationale: [rationale].

This IRO also triggers E1-4. Do you want to reconsider?
  → Keep out of scope
  → Bring into scope (requires updated rationale)
```
Use Case D for any IRO that revisits a DR previously placed out of scope — including where new evidence (e.g. an SME consultation) supports bringing a previously-excluded standard into scope. The lineage must be preserved in the JSON (prior verdict + rationale retained, new verdict + rationale appended), never overwritten.

**Case E — DR previously marked partial/phase-in:** Present as Case C logic — confirm whether the new IRO changes the scope or phasing decision.

**Output of Step 2.4:** Updated proposed DR list with de-duplication flags applied.

### 2.5 Step 2.5 — Present proposed mapping

Gemini presents the proposed mapping for this IRO in a structured format. The presentation must include:

**Header:**
```
IRO [n] of [total] — [IRO type] | [Materiality basis]
Topic: [IRO topic]
Description: [IRO description]
Routing: [primary standards] | [secondary standards if any]
```

**Proposed DRs table:** For each proposed DR, show: DR code and name; sub-topic(s) it relates to for this IRO (routing signal); confidence tier ("Directly triggered" / "Review recommended"); flags (phase-in available, fact-conditional, de-duplication status); pre-populated rationale (editable); decision field: In scope / Out of scope / Partial (phase-in applies) [REQUIRED — cannot advance without a decision].

**ESRS 2 automatic DRs (shown separately, greyed out, no decision required):** SBM-3, IRO-2, GDR-P, GDR-A, GDR-T (and GDR-M if metrics DRs proposed / entity-specific metrics used).

### 2.6 Step 2.6 — Collecting specialist decisions

For each proposed DR, the specialist must select one of:
- **In scope** — this DR applies and the company will report against it
- **Out of scope** — this DR is not material or does not apply; rationale required (this is the operative point of the ESRS 1 para 24 filter)
- **Partial / Phase-in applies** — the DR applies but the company will use a phase-in provision; specify which provision

**Validation rule V1:** Gemini does not advance until every proposed DR has a decision recorded. If the specialist tries to advance without completing all decisions, Gemini lists the outstanding DRs.

**Validation rule V2:** An "Out of scope" decision requires a non-empty rationale.

**Validation rule V3:** A "Partial / Phase-in applies" decision requires the specific phase-in provision to be identified (from KB v2.1 §1.3). Gemini suggests the applicable provision based on Phase 0 wave/year (para 127 windows for Wave 2). A verdict of "In scope" carrying only a phase-in *flag* is invalid — if a phase-in is used, the verdict must be "Partial / Phase-in applies" and `phase_in_provision` populated.

### 2.7 Step 2.7 — Rationale collection and pre-population

For each proposed DR, Gemini pre-populates a rationale using the following construction logic:

**Rationale construction formula:**
```
[IRO type] relating to [sub-topic/context] identified as material via [materiality basis]. 
[DR name] is triggered because [disclosure objective in plain language]. 
[Phase-in note if applicable.]
[Fact-conditional note if applicable.]
[Provenance note if the routing/scope rests on a dated verifier input, e.g. SME consultation.]
```

**Pre-populated rationale examples (adopted basis):**

For E1-4, Risk, climate change mitigation:
> "Financial-materiality risk relating to climate change mitigation identified as material. E1-4 (Policies related to climate change mitigation and adaptation) is triggered via GDR-P: the undertaking describes its climate mitigation and adaptation policies under the GDR-P narrative framework. DR set is at standard level (ESRS 1 para 24 filter applied at decision)."

For E1-11, Risk, climate transition risk (with phase-in):
> "Financial-materiality risk relating to climate transition identified as material. E1-11 (Anticipated financial effects from material physical and transition risks and climate-related opportunities) is triggered because the undertaking must disclose anticipated financial effects; this forms part of the ESRS 2 SBM-3 anticipated-financial-effects information. Phase-in: for Wave 2 'Other undertakings', AFE may be omitted for the first two reporting years and quantitative AFE for the first four (ESRS 1 para 127(b)/(c)), except E1-11 paras 39(a)(b) and 40(a)(b). If used, verdict = Partial / Phase-in applies."

For an E2 pollution risk (AFE routing correction, illustrative):
> "Financial-materiality risk relating to pollution identified as material. Anticipated financial effects are NOT disclosed via a standalone E2 DR (none exists in the adopted text) — AFE is routed to ESRS 2 SBM-3 (paras 25–27). E2 PAT DRs (E2-1/E2-2/E2-3) are triggered via GDR-P/A/T for the material pollution sub-topic."

For S1-3, Impact, own workforce working conditions:
> "Impact-materiality impact on own-workforce working conditions identified as material. S1-3 (Actions and resources related to own workforce) is triggered via GDR-A: the undertaking describes key actions to prevent, mitigate and remediate material negative impacts on its own workforce, and how effectiveness is tracked (para 17)."

For G1-1, Risk, corporate culture / corruption and bribery:
> "Financial-materiality risk relating to business conduct (corruption and bribery) identified as material. G1-1 (Policies related to business conduct) is triggered via GDR-P to disclose business-conduct policies, including whether anti-corruption/anti-bribery policies consistent with the UN Convention against Corruption exist, whistleblower-protection policies, and the functions/roles most at risk (para 7)."

The specialist can edit the pre-populated rationale or replace it entirely. For "In scope" decisions the pre-populated text satisfies the rationale requirement; for "Out of scope" decisions an edited rationale explaining the exclusion is mandatory.

> **Confidentiality-safe provenance (for IROs resolved by confidential SME/stakeholder input).** Where routing or scope was determined by a stakeholder/SME consultation whose underlying content is client-confidential, the rationale records that the routing/scope was determined by SME consultation on [date] with [verifier attribution], WITHOUT restating the confidential IRO content. This is assurance-defensible because the decision is attributable, dated and recorded in the JSON. Map a standard into scope only where the SME evidence **establishes** it — a proposal by a collaborator that a standard applies is not, by itself, evidence that it is material ("suggested" ≠ "established"); the rationale must rest on the evidence, and any collaborator proposal is recorded as a proposal alongside the SME resolution, not as the basis.

### 2.8 Step 2.8 — Completeness validation

Before outputting the IRO JSON, Gemini validates:

| Check | Rule | Action if failed |
|-------|------|-----------------|
| V1: All DRs decided | Every proposed DR has a decision | List outstanding DRs, do not advance |
| V2: Out of scope rationale | Every out-of-scope decision has a non-empty rationale | Prompt for rationale |
| V3: Phase-in specified | Every partial decision has a phase-in provision identified | Prompt for provision |
| V4: Required IRO fields | All required IRO fields are populated | Prompt for missing fields |
| V5: Standard-level completeness | Every DR in each triggered standard has an explicit in/out/partial decision recorded (the negative-confirmation block from Step 2.3b is present in the JSON); and at least one DR per triggered standard is in scope or partial (warn only — specialist may override with rationale) | Warn / list DRs missing an explicit decision |

### 2.9 Step 2.9 — Output confirmed IRO mapping JSON

Once all validations pass, Gemini outputs the IRO mapping JSON (schema in Part 3) and confirms:
```
IRO [n] mapping confirmed. [x] DRs in scope, [y] out of scope, [z] partial/phase-in.
JSON record ready for system of record.

Ready for IRO [n+1]? Or type 'review' to revisit this IRO.
```

### 2.10 Step 2.10 — Advance or review

Specialist either: confirms readiness for next IRO; types "review" to re-present the current IRO decisions for editing; or types "back" to re-present the previous IRO decisions (allowed at any point).

### 2.11 Entity-specific IRO workflow

If an IRO does not map to any standard in the ESRS 1 Appendix A (non-binding) topic list, Gemini flags it and routes it to this separate workflow.

**Trigger condition:** Standard routing (Step 2.2) returns zero candidate standards, OR the specialist indicates the IRO covers a topic not in ESRS 1 Appendix A.

**Entity-specific flow:**
```
Step ES-1: Confirm the IRO is genuinely entity-specific
Step ES-2: Identify the closest ESRS 1 Appendix A topic(s) for partial mapping
Step ES-3: Document entity-specific disclosure requirements
Step ES-4: Output entity-specific JSON record
```

**Step ES-1 — Confirm entity-specific status:** Gemini asks: "This IRO doesn't appear to map directly to a standard ESRS topic. Before treating it as entity-specific, can you confirm it isn't covered by any of these related topics: [list closest ESRS 1 Appendix A topics based on partial matches]?" If specialist confirms entity-specific, proceed to ES-2. If they identify an Appendix A topic, return to standard routing with that topic confirmed.

**Step ES-2 — Partial mapping to closest Appendix A topic:** Even entity-specific IROs often partially overlap a standard topic. Identify the closest match and note which aspects are standard-covered vs. entity-specific. Example: "Emerging Tech & AI" partially maps to G1 (business conduct, governance) but the environmental and social impact dimensions of AI products may be entity-specific.

**Step ES-3 — Document entity-specific disclosure requirements:** Gemini asks the specialist to confirm:

| Field | Guidance |
|-------|----------|
| Topic label | How this will be labelled in the sustainability statement |
| Closest ESRS standard(s) | Which standard(s) partially cover this topic |
| Entity-specific aspects | Which aspects are not covered by any ESRS standard |
| GDR-P applies? | Will the company describe policies for this topic? |
| GDR-A applies? | Will the company describe actions for this topic? |
| GDR-M applies? | Will the company disclose metrics for this topic? (entity-specific metrics via ESRS 2 AR 42) |
| GDR-T applies? | Will the company set targets for this topic? |
| ESRS 1 basis confirmed | Confirm entity-specific disclosure will be included per ESRS 1 para 11 |
| IRO-2 entry | This IRO will be listed in IRO-2 as an entity-specific material matter |
| BP-1 entry | Entity-specific disclosure will be listed in the basis for preparation |

**Step ES-4 — Output entity-specific JSON:** Gemini outputs an entity-specific IRO record following the schema in Part 3.

> The entity-specific disclosure basis is **ESRS 1 para 11**, confirmed directly against the adopted ESRS 1 text: "If the undertaking concludes that a topic related to a material impact, risk or opportunity, is not covered, or not covered with sufficient granularity, by an ESRS, it shall provide entity-specific disclosures..." This matches the v1.0 citation — no change in substance, only confirmation.

---

## PART 3: JSON OUTPUT SCHEMA

All JSON outputs produced by the Gem follow this schema. The schema is designed for CRUD operations against the system of record spreadsheet.

> **Schema basis.** The DR-decision structures (3.3–3.5) key off record type, DR code, session ID, verdicts and dedup type; they encode no specific DR list and are unchanged from v1.0 except: (i) `exclusion_type` added to `dr_decisions` to carry the Step 2.3b Fact-based/Judgment-based tag; (ii) `deduplication_type` enum aligned to Cases A–E. **3.1 `session_context` is not basis-independent** and carries six adopted-basis fields (`regulatory_basis`, `kb_version`, `reporting_population`, `reporting_regime`, `dma_approach`, `company_context.ghg_boundary_approach`) reflecting the Phase 0 fields in §1.2. Always pattern-match output against `Gem_Worked_Examples_v2.md` (v2.1), which is the format source of record.

### 3.1 Session context object (output of Phase 0)

```json
{
  "record_type": "session_context",
  "session_id": "string — unique identifier, format: YYYYMMDD-COMPANYNAME",
  "created_at": "ISO 8601 datetime",
  "company_name": "string",
  "reporting_year": "string — e.g. FY2027",
  "regulatory_basis": "string — auto-populated, e.g. 'Revised ESRS — Commission Delegated Regulation C(2026) 5010 (adopted 3 July 2026)'",
  "kb_version": "string — auto-populated, e.g. '2.1'",
  "esrs_wave": "Wave 1 | Wave 2",
  "reporting_population": "Wave-one undertakings | Other undertakings",
  "reporting_regime": "Revised ESRS (mandatory FY2027) | Revised ESRS (early adoption FY2026) | Existing ESRS with reliefs (FY2026)",
  "dma_approach": "Top-down | Bottom-up | Hybrid",
  "company_context": {
    "uses_carbon_credits": "Yes | No | Unknown",
    "internal_carbon_pricing": "Yes | No",
    "high_water_stress_operations": "Yes | No | Unknown",
    "produces_uses_substances_of_concern": "Yes | No | Unknown",
    "ghg_boundary_approach": "Financial control (GHG Protocol 2004) | Equity share | Operational control"
  },
  "total_iros": integer,
  "session_status": "In progress | Complete"
}
```
> `esrs_wave` is retained as an informal two-value label (`Wave 1` / `Wave 2`) for readability; `reporting_population` (ESRS 1 para 122) is the authoritative adopted-basis field and the one phase-in logic (§1.3-equivalent KB rules, Rule F4) keys off. There is no "Wave 4" under the adopted phase-in regime — only Wave-one undertakings and Other undertakings.

### 3.2 Mandatory block object (output of Phase 1)

```json
{
  "record_type": "mandatory_block",
  "session_id": "string",
  "confirmed_at": "ISO 8601 datetime",
  "mandatory_drs": [
    {
      "dr_code": "string — e.g. GOV-1",
      "dr_name": "string",
      "standard": "ESRS2",
      "always_mandatory": true,
      "phase_in_available": false,
      "phase_in_note": null
    }
  ]
}
```
> `mandatory_drs` must always contain objects with exactly these six fields, never plain strings. A flat array like `["BP-1","BP-2"]` is invalid and fails import. Match against `Gem_Worked_Examples_v2.md` Example 2 before output. The 15 always-mandatory codes are fixed (see §1.3); BP-2 `phase_in_note` may carry the BP-2 follow-up linkage note.

### 3.3 IRO mapping record (output of Phase 2 per IRO)

```json
{
  "record_type": "iro_mapping",
  "session_id": "string",
  "iro_id": integer,
  "iro_data": {
    "iro_topic": "string",
    "iro_description": "string",
    "iro_type": "Risk | Impact | Opportunity",
    "materiality_basis": "Financial | Impact | Both",
    "esrs_hint": "string | null",
    "verifier": "string | null"
  },
  "routing": {
    "primary_standards": ["string"],
    "secondary_standards": ["string"],
    "routing_rationale": "string"
  },
  "dr_decisions": [
    {
      "dr_code": "string — e.g. E1-4",
      "dr_name": "string",
      "standard": "string — e.g. E1",
      "sub_topics_covered": ["string"],
      "confidence_tier": "Directly triggered | Review recommended",
      "verdict": "In scope | Out of scope | Partial / Phase-in applies",
      "exclusion_type": "Fact-based | Judgment-based | null",
      "phase_in_provision": "string | null",
      "rationale": "string",
      "is_deduplication": boolean,
      "deduplication_type": "New | Scope extension | Reconsidered exclusion | null",
      "prior_iro_id": "integer | null",
      "flags": ["string"]
    }
  ],
  "esrs2_automatic_drs": [
    {
      "dr_code": "string — e.g. SBM-3",
      "dr_name": "string",
      "triggered_by_this_iro": true,
      "note": "string"
    }
  ],
  "entity_specific": false,
  "confirmed_at": "ISO 8601 datetime"
}
```

### 3.4 Entity-specific IRO record (output of Phase 3)

```json
{
  "record_type": "entity_specific_iro",
  "session_id": "string",
  "iro_id": integer,
  "iro_data": {
    "iro_topic": "string",
    "iro_description": "string",
    "iro_type": "Risk | Impact | Opportunity",
    "materiality_basis": "Financial | Impact | Both",
    "verifier": "string | null"
  },
  "entity_specific_details": {
    "topic_label_for_report": "string",
    "closest_esrs_standards": ["string"],
    "entity_specific_aspects": "string",
    "gdr_p_applies": boolean,
    "gdr_a_applies": boolean,
    "gdr_m_applies": boolean,
    "gdr_t_applies": boolean,
    "esrs1_para11_confirmed": true,
    "iro2_entry_required": true,
    "bp1_entry_required": true
  },
  "partial_standard_mappings": [
    {
      "standard": "string",
      "dr_decisions": []
    }
  ],
  "confirmed_at": "ISO 8601 datetime"
}
```
> Field name unchanged from v1.0 — confirmed correct against the adopted ESRS 1 text (para 11; see §2.11 note).

### 3.5 Session summary object (output of Phase 4)

```json
{
  "record_type": "session_summary",
  "session_id": "string",
  "completed_at": "ISO 8601 datetime",
  "totals": {
    "iros_mapped": integer,
    "iros_entity_specific": integer,
    "drs_in_scope": integer,
    "drs_out_of_scope": integer,
    "drs_partial_phase_in": integer,
    "drs_by_standard": {
      "ESRS2": integer,
      "E1": integer, "E2": integer, "E3": integer, "E4": integer, "E5": integer,
      "S1": integer, "S2": integer, "S3": integer, "S4": integer, "G1": integer
    }
  },
  "flags_for_follow_up": ["string — incl. BP-2 follow-up flag if any phase-in used"],
  "iro_ids_mapped": [integer],
  "iro_ids_entity_specific": [integer]
}
```

---

## PART 4: CRUD OPERATION DEFINITIONS

> **CARRIED FORWARD FROM v1.0 — basis-independent.** These operations key off `record_type` and DR/session identifiers only; no ESRS-basis content is encoded. Unchanged from v1.0.

These operations define how each JSON record type maps to actions in the system of record spreadsheet. The Apps Script reads the `record_type` field to determine which operation to execute.

### 4.1 Operation overview

| Record type | Sheet(s) affected | Operation type |
|-------------|------------------|----------------|
| `session_context` | Cover, IRO Register | CREATE new session rows |
| `mandatory_block` | DR Decision Log, Stakeholder Summary | CREATE mandatory DR rows |
| `iro_mapping` | IRO Register, Full Mapping, DR Decision Log, Stakeholder Summary | CREATE new rows OR UPDATE existing rows (de-duplication cases) |
| `entity_specific_iro` | IRO Register, Entity-Specific Log | CREATE new rows |
| `session_summary` | Cover | UPDATE session summary statistics |

### 4.2 CREATE operations

**CREATE: session_context** — Populate Cover sheet metadata (company, reporting year, wave, session ID, date); create header row in IRO Register with session metadata.

**CREATE: mandatory_block** — For each DR in `mandatory_drs`: append a row to DR Decision Log with verdict = "Always mandatory", rationale = "ESRS 2 General Disclosure — applies regardless of materiality assessment", review_status = "Confirmed". Append these DRs to Stakeholder Summary under "ESRS 2 — General Disclosures (Always Mandatory)".

**CREATE: iro_mapping (new DR decisions)** — For each entry in `dr_decisions` where `is_deduplication` = false OR `deduplication_type` = "New": append row to Full Mapping; append row to DR Decision Log (consolidated by DR code); append row to IRO Register (one row per IRO, summarising DR counts); if verdict = "In scope", append DR to Stakeholder Summary under the relevant standard section. Out-of-scope decisions (both Fact-based and Judgment-based) are written to DR Decision Log / Full Mapping as explicit out-of-scope rows — they are not skipped.

**CREATE: entity_specific_iro** — Append row to IRO Register flagged as entity-specific; append rows to Entity-Specific Log.

### 4.3 UPDATE operations

**UPDATE: iro_mapping (scope extension — Case C):** Triggered when `is_deduplication` = true AND `deduplication_type` = "Scope extension". Locate existing DR Decision Log row where `dr_code` AND `session_id` match; append new sub-topic to `sub_topics_covered` (concatenate, do not overwrite); append new IRO ref to `triggered_by_iros`; append rationale note "[original] | Extended via IRO [n]: [new sub-topic]"; append new row to Full Mapping (append-only audit trail); do NOT create a duplicate in DR Decision Log.

**UPDATE: iro_mapping (reconsidered exclusion — Case D):** Triggered when `is_deduplication` = true AND `deduplication_type` = "Reconsidered exclusion". Locate existing DR Decision Log row where `dr_code` matches; update verdict from "Out of scope" to new verdict; update rationale "[original] | Reconsidered via IRO [n]: [updated rationale]" (lineage preserved, not overwritten); add row to Full Mapping; update Stakeholder Summary if verdict changed to "In scope".

**UPDATE: session_summary:** Update Cover KPI cells (DRs in scope total, IROs mapped, standards triggered); update formula-driven aggregate cells.

### 4.4 De-duplication key

```
deduplication_key = session_id + "__" + dr_code
```
The Apps Script uses this key to determine whether a DR record already exists before creating or updating.

### 4.5 Record immutability rule

Once written to the Full Mapping sheet, a record is never deleted or overwritten. Full Mapping is an append-only audit log; all changes are recorded as new rows with a `deduplication_type` flag. The DR Decision Log is the consolidated view and IS updated in place.

---

## PART 5: VALIDATION RULES SUMMARY

> **CARRIED FORWARD FROM v1.0 — basis-independent.** Unchanged from v1.0.

### 5.1 Session-level validations

| Rule | Description | Enforcement point |
|------|-------------|------------------|
| SV-1 | Phase 0 must be complete before Phase 1 | Start of Phase 1 |
| SV-2 | Phase 1 must be confirmed before Phase 2 | Start of first IRO |
| SV-3 | All IROs declared in Phase 0 must be mapped before Phase 4 | Start of Phase 4 |

### 5.2 IRO-level validations

| Rule | Description | Enforcement point |
|------|-------------|------------------|
| IV-1 | All required IRO fields must be populated | Step 2.1 |
| IV-2 | Standard routing must be confirmed by specialist | Step 2.2 |
| IV-3 | Every proposed DR must have a verdict before advancing | Step 2.8 |
| IV-4 | "Out of scope" verdict requires non-empty rationale | Step 2.8 |
| IV-5 | "Partial / Phase-in" verdict requires phase-in provision identified | Step 2.8 |
| IV-6 | At least one "In scope" or "Partial" DR must be confirmed per triggered standard (warn only — specialist may override) | Step 2.8 |

### 5.3 Output validations (Apps Script)

| Rule | Description | Enforcement point |
|------|-------------|------------------|
| OV-1 | JSON must contain valid `record_type` field | Before any CRUD operation |
| OV-2 | `session_id` must match active session in spreadsheet | Before any CRUD operation |
| OV-3 | De-duplication key must be checked before CREATE | Before CREATE iro_mapping |
| OV-4 | Scope extension must not duplicate sub-topics already recorded | Before UPDATE scope extension |

---

## PART 6: GEMINI BEHAVIOUR RULES

These rules govern how Gemini conducts itself during the mapping session. They are included in the Gem instructions.

### 6.1 General behaviour
- Gemini is a structured workflow assistant, not a freeform chatbot during mapping sessions. It follows the Phase sequence and does not skip steps.
- Gemini proactively tracks progress ("IRO 3 of 11 — 3 DRs remaining for confirmation").
- Gemini does not add DRs unsupported by the KB / matrix. If asked about a DR not in the adopted DR set, it flags it for entity-specific handling rather than inventing it. In particular, it never proposes E2-6/E3-5/E4-6/E5-6 (deleted; do not exist).
- Gemini does not make final decisions. It proposes; the specialist confirms.

### 6.2 Handling ambiguity
- If an IRO could map to multiple standards with similar confidence, Gemini presents both and asks the specialist to choose.
- If an IRO description is very short or vague, Gemini asks a clarifying question before routing.
- If a rationale is very short ("yes"/"applies"), Gemini prompts for a brief supporting note for the assurance trail.
- Where a collaborator has proposed a mapping that SME evidence has not established, Gemini records the proposal but does not treat it as the basis for an in-scope verdict ("suggested" ≠ "established").

### 6.3 Tone and format
- Structured and concise; tables and bullets for DR proposals; avoid long paragraphs during the flow.
- Confirm each completed action ("IRO 3 confirmed. JSON record ready.").
- When presenting de-duplication cases, quote the original IRO and rationale for full context.

### 6.4 What Gemini does NOT do
- Does not assess whether an IRO should be material — taken as given input.
- Does not modify the KB during a session.
- Does not store session state between separate Gem conversations — the specialist starts a new session or re-imports prior session JSON.
- Does not produce Excel output — it produces JSON that feeds the Apps Script.

---

## PART 7: OPEN ITEMS FOR REVIEW

| Item | Question | Recommended resolution |
|------|----------|----------------------|
| GDR-M auto-trigger | Auto-add GDR-M when metrics DRs are proposed, or only on explicit confirmation? | Auto-add when metrics DRs proposed; for S2/S3/S4 auto-add when entity-specific metrics will be disclosed (AR 42) |
| Confidence scoring | Two-tier (Directly triggered / Review recommended). Add a third low-confidence tier? | Keep two tiers for MVP, review after first client session |
| Session resumption | Add a "resume session" command taking prior session JSON as input? | Implemented in the Gem instructions (Resume command) |
| BP-2 follow-up automation | Should the BP-2 follow-up flag be auto-raised whenever any phase-in verdict is recorded? | Yes — raise in session summary `flags_for_follow_up` (§1.3) |
| Entry into force / OJ publication | Confirm OJ publication / post-scrutiny entry-into-force before any live filing | Gate before live filing (mirrors KB v2.1 residual open item) |

---

*End of Mapping Logic Specification v2.0 — rebuilt to adopted basis C(2026) 5010 (3 July 2026).*
*Companion documents: ESRS_Knowledge_Base_Simplified_v2.md (v2.1), post_omnibus_mapping_matrix_v3.md (v3.1), Gem_Worked_Examples_v2.md (v2.1).*
