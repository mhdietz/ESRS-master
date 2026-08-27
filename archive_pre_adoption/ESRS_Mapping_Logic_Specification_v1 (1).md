# ESRS DMA Mapping Logic Specification
## Version: 1.0 | Status: DRAFT FOR REVIEW
## Purpose: Defines the exact decision rules for the IRO → ESRS DR mapping process.
## Consumer: Gemini Gem system prompt + Apps Script CRUD logic
## Companion document: ESRS_Knowledge_Base_Simplified_v1.md
## Last updated: May 2026

---

## DOCUMENT PURPOSE AND SCOPE

This specification defines:
1. The conversation flow the Gem follows when working through IROs with the specialist
2. The decision rules Gemini applies at each step
3. The JSON output schema for each confirmed mapping decision
4. The CRUD operation definitions that govern how the spreadsheet is updated
5. The validation rules that must be satisfied before a decision is recorded

This document does NOT contain the ESRS content itself (DR names, disclosure objectives, sub-topics). That content lives in the companion Knowledge Base document, which must be loaded alongside this specification in the Gem.

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
| ESRS wave | "Which CSRD reporting wave does the company fall under?" | Wave 1 (FY2024+) / Wave 2 (FY2027+) / Wave 4 (FY2028+) | Affects phase-in eligibility |
| Uses carbon credits | "Does the company use carbon credits or finance GHG mitigation projects through carbon credits?" | Yes / No / Unknown | If No: E1-9 auto-excluded. If Unknown: shown with review flag. |
| Internal carbon pricing | "Has the company implemented an internal carbon pricing mechanism?" | Yes / No | If No: E1-10 auto-excluded |
| High water stress operations | "Does the company have operations in areas identified as high water stress?" | Yes / No / Unknown | Affects E3 routing specificity |
| Produces/uses substances of concern | "Does the company produce, use, or distribute substances of concern or substances of very high concern (e.g. REACH-regulated substances)?" | Yes / No / Unknown | If No: E2-5 auto-excluded |
| Number of material IROs | "How many material IROs will we be mapping today?" | Integer | Used for progress tracking |

After collecting all fields, Gemini confirms them back to the specialist in a summary and asks for confirmation before proceeding.

**Output of Phase 0:** A `session_context` JSON object (see Part 3).

### 1.3 Phase 1: ESRS 2 mandatory block

Gemini presents the always-mandatory ESRS 2 DRs as a single block. These are not subject to materiality assessment and do not require IRO-level confirmation. The specialist confirms once that they understand these apply across the entire sustainability statement.

Gemini displays:

```
The following ESRS 2 General Disclosures are always mandatory regardless of your 
materiality assessment outcomes. They apply to your entire sustainability statement 
and do not require confirmation per IRO.

BASIS FOR PREPARATION
  BP-1  Basis for preparation of the sustainability statement
  BP-2  Phasing-in options used (if applicable)

GOVERNANCE
  GOV-1  Role of AMSB in relation to sustainability
  GOV-2  Integration of sustainability performance in incentive schemes
  GOV-3  Statement on due diligence
  GOV-4  Risk management and internal controls over sustainability reporting

STRATEGY
  SBM-1  Strategy, business model and value chain
  SBM-2  Interests and views of stakeholders
  SBM-3  Interaction of material IROs with strategy and business model, 
          and financial effects [note: IRO-specific content populated per IRO in Phase 2]
  IRO-1  Process to identify and assess material IROs
  IRO-2  Material IROs and DRs included in the sustainability statement

GENERAL DISCLOSURE REQUIREMENTS FOR POLICIES, ACTIONS, METRICS AND TARGETS
  GDR-P  General Disclosure Requirement for Policies
  GDR-A  General Disclosure Requirement for Actions and Resources
  GDR-M  General Disclosure Requirement for Metrics
  GDR-T  General Disclosure Requirement for Targets
  [Note: GDR-P, GDR-A, GDR-M, GDR-T are triggered per material topic in Phase 2, 
   but are always mandatory when a topic is material]

Please confirm you understand these apply across all material topics.
```

