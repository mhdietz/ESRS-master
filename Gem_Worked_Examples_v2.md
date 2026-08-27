# ESRS DMA Mapping — Worked Examples
## Purpose: Reference examples for Gemini to pattern-match JSON output format against
## Version: 2.1 | Basis: Commission Delegated Regulation C(2026) 5010 (adopted 3 July 2026) | Source IROs: Anonymized sample DMA
## Note: These are illustrative examples, not prescriptive mapping decisions.
## The specialist must still confirm all decisions — these show FORMAT, not correct answers.
## Schema notes: session_context includes `regulatory_basis`, `dma_approach`, `ghg_boundary_approach`. Out-of-scope DR decisions include `retention_basis` (the ESRS 1 para 107–108 carve-out under which non-material content is retained, or null) — an "Out of scope" verdict means the DR/datapoint is PROHIBITED from the statement (ESRS 1 para 24), not merely "not required." A separate `commercial_prejudice_omission` flag covers material-but-omitted content (ESRS 1 Ch. 7.7 / para 100) — do not conflate the two. GDR-M is part of the ESRS 2 mandatory block. See KB §1.2a, §1.7.

---

## HOW TO USE THESE EXAMPLES

When producing JSON output, match the structure, field names, field order, and value formats shown here exactly. Do not invent new fields. Do not omit fields that appear here. Where a field is null or not applicable, include it as null — do not omit it.

---

## EXAMPLE 1: PHASE 0 — SESSION CONTEXT OUTPUT

**Scenario:** Sustainability specialist begins a new mapping session for a technology company with 11 material IROs to map.

**Specialist input (Phase 0 answers):**
- Company: Trimble Inc.
- Reporting year: FY2027
- Reporting population: Other undertakings (Wave 2)
- Reporting regime: Revised ESRS (mandatory for Wave 2 from FY2027)
- DMA approach: Bottom-up (or Top-down / Hybrid — see KB §1.7)
- GHG boundary approach: Financial control (GHG Protocol 2004) — default per adopted E1 AR 19; alternatives: equity share / operational control
- Uses carbon credits: No
- Internal carbon pricing: No
- High water stress operations: Unknown
- Substances of concern: No
- Total IROs: 11

**Gem JSON output:**

```json
{
  "record_type": "session_context",
  "session_id": "20270101-TrimbleInc",
  "created_at": "2027-01-01T09:00:00Z",
  "company_name": "Trimble Inc.",
  "reporting_year": "FY2027",
  "regulatory_basis": "Revised ESRS — Commission Delegated Regulation C(2026) 5010 (adopted 3 July 2026)",
  "kb_version": "2.0",
  "esrs_wave": "Wave 2",
  "reporting_population": "Other undertakings",
  "reporting_regime": "Revised ESRS (mandatory FY2027)",
  "dma_approach": "Bottom-up",
  "company_context": {
    "uses_carbon_credits": "No",
    "internal_carbon_pricing": "No",
    "high_water_stress_operations": "Unknown",
    "produces_uses_substances_of_concern": "No",
    "ghg_boundary_approach": "Financial control (GHG Protocol 2004)"
  },
  "total_iros": 11,
  "session_status": "In progress"
}
```

**Field notes:**
- `regulatory_basis` / `kb_version` — provenance fields recording which regulatory text and KB version the session was mapped against. `reporting_regime` values: `"Revised ESRS (mandatory FY2027)"`, `"Revised ESRS (early adoption FY2026)"`, or `"Existing ESRS with reliefs (FY2026)"` — the adopted DA offers this third transition path (old ESRS + eight specific reliefs). Wave 2 is always the first.
- `reporting_population` — `"Other undertakings"` or `"Wave-one undertakings"` (ESRS 1 para 122); drives the §1.3 phase-in schedule. For "Other undertakings," E4/S2/S3/S4 and AFE phase-ins are keyed to the **first two** reporting years, quantitative AFE to the **first four** — not fixed calendar years.
- `dma_approach` — `"Top-down"`, `"Bottom-up"`, or `"Hybrid"` (ESRS 1 para 27). Under top-down, the IRO queue may be topic-level rather than fully enumerated.
- `ghg_boundary_approach` — per adopted ESRS E1 AR 19, default `"Financial control (GHG Protocol 2004)"`; permitted alternatives `"Equity share"` or `"Operational control"` if the undertaking so elects. ESRS 1 paras 71/72 and AR 36 (joint operations) prevail over E1-8; E1-8 is carved out of the para 91 partial-boundary relief. Record the elected approach here so E1-8 reporting is consistent with it.

