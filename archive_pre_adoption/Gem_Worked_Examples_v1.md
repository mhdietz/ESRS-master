# ESRS DMA Mapping — Worked Examples
## Purpose: Reference examples for Gemini to pattern-match JSON output format against
## Version: 1.0 | Source IROs: Anonymized sample DMA
## Note: These are illustrative examples, not prescriptive mapping decisions.
## The specialist must still confirm all decisions — these show FORMAT, not correct answers.

---

## HOW TO USE THESE EXAMPLES

When producing JSON output, match the structure, field names, field order, and value formats shown here exactly. Do not invent new fields. Do not omit fields that appear here. Where a field is null or not applicable, include it as null — do not omit it.

---

## EXAMPLE 1: PHASE 0 — SESSION CONTEXT OUTPUT

**Scenario:** Sustainability specialist begins a new mapping session for a technology company with 11 material IROs to map.

**Specialist input (Phase 0 answers):**
- Company: Trimble Inc.
- Reporting year: FY2027
- Wave: Wave 2
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
  "esrs_wave": "Wave 2",
  "company_context": {
    "uses_carbon_credits": "No",
    "internal_carbon_pricing": "No",
    "high_water_stress_operations": "Unknown",
    "produces_uses_substances_of_concern": "No"
  },
  "total_iros": 11,
  "session_status": "In progress"
}
```

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
- E1-2 Identification of climate-related risks and scenario analysis — Directly triggered (MANDATORY regardless of materiality)
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
      "sub_topics_covered": ["Climate change mitigation", "Climate change adaptation", "Energy"],
      "confidence_tier": "Directly triggered",
      "verdict": "In scope",
      "phase_in_provision": null,
      "rationale": "E1-2 is mandatory per ESRS 2 Appendix C regardless of materiality outcome. This DR requires disclosure of the methodology to identify and assess climate-related physical and transition risks, and results of any climate scenario analysis. Directly relevant to regulatory risk assessment requirements.",
      "is_deduplication": false,
      "deduplication_type": null,
      "prior_iro_id": null,
      "flags": ["Always mandatory per ESRS 2 Appendix C"]
    },
    {
      "dr_code": "E1-3",
      "dr_name": "Resilience in relation to climate change",
      "standard": "E1",
      "sub_topics_covered": ["Climate change mitigation", "Climate change adaptation"],
      "confidence_tier": "Review recommended",
      "verdict": "In scope",
      "phase_in_provision": null,
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
      "rationale": "Financial materiality risk relating to climate change mitigation. E1-8 is triggered because the undertaking must disclose gross Scope 1, 2 and 3 GHG emissions using the financial control boundary per simplified ESRS. Directly required to meet regulatory reporting obligations identified in this IRO.",
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
      "phase_in_provision": "Quantitative information on anticipated financial effects may be omitted for financial years prior to FY2030 per ESRS 1 simplified phase-in provisions. Qualitative disclosure still required from FY2027.",
      "rationale": "Financial materiality risk with clear financial effect dimension (regulatory fines, compliance costs). E1-11 is triggered to disclose anticipated financial effects from transition risks. Wave 2 entity reporting FY2027 — quantitative phase-in applied; qualitative disclosure confirmed in scope.",
      "is_deduplication": false,
      "deduplication_type": null,
      "prior_iro_id": null,
      "flags": ["Phase-in available — quantitative omitted until FY2030"]
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
12. Verdict values are exactly: "In scope" or "Out of scope" or "Partial / Phase-in applies" — no other values
13. Deduplication type values are exactly: "New" or "Scope extension" or "Reconsidered exclusion" or null