Specialist responds to confirm. Gemini records the mandatory block and moves to Phase 2.

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

If one or more IROs were flagged during Phase 2 as not mapping to any standard in the ESRS topic list (AR 16), they are handled here with a separate workflow (see Part 2, Section 2.7).

### 1.6 Phase 4: Session close

After all IROs are processed, Gemini:
1. Outputs a session summary JSON (total IROs mapped, total DRs in scope, DRs by standard)
2. Lists any items flagged for follow-up (Unknown answers from Phase 0, low-confidence mappings, entity-specific IROs)
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
| `verifier` | Who validated this IRO as material in the DMA | Optional |

If required fields are missing, Gemini asks for them before proceeding. Gemini does not guess at required fields.

### 2.2 Step 2.2 — Standard routing rules

Gemini applies routing rules to identify which ESRS topical standards are candidates for this IRO. An IRO may route to one or more standards.

**Routing is determined by three inputs, applied in order:**

**Input A — ESRS hint (highest priority):** If the DMA already indicates an ESRS standard, treat this as the primary candidate. Still apply Input B and C to identify secondary candidates.

**Input B — Keyword matching:** Match IRO topic and description text against the keyword routing rules in the Knowledge Base (Part 5). Produce a list of candidate standards ranked by number of keyword matches.

**Input C — IRO type modifier:** Apply the IRO type routing modifiers from the Knowledge Base (Part 5.2) to adjust the candidate list.

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

**Filtering logic:**

**Rule F1 — Sub-topic relevance:**
Map the IRO description to the relevant sub-topic(s) within the standard using the sub-topic → DR cross-reference in the Knowledge Base (Part 4.2). Only propose DRs that are linked to the relevant sub-topic(s). Do not propose DRs linked exclusively to sub-topics not triggered by this IRO.

**Rule F2 — IRO type filter:**
- If IRO type is Impact: always propose PAT DRs (GDR-P/A/T equivalents) and engagement/remedy DRs. Also propose metrics DRs linked to the triggered sub-topic.
- If IRO type is Risk: always propose PAT DRs, metrics DRs, and the anticipated financial effects DR for the standard (E1-11, E2-6, E3-5, E4-6, E5-6). Also propose strategy DRs if the risk informs business model resilience.
- If IRO type is Opportunity: always propose strategy DRs (E1-1, SBM-3) and anticipated financial effects DR. PAT DRs apply if the company is actively pursuing the opportunity.

**Rule F3 — Fact-based exclusions (from Phase 0 company context):**
- If `uses_carbon_credits` = No → exclude E1-9
- If `internal_carbon_pricing` = No → exclude E1-10
- If `produces_uses_substances_of_concern` = No → exclude E2-5
- If any of the above = Unknown → include the DR with a "Review recommended" confidence flag and note: "Only applies if [condition] — confirm with client"

**Rule F4 — Phase-in check:**
Compare the reporting year and wave from Phase 0 against the phase-in provisions in the Knowledge Base (Part 1.3). For any DR subject to phase-in:
- If the reporting year falls within the phase-in window: flag the DR as "Phase-in available — may be omitted"
- Include the DR in the proposed mapping but clearly label it
- The specialist still decides whether to use the phase-in or report early

**Rule F5 — ESRS 2 linkage:**
Always append the following to the proposed DR list for any material topical standard, without requiring specialist confirmation (these are automatic):
- SBM-3 (IRO interaction with strategy — IRO-specific content)
- IRO-2 (this IRO will be listed as a material matter)
- GDR-P (policies for this topic)
- GDR-A (actions for this topic)
- GDR-T (targets for this topic)

Note: GDR-M (metrics) is only appended automatically if at least one metrics DR is proposed in the topical standard.

**Output of Step 2.3:**
A filtered, ranked list of proposed DRs for the specialist to review, with confidence tier and any flags attached to each DR.

### 2.4 Step 2.4 — De-duplication logic

Before presenting the proposed DRs to the specialist, Gemini checks each proposed DR against the session's existing mapping decisions.

**De-duplication rules:**