---

## EXAMPLE 2: PHASE 1 — MANDATORY BLOCK OUTPUT

**Scenario:** Specialist confirms the ESRS 2 mandatory block.

**Gem JSON output (abbreviated — shows structure; full mandatory_drs list includes all 15 ESRS 2 DRs):**

```json
{
  "record_type": "mandatory_block",
  "session_id": "20270101-TrimbleInc",
  "confirmed_at": "2027-01-01T09:05:00Z",
  "mandatory_drs": [
    {
      "dr_code": "BP-1",
      "dr_name": "Basis for preparation of the sustainability statement",
      "standard": "ESRS2",
      "always_mandatory": true,
      "phase_in_available": false,
      "phase_in_note": null
    },
    {
      "dr_code": "GOV-1",
      "dr_name": "Role of the administrative, management and supervisory bodies in relation to sustainability",
      "standard": "ESRS2",
      "always_mandatory": true,
      "phase_in_available": false,
      "phase_in_note": null
    },
    {
      "dr_code": "SBM-3",
      "dr_name": "Interaction of material impacts, risks and opportunities with strategy and business model, and financial effects",
      "standard": "ESRS2",
      "always_mandatory": true,
      "phase_in_available": false,
      "phase_in_note": null
    },
    {
      "dr_code": "IRO-1",
      "dr_name": "Description of the process to identify and assess material impacts, risks and opportunities and material information to be reported",
      "standard": "ESRS2",
      "always_mandatory": true,
      "phase_in_available": false,
      "phase_in_note": null
    },
    {
      "dr_code": "GDR-P",
      "dr_name": "General Disclosure Requirement for Policies",
      "standard": "ESRS2",
      "always_mandatory": true,
      "phase_in_available": false,
      "phase_in_note": "Applies per material topic when topic is confirmed material"
    }
  ]
}
```

---

## EXAMPLE 3: PHASE 2 — STANDARD IRO MAPPING (RISK, FINANCIAL MATERIALITY)

**Scenario:** IRO 1 — Climate-related regulatory/legal risk.

**Specialist input:**
```
Topic: Climate Risk & Resilience
Description: Policy/Legal Risk — Increasing regulatory focus on measuring and 
reporting on climate risk management, including decarbonisation plan requirements.
Type: Risk
Materiality basis: Financial
ESRS hint: E1
Verifier: Chief Sustainability Officer
```

**Gem routing proposal (Step 2.2):**
Primary: E1 (Climate Change) — Directly triggered (ESRS hint present + keywords: regulatory, climate, reporting, decarbonisation)
Secondary: G1 (Business Conduct) — Review recommended (regulatory compliance dimension)

Specialist confirms: E1 primary only.

**Gem DR filtering result (Step 2.3):**
Sub-topic triggered: Climate change mitigation (regulatory/transition risk)