**Case A — DR not previously seen:**
Present as a new proposed DR. Standard flow applies.

**Case B — DR previously confirmed as in scope, same sub-topic scope:**
Do not re-present for confirmation. Automatically carry the existing decision forward. Note in the IRO output: "E1-4 already confirmed in scope via IRO [n]. No additional sub-topic scope added."

**Case C — DR previously confirmed as in scope, NEW sub-topic scope added by this IRO:**
Present to specialist with the following framing:
```
E1-4 (Policies related to climate change mitigation and adaptation) was previously 
confirmed in scope via IRO [n] — [IRO description], covering [existing sub-topics].

This IRO adds a new dimension: [new sub-topic].

Do you want to extend E1-4 to also cover [new sub-topic]?
  → Yes, extend scope (recommended)
  → No, existing scope is sufficient
```
This is a scope extension decision, not a new in/out decision. Rationale field is pre-populated with the extension context and can be edited.

**Case D — DR previously confirmed as out of scope:**
Present to specialist with the following framing:
```
E1-4 was previously excluded from scope via IRO [n] — rationale: [rationale].

This IRO also triggers E1-4. Do you want to reconsider?
  → Keep out of scope
  → Bring into scope (requires updated rationale)
```

**Case E — DR previously marked as partial/phase-in:**
Present as Case C logic — confirm whether new IRO changes the scope or phasing decision.

**Output of Step 2.4:**
Updated proposed DR list with de-duplication flags applied. New DRs, scope extensions, and reconsidered exclusions are clearly distinguished.

### 2.5 Step 2.5 — Present proposed mapping

Gemini presents the proposed mapping for this IRO in a structured format. The presentation must include:

**Header:**
```
IRO [n] of [total] — [IRO type] | [Materiality basis]
Topic: [IRO topic]
Description: [IRO description]
Routing: [primary standards] | [secondary standards if any]
```

**Proposed DRs table:**
For each proposed DR, show:
- DR code and name
- Sub-topic(s) it covers for this IRO
- Confidence tier: "Directly triggered" or "Review recommended"
- Any flags: phase-in available, fact-conditional, de-duplication status
- Pre-populated rationale (editable)
- Decision field: In scope / Out of scope / Partial (phase-in applies) [REQUIRED — cannot advance without a decision]

**ESRS 2 automatic DRs (shown separately, greyed out, no decision required):**
- SBM-3, IRO-2, GDR-P, GDR-A, GDR-T (and GDR-M if metrics DRs proposed)

### 2.6 Step 2.6 — Collecting specialist decisions

For each proposed DR, the specialist must select one of:
- **In scope** — this DR applies and the company will report against it
- **Out of scope** — this DR does not apply or is not material; rationale required
- **Partial / Phase-in applies** — the DR applies but the company will use a phase-in provision; specify which phase-in

**Validation rule V1:** Gemini does not advance to the next IRO until every proposed DR has a decision recorded. If the specialist tries to advance without completing all decisions, Gemini lists the outstanding DRs.

**Validation rule V2:** An "Out of scope" decision requires a non-empty rationale. Gemini prompts for one if the field is blank.

**Validation rule V3:** A "Partial / Phase-in applies" decision requires the specialist to confirm which phase-in provision is being used (from the Knowledge Base Part 1.3). Gemini suggests the applicable provision based on Phase 0 context.

### 2.7 Step 2.7 — Rationale collection and pre-population

For each proposed DR, Gemini pre-populates a rationale using the following construction logic:

**Rationale construction formula:**
```
[IRO type] relating to [sub-topic] identified as material via [materiality basis]. 
[DR name] is triggered because [disclosure objective in plain language]. 
[Phase-in note if applicable.]
[Fact-conditional note if applicable.]
```

**Pre-populated rationale examples:**

For E1-4, Risk, Climate change mitigation:
> "Financial materiality risk relating to climate change mitigation identified as material. E1-4 (Policies related to climate change mitigation and adaptation) is triggered because the undertaking must describe its policies for managing this material climate-related risk. Disclosure uses the GDR-P narrative framework — no prescriptive sub-elements required under simplified ESRS."

For S1-13, Impact, Working conditions:
> "Impact materiality impact on own workforce working conditions identified as material. S1-13 (Health and safety metrics) is triggered because the undertaking must disclose work-related injury rates, fatalities and ill-health cases for its material own workforce topic. Note: phase-in available — this DR may be omitted prior to FY2027."

For G1-1, Risk, Corruption and bribery:
> "Financial materiality risk relating to corruption and bribery identified as material. G1-1 (Business conduct policies and corporate culture) is triggered because the undertaking must describe its anti-corruption and bribery policies, whistleblower protection mechanisms, and how it evaluates corporate culture."

The specialist can edit the pre-populated rationale or replace it entirely. For "In scope" decisions, a rationale is recommended but not mandatory (the pre-populated text satisfies the requirement). For "Out of scope" decisions, an edited rationale explaining the exclusion is mandatory.

### 2.8 Step 2.8 — Completeness validation

Before outputting the IRO JSON, Gemini validates:

| Check | Rule | Action if failed |
|-------|------|-----------------|
| V1: All DRs decided | Every proposed DR has a decision | List outstanding DRs, do not advance |
| V2: Out of scope rationale | Every out-of-scope decision has a non-empty rationale | Prompt for rationale |
| V3: Phase-in specified | Every partial decision has a phase-in provision identified | Prompt for provision |
| V4: Required IRO fields | All required IRO fields are populated | Prompt for missing fields |
| V5: Sub-topic scope | At least one sub-topic confirmed per triggered standard | Warn if no sub-topic confirmed |

### 2.9 Step 2.9 — Output confirmed IRO mapping JSON

Once all validations pass, Gemini outputs the IRO mapping JSON (schema defined in Part 3) and confirms to the specialist:

```
IRO [n] mapping confirmed. [x] DRs in scope, [y] out of scope, [z] partial/phase-in.
JSON record ready for system of record.

Ready for IRO [n+1]? Or type 'review' to revisit this IRO.
```

### 2.10 Step 2.10 — Advance or review

Specialist either:
- Confirms readiness for next IRO → Gemini moves to next IRO
- Types "review" → Gemini re-presents the current IRO decisions for editing
- Types "back" → Gemini re-presents the previous IRO decisions for editing (allowed at any point)

### 2.7 Entity-specific IRO workflow

If an IRO does not map to any standard in the ESRS topic list (AR 16), Gemini flags it and routes it to this separate workflow.

**Trigger condition:** Standard routing (Step 2.2) returns zero candidate standards, OR the specialist indicates the IRO covers a topic not in AR 16.

**Entity-specific flow:**

```
Step ES-1: Confirm the IRO is genuinely entity-specific
Step ES-2: Identify the closest AR 16 topic(s) for partial mapping
Step ES-3: Document entity-specific disclosure requirements
Step ES-4: Output entity-specific JSON record
```

**Step ES-1 — Confirm entity-specific status:**
Gemini asks: "This IRO doesn't appear to map directly to a standard ESRS topic. Before treating it as entity-specific, can you confirm it isn't covered by any of these related topics: [list closest AR 16 topics based on partial keyword matches]?"

If specialist confirms entity-specific: proceed to ES-2.
If specialist identifies an AR 16 topic: return to standard routing with that topic confirmed.

**Step ES-2 — Partial mapping to closest AR 16 topic:**
Even entity-specific IROs often partially overlap with a standard topic. Identify the closest match and note which aspects are standard-covered vs. entity-specific.

Example: "Emerging Tech & AI" partially maps to G1 (business conduct, governance of AI risks) but the environmental and social impact dimensions of AI products are entity-specific.

**Step ES-3 — Document entity-specific disclosure requirements:**
Gemini asks the specialist to confirm the following for entity-specific IROs:

| Field | Guidance |
|-------|----------|
| Topic label | How this will be labelled in the sustainability statement |
| Closest ESRS standard(s) | Which standard(s) partially cover this topic |
| Entity-specific aspects | Which aspects are not covered by any ESRS standard |
| GDR-P applies? | Will the company describe policies for this topic? |
| GDR-A applies? | Will the company describe actions for this topic? |
| GDR-M applies? | Will the company disclose metrics for this topic? |
| GDR-T applies? | Will the company set targets for this topic? |
| ESRS 1 para 11 confirmed | Confirm: entity-specific disclosure will be included per ESRS 1 §11 |
| IRO-2 entry | This IRO will be listed in IRO-2 as an entity-specific material matter |
| BP-1 entry | Entity-specific disclosure will be listed in the basis for preparation |

**Step ES-4 — Output entity-specific JSON:**
Gemini outputs an entity-specific IRO record following the schema in Part 3.

---

## PART 3: JSON OUTPUT SCHEMA

All JSON outputs produced by the Gem follow this schema. The schema is designed for CRUD operations against the system of record spreadsheet.

### 3.1 Session context object (output of Phase 0)

```json
{
  "record_type": "session_context",
  "session_id": "string — unique identifier, format: YYYYMMDD-COMPANYNAME",
  "created_at": "ISO 8601 datetime",
  "company_name": "string",
  "reporting_year": "string — e.g. FY2027",
  "esrs_wave": "Wave 1 | Wave 2 | Wave 4",
  "company_context": {
    "uses_carbon_credits": "Yes | No | Unknown",
    "internal_carbon_pricing": "Yes | No",
    "high_water_stress_operations": "Yes | No | Unknown",
    "produces_uses_substances_of_concern": "Yes | No | Unknown"
  },
  "total_iros": integer,
  "session_status": "In progress | Complete"
}
```

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

### 3.3 IRO mapping record (output of Phase 2 per IRO)