Proposed DRs:
- E1-1 Transition plan — Directly triggered (transition risk implies transition plan disclosure)
- E1-2 Identification of climate-related risks and scenario analysis — Directly triggered (material climate-related risk; materiality-conditional, NOT always-mandatory)
- E1-3 Resilience in relation to climate change — Review recommended (resilience disclosure relevant to regulatory transition risk)
- E1-4 Policies related to climate change mitigation and adaptation — Directly triggered (via GDR-P, material topic)
- E1-5 Actions and resources — Directly triggered (via GDR-A)
- E1-6 Targets related to climate change — Directly triggered (via GDR-T, regulatory decarbonisation targets)
- E1-7 Energy consumption and mix — Review recommended (energy relevant to decarbonisation)
- E1-8 Gross Scope 1, 2, 3 GHG emissions — Directly triggered (regulatory reporting requires GHG data)
- E1-9 GHG removals — EXCLUDED (carbon credits = No per company context)
- E1-10 Internal carbon pricing — EXCLUDED (internal carbon pricing = No per company context)
- E1-11 Anticipated financial effects — Directly triggered (financial materiality risk, regulatory fines/costs)

ESRS 2 automatic (no decision needed): SBM-3, IRO-2, GDR-P, GDR-A, GDR-T, GDR-M

**Specialist decisions:**
- E1-1: In scope
- E1-2: In scope (mandatory)
- E1-3: In scope
- E1-4: In scope
- E1-5: In scope
- E1-6: In scope
- E1-7: In scope
- E1-8: In scope
- E1-11: Partial / Phase-in applies (Wave 2, FY2027 — quantitative information phase-in until FY2030)

**Gem JSON output:**

```json
{
  "record_type": "iro_mapping",
  "session_id": "20270101-TrimbleInc",
  "iro_id": 1,
  "iro_data": {
    "iro_topic": "Climate Risk & Resilience",
    "iro_description": "Policy/Legal Risk — Increasing regulatory focus on measuring and reporting on climate risk management, including decarbonisation plan requirements.",
    "iro_type": "Risk",
    "materiality_basis": "Financial",
    "esrs_hint": "E1",
    "verifier": "Chief Sustainability Officer"
  },
  "routing": {
    "primary_standards": ["E1"],
    "secondary_standards": [],
    "routing_rationale": "ESRS hint E1 present. Keywords matched: regulatory, climate, reporting, decarbonisation. G1 secondary considered but specialist confirmed E1 only."
  },
  "dr_decisions": [
    {
      "dr_code": "E1-1",
      "dr_name": "Transition plan for climate change mitigation",
      "standard": "E1",
      "sub_topics_covered": ["Climate change mitigation"],
      "confidence_tier": "Directly triggered",
      "verdict": "In scope",
      "phase_in_provision": null,
      "retention_basis": null,
      "commercial_prejudice_omission": false,
      "rationale": "Financial materiality risk relating to climate change mitigation identified as material. E1-1 is triggered because the undertaking must disclose its transition plan for climate change mitigation, including GHG reduction targets and decarbonisation levers. The regulatory focus on decarbonisation plans makes this directly relevant.",
      "is_deduplication": false,
      "deduplication_type": null,
      "prior_iro_id": null,
      "flags": []
    },
    {
      "dr_code": "E1-2",
      "dr_name": "Identification of climate-related risks and scenario analysis",
      "standard": "E1",
      "sub_topics_covered": ["Climate change mitigation"],
      "confidence_tier": "Directly triggered",
      "verdict": "In scope",
      "phase_in_provision": null,
      "retention_basis": null,
      "commercial_prejudice_omission": false,
      "rationale": "Material climate-related transition risk identified. E1-2 is triggered to disclose, for this material risk, whether it is a physical or transition risk (para 15) and the key elements of the methodology used to assess exposure and sensitivity of assets/activities over the short/medium/long term (para 16). Where climate-related scenario analysis is used, the scenario ranges, scope, assumptions and time period are disclosed (para 17). E1-2 is materiality-conditional under adopted ESRS E1 — it is not always-mandatory.",
      "is_deduplication": false,
      "deduplication_type": null,
      "prior_iro_id": null,
      "flags": []
    },
    {
      "dr_code": "E1-3",
      "dr_name": "Resilience in relation to climate change",
      "standard": "E1",
      "sub_topics_covered": ["Climate change mitigation", "Climate change adaptation"],
      "confidence_tier": "Review recommended",
      "verdict": "In scope",
      "phase_in_provision": null,
      "retention_basis": null,
      "commercial_prejudice_omission": false,
      "rationale": "Financial materiality risk relating to climate change resilience. E1-3 is triggered because the undertaking must disclose the results of its climate resilience analysis and how its strategy and business model respond to climate-related risks. Relevant given regulatory scrutiny of resilience planning.",
      "is_deduplication": false,
      "deduplication_type": null,
      "prior_iro_id": null,
      "flags": []
    },
    {
      "dr_code": "E1-4",
      "dr_name": "Policies related to climate change mitigation and adaptation",
      "standard": "E1",
      "sub_topics_covered": ["Climate change mitigation"],
      "confidence_tier": "Directly triggered",
      "verdict": "In scope",
      "phase_in_provision": null,
      "retention_basis": null,
      "commercial_prejudice_omission": false,
      "rationale": "Financial materiality risk relating to climate change mitigation identified as material. E1-4 is triggered via GDR-P — the undertaking must describe its climate change mitigation and adaptation policies under the narrative GDR-P framework.",
      "is_deduplication": false,
      "deduplication_type": null,
      "prior_iro_id": null,
      "flags": []
    },
    {
      "dr_code": "E1-8",
      "dr_name": "Gross Scope 1, 2, 3 GHG emissions",
      "standard": "E1",
      "sub_topics_covered": ["Climate change mitigation"],
      "confidence_tier": "Directly triggered",
      "verdict": "In scope",
      "phase_in_provision": null,
      "retention_basis": null,
      "commercial_prejudice_omission": false,
      "rationale": "Material climate-related transition risk relating to climate change mitigation. E1-8 is triggered because the undertaking must disclose gross Scope 1, 2 (location- and market-based) and 3 GHG emissions by significant category (para 30). Reporting boundary is financial control per the GHG Protocol (2004), with equity share or operational control permitted as alternatives (adopted ESRS E1 AR 19); ESRS 1 paras 71/72 and AR 36 (joint operations) prevail, and E1-8 is carved out of the ESRS 1 para 91 partial-boundary relief. Boundary recorded in session_context.ghg_boundary_approach.",
      "is_deduplication": false,
      "deduplication_type": null,
      "prior_iro_id": null,
      "flags": []
    },
    {
      "dr_code": "E1-11",
      "dr_name": "Anticipated financial effects from material physical and transition risks and potential climate-related opportunities",
      "standard": "E1",
      "sub_topics_covered": ["Climate change mitigation"],
      "confidence_tier": "Directly triggered",
      "verdict": "Partial / Phase-in applies",
      "phase_in_provision": "Other undertakings (Wave 2): all AFE information may be omitted for the FIRST TWO reporting years, and quantitative AFE for the FIRST FOUR reporting years, except E1-11 paras 39(a)(b) and 40(a)(b) (ESRS 1 para 127(b)-(c)). FY2027 = first reporting year → qualitative AFE may be omitted this year; excepted datapoints still required.",
      "retention_basis": null,
      "commercial_prejudice_omission": false,
      "rationale": "Financial materiality risk with clear financial-effect dimension (regulatory fines, compliance costs). E1-11 is triggered to disclose anticipated financial effects from transition risks. As an 'Other undertaking' in its first reporting year (FY2027), AFE phase-in under ESRS 1 para 127 applies; excepted datapoints (paras 39(a)(b), 40(a)(b)) remain in scope.",
      "is_deduplication": false,
      "deduplication_type": null,
      "prior_iro_id": null,
      "flags": ["Phase-in: AFE omitted first two years; quantitative AFE first four years (ESRS 1 para 127); excepted E1-11 datapoints still required"]
    }
  ],
  "esrs2_automatic_drs": [
    {
      "dr_code": "SBM-3",
      "dr_name": "Interaction of material impacts, risks and opportunities with strategy and business model, and financial effects",
      "triggered_by_this_iro": true,
      "note": "This IRO will be described in SBM-3 as a material climate-related transition risk and its interaction with strategy documented."
    },
    {
      "dr_code": "IRO-2",
      "dr_name": "Material impacts, risks and opportunities and DRs included in the sustainability statement",
      "triggered_by_this_iro": true,
      "note": "This IRO will be listed as a material matter in IRO-2 alongside the DRs confirmed in scope."
    },
    {
      "dr_code": "GDR-P",
      "dr_name": "General Disclosure Requirement for Policies",
      "triggered_by_this_iro": true,
      "note": "E1 confirmed material — GDR-P applies for climate change mitigation and adaptation policies."
    },
    {
      "dr_code": "GDR-A",
      "dr_name": "General Disclosure Requirement for Actions and Resources",
      "triggered_by_this_iro": true,
      "note": "E1 confirmed material — GDR-A applies for climate change actions and resources."
    },
    {
      "dr_code": "GDR-T",
      "dr_name": "General Disclosure Requirement for Targets",
      "triggered_by_this_iro": true,
      "note": "E1 confirmed material — GDR-T applies for climate change targets."
    },
    {
      "dr_code": "GDR-M",
      "dr_name": "General Disclosure Requirement for Metrics",
      "triggered_by_this_iro": true,
      "note": "Metrics DRs proposed (E1-7, E1-8) — GDR-M applies."
    }
  ],
  "entity_specific": false,
  "confirmed_at": "2027-01-01T09:25:00Z"
}
```