This is the primary record type. One record is output per IRO.

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
      "E1": integer,
      "E2": integer,
      "E3": integer,
      "E4": integer,
      "E5": integer,
      "S1": integer,
      "S2": integer,
      "S3": integer,
      "S4": integer,
      "G1": integer
    }
  },
  "flags_for_follow_up": ["string"],
  "iro_ids_mapped": [integer],
  "iro_ids_entity_specific": [integer]
}
```

---

## PART 4: CRUD OPERATION DEFINITIONS

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

**CREATE: session_context**
- Populate Cover sheet metadata fields (company name, reporting year, wave, session ID, date)
- Create header row in IRO Register with session metadata

**CREATE: mandatory_block**
- For each DR in `mandatory_drs`: append a row to DR Decision Log with verdict = "Always mandatory", rationale = "ESRS 2 General Disclosure — applies regardless of materiality assessment", review_status = "Confirmed"
- Append these DRs to Stakeholder Summary under "ESRS 2 — General Disclosures (Always Mandatory)" section

**CREATE: iro_mapping (new DR decisions)**
For each entry in `dr_decisions` where `is_deduplication` = false OR `deduplication_type` = "New":
- Append row to Full Mapping sheet with all IRO and DR fields
- Append row to DR Decision Log (consolidated by DR code)
- Append row to IRO Register (one row per IRO, summarising DR counts)
- If verdict = "In scope": append DR to Stakeholder Summary under the relevant standard section

**CREATE: entity_specific_iro**
- Append row to IRO Register flagged as entity-specific
- Append rows to Entity-Specific Log sheet

### 4.3 UPDATE operations

**UPDATE: iro_mapping (scope extension — Case C de-duplication)**
Triggered when `is_deduplication` = true AND `deduplication_type` = "Scope extension"

- Locate existing row in DR Decision Log where `dr_code` matches AND `session_id` matches
- Append new sub-topic to `sub_topics_covered` field (do not overwrite — concatenate with delimiter)
- Append new IRO reference to `triggered_by_iros` field
- Append updated rationale note: "[original rationale] | Extended via IRO [n]: [new sub-topic]"
- Append new row to Full Mapping sheet (one row per IRO per DR is maintained for audit trail)
- Do NOT create a duplicate in DR Decision Log — update the existing row only

**UPDATE: iro_mapping (reconsidered exclusion — Case D de-duplication)**
Triggered when `is_deduplication` = true AND `deduplication_type` = "Reconsidered exclusion"

- Locate existing row in DR Decision Log where `dr_code` matches
- Update verdict from "Out of scope" to new verdict
- Update rationale to reflect the change: "[original rationale] | Reconsidered via IRO [n]: [updated rationale]"
- Add row to Full Mapping sheet with the updated decision
- Update Stakeholder Summary if verdict changed to "In scope"

**UPDATE: session_summary**
- Update Cover sheet KPI cells (DRs in scope total, IROs mapped, standards triggered)
- Update any formula-driven cells that aggregate from other sheets

### 4.4 De-duplication key

The system of record uses a compound key for de-duplication lookups:

```
deduplication_key = session_id + "__" + dr_code
```

The Apps Script uses this key to determine whether a DR record already exists before creating or updating.

### 4.5 Record immutability rule

Once a record is written to the Full Mapping sheet, it is never deleted or overwritten. The Full Mapping sheet is an append-only audit log. All changes are recorded as new rows with a `deduplication_type` flag. The DR Decision Log is the consolidated view and IS updated in place.

---

## PART 5: VALIDATION RULES SUMMARY

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
- Gemini proactively tracks progress and reminds the specialist where they are ("IRO 3 of 11 — 3 DRs remaining for confirmation")
- Gemini does not add DRs to the proposed list that are not supported by the Knowledge Base. If the specialist asks about a DR not in the Knowledge Base, Gemini flags it for entity-specific handling.
- Gemini does not make final decisions. It proposes; the specialist confirms.

### 6.2 Handling ambiguity

- If an IRO is ambiguous and could map to multiple standards with similar confidence, Gemini presents both options and asks the specialist to choose. It does not pick one arbitrarily.
- If an IRO description is very short or vague, Gemini asks a clarifying question before routing. It does not route based on insufficient information.
- If a specialist's rationale is very short (e.g. "yes" or "applies"), Gemini prompts: "Could you add a brief note on why this DR is relevant to this IRO? This will support the assurance trail."

### 6.3 Tone and format

- Keep responses structured and concise. Use tables and bullet lists for DR proposals. Avoid long paragraphs during the mapping flow.
- Confirm each completed action: "IRO 3 confirmed. JSON record ready."
- When presenting de-duplication cases, always quote the original IRO and rationale so the specialist has full context without needing to scroll back.

### 6.4 What Gemini does NOT do

- Does not assess whether an IRO should be material — this is taken as given input
- Does not modify the ESRS Knowledge Base during a session
- Does not store session state between separate Gem conversations — the specialist must start a new session or re-import prior session JSON to continue
- Does not produce the final Excel output — it produces JSON that feeds the Apps Script

---

## PART 7: OPEN ITEMS FOR REVIEW

| Item | Question | Recommended resolution |
|------|----------|----------------------|
| GDR-M auto-trigger | Should GDR-M be automatically added when metrics DRs are proposed, or only when the specialist explicitly confirms metrics will be disclosed? | Currently set to auto-add when metrics DRs are proposed — confirm this is right |
| Confidence scoring in MVP | Two-tier (Directly triggered / Review recommended) is defined. Should a third tier be added for very low confidence / speculative mappings? | Keep two tiers for MVP, review after first client session |
| Session resumption | The spec states the Gem does not store state between conversations. The specialist must re-import prior JSON to continue. Should we add a "resume session" command that takes the prior session JSON as input? | Recommended for MVP — simple to implement, avoids losing progress |
| Mandatory rationale for in-scope DRs | Currently only out-of-scope decisions require rationale. Should in-scope decisions also require rationale (pre-populated text counts)? | Pre-populated text satisfies the requirement — no change needed |
| Multi-standard IROs | When an IRO maps to two standards (e.g. S3 and G1), does the specialist confirm DRs for both standards in a single screen, or standard by standard? | Recommended: standard by standard within the same IRO screen — cleaner UX |

---

*End of Mapping Logic Specification v1.0*
*Companion document: ESRS_Knowledge_Base_Simplified_v1.md*
*Next artifacts: JSON schema validation examples, Spreadsheet shell, Apps Script specification*