---

## EXAMPLE 3b: PHASE 2 — OUT-OF-SCOPE DECISIONS (PROHIBITION + CARVE-OUT)

**Scenario:** Within IRO 1's E1 mapping, two DRs are proposed but assessed non-material for this undertaking: E1-9 (GHG removals/carbon credits — company uses none) and E1-10 (internal carbon pricing — none in place). Under the adopted ESRS 1 para 24, a non-material DR is **prohibited** from the statement, not merely "not required." The tool records this explicitly, capturing whether any ESRS 1 para 107–108 carve-out nonetheless applies.

**Key rule:** verdict "Out of scope" ⇒ the DR/datapoint shall NOT be reported (ESRS 1 para 24). The only routes back in are the three carve-outs in `retention_basis` (other legislation / GAAP-or-framework such as GRI / specific user demand) — each of which must then be flagged as NOT resulting from the materiality assessment (para 109). This is distinct from `commercial_prejudice_omission`, which is for information that IS material but is withheld under ESRS 1 Ch. 7.7 (commercial prejudice, trade secret, classified).

**Gem JSON output (the two out-of-scope decisions from IRO 1):**

```json
{
  "record_type": "iro_mapping",
  "session_id": "20270101-TrimbleInc",
  "iro_id": 1,
  "dr_decisions": [
    {
      "dr_code": "E1-9",
      "dr_name": "GHG removals and GHG mitigation projects financed through carbon credits",
      "standard": "E1",
      "sub_topics_covered": ["Climate change mitigation"],
      "confidence_tier": "Directly triggered",
      "verdict": "Out of scope",
      "phase_in_provision": null,
      "retention_basis": null,
      "commercial_prejudice_omission": false,
      "rationale": "Company context confirms no use of carbon credits or GHG removal projects. Non-material. Under ESRS 1 para 24 this DR shall not be reported. No para 107-108 carve-out applies (retention_basis null).",
      "is_deduplication": false,
      "deduplication_type": null,
      "prior_iro_id": null,
      "flags": ["Prohibited from statement per ESRS 1 para 24 — non-material"]
    },
    {
      "dr_code": "E1-10",
      "dr_name": "Internal carbon pricing",
      "standard": "E1",
      "sub_topics_covered": ["Climate change mitigation"],
      "confidence_tier": "Directly triggered",
      "verdict": "Out of scope",
      "phase_in_provision": null,
      "retention_basis": null,
      "commercial_prejudice_omission": false,
      "rationale": "No internal carbon pricing mechanism in place. Non-material; shall not be reported (ESRS 1 para 24).",
      "is_deduplication": false,
      "deduplication_type": null,
      "prior_iro_id": null,
      "flags": ["Prohibited from statement per ESRS 1 para 24 — non-material"]
    }
  ],
  "confirmed_at": "2027-01-01T09:28:00Z"
}
```

**Illustrative carve-out case (for pattern reference):** if a non-material datapoint were nonetheless retained because another EU law requires it, the block would read `"verdict": "Out of scope"`, `"retention_basis": "Other legislation"`, and a flag such as `"Retained as supplementary per ESRS 1 para 107 — clearly identified as not resulting from the materiality assessment (para 109)"`. Permitted `retention_basis` values: `"Other legislation"`, `"Generally accepted framework (e.g. GRI)"`, `"Specific user data demand"`, or `null`.

---

## EXAMPLE 4: PHASE 2 — DE-DUPLICATION SCOPE EXTENSION (CASE C)

**Scenario:** IRO 2 is a second climate risk — physical/operational. E1-4 was already confirmed in scope via IRO 1 covering climate change mitigation. IRO 2 adds climate change adaptation as a new sub-topic.

**Specialist input:**
```
Topic: Climate Risk & Resilience
Description: Operational/Supply Chain Disruption — Exposure to acute and chronic 
climate risks like extreme weather events disrupting operations and supply chain.
Type: Risk
Materiality basis: Financial
ESRS hint: E1
Verifier: VP of Supply Chain
```

**Gem de-duplication check result:**
E1-4 was previously confirmed in scope via IRO 1, covering sub-topic: Climate change mitigation.
IRO 2 adds sub-topic: Climate change adaptation (physical risk dimension).
→ Case C: Scope extension.

**Gem presents to specialist:**
"E1-4 (Policies related to climate change mitigation and adaptation) was previously confirmed in scope via IRO 1 — Policy/Legal Risk, covering climate change mitigation. This IRO adds a new dimension: climate change adaptation (physical risk). Do you want to extend E1-4 to also cover adaptation?"

Specialist confirms: Yes, extend scope.

**Gem JSON output for the scope extension decision only:**

```json
{
  "record_type": "iro_mapping",
  "session_id": "20270101-TrimbleInc",
  "iro_id": 2,
  "iro_data": {
    "iro_topic": "Climate Risk & Resilience",
    "iro_description": "Operational/Supply Chain Disruption — Exposure to acute and chronic climate risks like extreme weather events disrupting operations and supply chain.",
    "iro_type": "Risk",
    "materiality_basis": "Financial",
    "esrs_hint": "E1",
    "verifier": "VP of Supply Chain"
  },
  "routing": {
    "primary_standards": ["E1"],
    "secondary_standards": [],
    "routing_rationale": "ESRS hint E1 present. Keywords matched: climate, physical risk, extreme weather, supply chain disruption, operations."
  },
  "dr_decisions": [
    {
      "dr_code": "E1-4",
      "dr_name": "Policies related to climate change mitigation and adaptation",
      "standard": "E1",
      "sub_topics_covered": ["Climate change mitigation", "Climate change adaptation"],
      "confidence_tier": "Directly triggered",
      "verdict": "In scope",
      "phase_in_provision": null,
      "retention_basis": null,
      "commercial_prejudice_omission": false,
      "rationale": "E1-4 previously confirmed in scope via IRO 1 covering climate change mitigation. Extended via IRO 2 to also cover climate change adaptation — physical risks from extreme weather require adaptation policy disclosure. E1-4 now covers both mitigation and adaptation sub-topics.",
      "is_deduplication": true,
      "deduplication_type": "Scope extension",
      "prior_iro_id": 1,
      "flags": ["Sub-topic scope extended from mitigation only to mitigation + adaptation"]
    },
    {
      "dr_code": "E1-3",
      "dr_name": "Resilience in relation to climate change",
      "standard": "E1",
      "sub_topics_covered": ["Climate change adaptation"],
      "confidence_tier": "Directly triggered",
      "verdict": "In scope",
      "phase_in_provision": null,
      "retention_basis": null,
      "commercial_prejudice_omission": false,
      "rationale": "Financial materiality risk relating to climate change adaptation — physical risk dimension. E1-3 is triggered because the undertaking must disclose results of its climate resilience analysis and capacity to adapt strategy and business model to physical climate risks including extreme weather.",
      "is_deduplication": false,
      "deduplication_type": null,
      "prior_iro_id": null,
      "flags": []
    }
  ],
  "esrs2_automatic_drs": [
    {
      "dr_code": "SBM-3",
      "dr_name": "Interaction of material impacts, risks and opportunities with strategy and business model, and financial effects",
      "triggered_by_this_iro": true,
      "note": "This IRO will be described in SBM-3 as a material climate-related physical risk — operational and supply chain disruption."
    },
    {
      "dr_code": "IRO-2",
      "dr_name": "Material impacts, risks and opportunities and DRs included in the sustainability statement",
      "triggered_by_this_iro": true,
      "note": "This IRO listed as a separate material matter in IRO-2 — physical risk distinct from transition risk of IRO 1."
    }
  ],
  "entity_specific": false,
  "confirmed_at": "2027-01-01T09:40:00Z"
}
```

---

## EXAMPLE 5: PHASE 3 — ENTITY-SPECIFIC IRO

**Scenario:** IRO — "Risk from not fully understanding environmental and social impacts of advanced AI solutions in operations and products." No direct AR 16 topic match. Partially maps to G1 for governance/conduct dimension.

**Gem entity-specific flow output:**

```json
{
  "record_type": "entity_specific_iro",
  "session_id": "20270101-TrimbleInc",
  "iro_id": 6,
  "iro_data": {
    "iro_topic": "Emerging Tech & AI",
    "iro_description": "Risk from not fully understanding environmental and social impacts of advanced AI solutions in operations and products.",
    "iro_type": "Risk",
    "materiality_basis": "Financial",
    "verifier": "AI Strategy Lead & Data Science Lead"
  },
  "entity_specific_details": {
    "topic_label_for_report": "Responsible AI and emerging technology impacts",
    "closest_esrs_standards": ["G1"],
    "entity_specific_aspects": "Environmental and social impacts of AI products are not covered by any AR 16 topic at the required granularity. G1 covers the governance and business conduct dimension (AI ethics policies, oversight). The environmental impact of AI infrastructure (energy use) would fall under E1 if material separately. The social impact of AI on end-users would fall under S4 if material separately. These are assessed as entity-specific given the company's specific AI product exposure.",
    "gdr_p_applies": true,
    "gdr_a_applies": true,
    "gdr_m_applies": true,
    "gdr_t_applies": false,
    "esrs1_para11_confirmed": true,
    "iro2_entry_required": true,
    "bp1_entry_required": true
  },
  "partial_standard_mappings": [
    {
      "standard": "G1",
      "dr_decisions": [
        {
          "dr_code": "G1-1",
          "dr_name": "Business conduct policies and corporate culture",
          "standard": "G1",
          "sub_topics_covered": ["Corporate culture"],
          "confidence_tier": "Review recommended",
          "verdict": "In scope",
          "phase_in_provision": null,
          "retention_basis": null,
          "commercial_prejudice_omission": false,
          "rationale": "The governance and oversight dimension of AI risk partially maps to G1-1 — the undertaking should describe its approach to responsible AI as part of its corporate culture and business conduct policies. Entity-specific aspects of AI environmental and social impact require additional disclosure beyond G1-1.",
          "is_deduplication": false,
          "deduplication_type": null,
          "prior_iro_id": null,
          "flags": ["Entity-specific IRO — partial G1 mapping only"]
        }
      ]
    }
  ],
  "confirmed_at": "2027-01-01T10:15:00Z"
}
```

---

## EXAMPLE 6: PHASE 4 — SESSION SUMMARY

**Scenario:** All 11 IROs mapped. Session complete.

```json
{
  "record_type": "session_summary",
  "session_id": "20270101-TrimbleInc",
  "completed_at": "2027-01-01T12:00:00Z",
  "totals": {
    "iros_mapped": 9,
    "iros_entity_specific": 2,
    "drs_in_scope": 24,
    "drs_out_of_scope": 3,
    "drs_partial_phase_in": 4,
    "drs_by_standard": {
      "ESRS2": 15,
      "E1": 9,
      "E2": 0,
      "E3": 0,
      "E4": 0,
      "E5": 0,
      "S1": 6,
      "S2": 2,
      "S3": 2,
      "S4": 4,
      "G1": 4
    }
  },
  "flags_for_follow_up": [
    "High water stress operations answered Unknown — confirm with client whether E3 routing is needed",
    "IRO 6 (Emerging Tech & AI) treated as entity-specific — confirm topic label and metric approach with client",
    "IRO 7 (AI Regulations) treated as entity-specific — confirm G1 partial mapping is sufficient or whether additional entity-specific DRs needed",
    "E1-11 phase-in applied for IROs 1 and 2 — confirm client will provide qualitative anticipated financial effects from FY2027"
  ],
  "iro_ids_mapped": [1, 2, 3, 4, 5, 8, 9, 10, 11],
  "iro_ids_entity_specific": [6, 7]
}
```

---

## KEY FORMAT RULES (SUMMARY)

Always follow these regardless of IRO content:

1. `record_type` is always the first field in every JSON object
2. `session_id` is always the second field
3. `dr_decisions` is an array — even if only one DR decision, it is an array
4. `sub_topics_covered` is always an array — even if only one sub-topic
5. `flags` is always an array — empty array `[]` if no flags, never null
6. `is_deduplication` is always a boolean — never a string
7. `phase_in_provision` is a descriptive string when applicable, null when not
8. `prior_iro_id` is an integer when applicable, null when not
9. `entity_specific` in iro_mapping is always boolean false — entity-specific IROs use the entity_specific_iro record type instead
10. Timestamps are always ISO 8601 format
11. Confidence tier values are exactly: "Directly triggered" or "Review recommended" — no other values
12. Verdict values are exactly: "In scope" or "Out of scope" or "Partial / Phase-in applies" — no other values. **"Out of scope" means the DR/datapoint is PROHIBITED from the statement (ESRS 1 para 24), not "optional."**
13. Deduplication type values are exactly: "New" or "Scope extension" or "Reconsidered exclusion" or null
14. `retention_basis` is present on every DR decision: one of "Other legislation", "Generally accepted framework (e.g. GRI)", "Specific user data demand", or null. Non-null only when a non-material item is retained as supplementary under ESRS 1 paras 107–108 (and must then be flagged as not resulting from the materiality assessment, para 109).
15. `commercial_prejudice_omission` is always a boolean. true only when MATERIAL information is omitted under ESRS 1 Ch. 7.7 / para 100 (commercial prejudice, trade secret, classified). Never use this for non-material items — those use verdict "Out of scope" with retention_basis.
16. `session_context` always includes `regulatory_basis`, `kb_version`, `reporting_population`, `reporting_regime`, `dma_approach`, and `company_context.ghg_boundary_approach`.
17. The ESRS 2 mandatory block is 15 DRs and **includes GDR-M** (BP-1, BP-2, GOV-1..4, SBM-1..3, IRO-1, IRO-2, GDR-P, GDR-A, GDR-M, GDR-T).
