# ESRS Knowledge Base — Simplified Standards
## Source: Commission Delegated Regulation C(2026) 5010 — revised ESRS, adopted 3 July 2026 (amending Delegated Regulation (EU) 2023/2772)
## Purpose: Input for AI-assisted DMA → ESRS mapping tool (Gemini system prompt)
## Version: 2.1

## Regulatory status
The Delegated Act was adopted by the Commission on 3 July 2026 and is in the European Parliament / Council two-month scrutiny period (extendable by two months). It cannot be amended, only accepted or rejected in full. It enters into force on publication in the Official Journal and applies to financial years beginning on or after 1 January 2027, with optional early application for FY2026. Confirm entry-into-force / OJ publication status before relying on this KB for a live filing.

---

## PART 1: ARCHITECTURE AND FOUNDATIONAL RULES

### 1.1 Structure of the simplified ESRS

The simplified ESRS consists of:
- **ESRS 1** — General Requirements (architecture, drafting conventions, materiality rules, fair presentation, reporting obligations). Does not contain Disclosure Requirements, but does contain mandatory Application Requirements (ARs) with the same authority as the main text.
- **ESRS 2** — General Disclosures (cross-cutting; the undertaking applies these DRs when reporting on material IROs and the topics related to them). Contains BP, GOV, SBM, IRO and the four General Disclosure Requirements (GDR-P/A/M/T).
- **Topical standards** — E1 through G1 (apply only when the relevant topic relates to a material impact, risk or opportunity).

Sector-specific standards have been eliminated under the simplified ESRS. No sector standards will be adopted.

The simplified ESRS uses **topics and sub-topics only** (ESRS 1 Appendix A); there is no "sub-sub-topic" layer.

**Entity-specific disclosures (ESRS 1 para 11):** If the undertaking concludes that a topic related to a material IRO is not covered, or **not covered with sufficient granularity**, by an ESRS, it shall provide entity-specific disclosures, taking account of the fair presentation provisions in Chapter 2. Entity-specific disclosure is a positive obligation triggered by granularity gaps, not merely a residual bucket for unmapped IROs. It may arise from sectoral specificities or other facts and circumstances specific to the undertaking.

### 1.2 Mandatory vs. materiality-conditional DRs

**Always mandatory (apply regardless of materiality assessment outcome):**
- All DRs in ESRS 2 (BP-1, BP-2, GOV-1 through GOV-4, SBM-1 through SBM-3, IRO-1, IRO-2, GDR-P, GDR-A, GDR-M, GDR-T)
- Climate "not material" rule (ESRS 2 IRO-2 para 37(b)): if the undertaking concludes climate change is NOT material and therefore omits all E1 DRs, it must disclose the basis for that conclusion. This is the only always-present climate obligation.
- E1-2 ("Identification of climate-related risks and scenario analysis", paras 14–17) is a normal **materiality-conditional** DR, not always-mandatory. Its scenario-analysis element is itself conditional ("if used", para 17).

**Materiality-conditional (apply only if the relevant topic relates to a material IRO):**
- All other DRs in topical standards E1 through G1.

### 1.2a The materiality prohibition (ESRS 1 paras 23–24)

Under the adopted text this is a **prohibition, not a permission**. ESRS 1 para 24: except for supplementary information under Chapter 8.2, the undertaking **shall not disclose** information prescribed by an ESRS DR or datapoint if that information is not material, and **shall not disclose** non-material information when providing entity-specific disclosures under para 11.

Implication for this tool: an "Out of scope" verdict is an instruction **not to report**, not a neutral "not required." The mapping engine must treat non-material DRs as prohibited from the sustainability statement, subject only to the narrow carve-outs below.

**Carve-outs — the only routes by which non-material content may still appear (ESRS 1 paras 107–108):**
1. **Other legislation** — information another law requires the undertaking to disclose.
2. **Generally accepted frameworks** — content stemming from generally accepted reporting standards/frameworks, including non-mandatory or sector guidance from other standard-setters (e.g. GRI).
3. **Specific user data demand** — supplementary disclosures needed to meet the data demands of a specific user.

Any such content (ESRS 1 para 109) shall be (a) clearly identified as **not** resulting from the materiality assessment, (b) faithfully represented, and (c) presented so as not to obscure material information.

**Distinguish from commercial-prejudice omission (ESRS 1 para 100 / Chapter 7.7):** that is the opposite situation — information that IS material but MAY be omitted because it is seriously prejudicial to commercial position, or is a trade secret (Directive (EU) 2016/943), or is classified information. Do not conflate "non-material but retained under a carve-out" with "material but omitted for prejudice." The schema captures these separately (see Part 4 / worked-examples schema notes).

**Special materiality rules in S1 (adopted ESRS S1 obj. para 1):**
- S1-5 (Characteristics of the undertaking's employees) applies if the undertaking concludes its own workforce is material — an explicit exception (obj. para 1(a)) to the para-30 sub-topic routing. The para-23 information-materiality filter still applies.
- S1-6 (Characteristics of non-employees in the undertaking's own workforce) applies if the undertaking concludes non-employees in its own workforce are connected to material IROs (obj. para 1(b); AR 12).

### 1.3 Phase-in provisions (adopted ESRS 1, Chapter 10, paras 121–127)

Phase-in is keyed to the undertaking's **population** and to its **first years of reporting**, not to fixed calendar-year cutoffs — except for Wave-one undertakings, which use fixed calendar-year cutoffs (see below).

**Population definitions (para 122):**
- **Wave-one undertakings** — required to report for financial years starting between 1 Jan 2024 and 31 Dec 2026.
- **Other undertakings** — required to report for financial years starting on or after 1 Jan 2027. *(Trimble Wave 2 = "Other undertakings.")*

Para 121: earlier voluntary application of ESRS does **not** trigger the start of the phase-in clock and does not limit use of the reliefs. "Financial year" here refers to reporting periods starting on/after 1 January of the respective year.

**For "Other undertakings" (para 127) — the relevant case for a Wave 2 client:**
| Item | May be omitted for... |
|------|----------------------|
| All DRs of E4, S2, S3, S4 (para 127(a)) | the **first two** financial years of reporting |
| All information on anticipated financial effects (ESRS 2 para 27 and E1-11) (para 127(b)) | the **first two** financial years of reporting, except E1-11 para 39(a)(b) and 40(a)(b) |
| Quantitative information on anticipated financial effects (para 127(c)) | the **first four** financial years of reporting, except E1-11 para 39(a)(b) and 40(a)(b) |
| Quantitative information on substances of concern (E2-5) (para 127(d)) | the **first three** financial years of reporting |
| Information on SVHC (if user of articles containing them) (para 127(e)) | the **first** financial year of reporting |
| S1-6, S1-7 (own employees non-EEA), S1-10, S1-11, S1-12, S1-13 datapoints in para 37(d)(e) and non-employee datapoints, S1-14 (para 127(f)) | the **first** year of reporting |

**For Wave-one undertakings (paras 125–126):** two sub-populations split by size (the EUR 450m net turnover / 1,000 employee threshold). Above threshold (para 125): may omit E4/S2/S3/S4 for financial years **prior to FY2027**; AFE prior to **FY2028**; quantitative AFE prior to **FY2030**; SoC quantitative prior to **FY2030**; with the same E1-11 para 39(a)(b)/40(a)(b) exception. Below threshold (para 126): may omit all topical DRs for years prior to FY2027 (subject to ESRS 2 paras 7–10), with the same AFE/SoC/SVHC/S1 schedule.

The E1-11 exception paragraph references throughout paras 125–127 are **39(a)(b) and 40(a)(b)** — these match the internal numbering used in the E1 DR table below (E1-11 paras 39–40).

**BP-2 linkage (ESRS 2):** whenever a phase-in omission under para 125(a)/126(a)/127(a) is used, BP-2 requires the undertaking to still disclose whether those topics were assessed material, and if so to give the topic/sub-topic, a brief business-model/strategy note, targets, policies, actions and relevant metrics (ESRS 2 BP-2 paras 8–9). Other phase-in omissions (125(b)–(e) etc.) require a bare statement of the fact (BP-2 para 10). The tool should auto-populate a BP-2 follow-up flag whenever any phase-in is applied.

### 1.4 Key structural changes from 2023 ESRS to simplified ESRS

| Area | 2023 ESRS | Simplified ESRS |
|------|-----------|-----------------|
| Strategy & governance | Repeated in each topical standard | Consolidated entirely in ESRS 2 |
| PAT disclosures | Prescriptive sub-elements per topic | Narrative flexibility via GDR-P, GDR-A, GDR-T in ESRS 2 |
| Sector-specific standards | Planned | Eliminated |
| Sub-subtopics | Present | Eliminated — topics and sub-topics only |
| Voluntary datapoints | 269 "may disclose" datapoints | All eliminated |
| Mandatory datapoints | Baseline | Reduced by 61% |
| GHG reporting boundary | Operational control | **Financial control** (GHG Protocol 2004) is the default (adopted ESRS E1 AR 19), with **equity share** or **operational control** as permitted alternatives. ESRS 1 paras 71/72 and AR 36 (joint operations) prevail over E1-8. Where ESRS and GHG Protocol conflict, ESRS prevails (AR 20(a)). |
| Fair presentation | Implicit | Explicit principle in ESRS 1 Chapter 2 (paras 19–20, AR 6–7). Applies to the sustainability statement **taken as a whole**, not datapoint-by-datapoint; can require adding entity-specific information. See 1.6. |
| Topic list location & status | "AR 16 topic list", mandatory input to DMA | **Appendix A of ESRS 1**, explicitly **non-binding guidance**. In the adopted text, "AR 16" is a different provision — *Level of the materiality assessment*. |

### 1.5 General Disclosure Requirements for PAT (GDR-P, GDR-A, GDR-M, GDR-T)

Policies, actions, metrics, and targets are governed by four General Disclosure Requirements in ESRS 2 that apply across all material topics, rather than by prescriptive topic-specific requirements:

- **GDR-P** — General Disclosure Requirement for Policies: For each material topic, describe policies adopted to manage material impacts, risks and opportunities. Narrative approach; no mandatory sub-elements specified per topic.
- **GDR-A** — General Disclosure Requirement for Actions and Resources: Describe key actions taken and resources allocated. May reference actions across topics if they serve multiple purposes.
- **GDR-M** — General Disclosure Requirement for Metrics: Disclose metrics that are material for understanding the undertaking's performance on material topics.
- **GDR-T** — General Disclosure Requirement for Targets: Disclose time-bound, outcome-oriented targets for material topics.

In topical standards, DRs for policies, actions, and targets reference GDR-P, GDR-A, and GDR-T rather than containing standalone requirements. The topical DRs for PAT add topic-specific context but the core disclosure logic sits in ESRS 2. ESRS 2 para 39: if the undertaking has no policy/action/target for a material topic, it shall disclose that fact.

### 1.6 Fair presentation (ESRS 1 Chapter 2, paras 19–20; AR 6–7)

Fair presentation requires disclosure of relevant information about material IROs and their faithful representation (complete, neutral, accurate). It also requires information that is **comparable, verifiable and understandable** (Appendix B qualitative characteristics).

Key operating rule (AR 6): the fair-presentation test is applied to the sustainability statement **taken as a whole** — the overall picture across the statement, not individual disclosures in isolation. This **can require adding entity-specific information** (and applying the para 24 non-material filter). Making use of reliefs in Chapters 5.4, 7.3, 7.4 or 7.7 is not detrimental to fair presentation provided the undertaking explains the consequences and resulting limitations.

Implication for the tool: entity-specific disclosure (para 11) is a fair-presentation obligation that can be triggered even where a topical DR *is* mapped, if that DR does not cover a material IRO with sufficient granularity. The mapping engine should run a granularity/sufficiency check on mapped IROs (Phase 2), not only route unmapped IROs to the entity-specific branch (Phase 3).

### 1.7 Top-down approach to materiality (ESRS 1 para 27; AR 9–10)

The undertaking **may** derive a materiality/non-materiality conclusion without further assessment, on the basis of an analysis of its strategy and business model — sectors, geographies, and features of the upstream and downstream value chain ("top-down" approach). If materiality is not evident from that analysis, it must then perform a specific assessment of the IROs in question.

- AR 9: a top-down approach lets the undertaking avoid materiality assessment at the level of individual IROs; the conclusion can be reached **at topic level for combined impacts, risks and opportunities**. A more granular level is needed only where it could reasonably change the conclusion.
- AR 10: top-down for some topics **may be combined with** bottom-up for others (hybrid).

Implication: Phase 0 must record which approach produced the DMA (top-down / bottom-up / hybrid), because it affects how the IRO queue was built and is a question an assurance provider will ask. Note that under a top-down approach the input "IRO queue" may be expressed at topic level rather than as discrete IROs — the tool must not assume a fully enumerated bottom-up IRO list.

### 1.8 Consideration of mitigating measures in the DMA (ESRS 1 para 43; AR 26–28)

- **43(a) Actual negative impacts:** assessed as they actually manifested in the reporting year. The assessment **shall not consider** remediation activities undertaken during the reporting period.
- **43(b) Potential negative impacts:** severity/likelihood assessed taking account of implemented prevention/mitigation policies and actions **only if** they can reasonably be assumed to effectively reduce severity or likelihood. Actions/policies **not yet implemented shall not be considered** (AR 27: a policy implying future actions is not counted).
- **43(c):** information about impacts and how they are managed may be decision-useful **irrespective of** how effectively they are managed — so managed-down impacts may still require disclosure.
- Positive impacts (paras 41, QC7/QC8): assessed **without netting** against negative impacts; mere mitigation/prevention or legal compliance does not qualify as a positive impact.

Implication: the tool should not let a specialist mark an IRO immaterial purely because a policy/action exists, unless that action is implemented and effective (43(b)); and must flag managed impacts that may still be decision-useful (43(c)).

### 1.9 Reporting on material opportunities (ESRS 1 para 102)

The undertaking **shall not report general opportunities for the sector** — only opportunities **currently being pursued or incorporated in its general strategy**. Opportunity-type IROs that fail this test should be flagged rather than routed to disclosure. Financial-effects provisions in ESRS 2 apply when reporting opportunities.

### 1.10 Reasonable and supportable information / undue cost or effort (ESRS 1 Chapter 7.4, paras 93–95; AR 45)

Across the DMA and preparation of the statement, the undertaking uses all reasonable and supportable information available at the reporting date **without undue cost or effort**. Quantitative scoring is not necessarily required (AR 13); an exhaustive search is not required. "Undue cost or effort" is not fixed — it is reassessed each period against the undertaking's size, resources and technical readiness. Value-chain relief: the undertaking has latitude to use proxies/estimates for value-chain data (paras 33, 93), except E1-8 Scope 1/2/3 which is carved out of the partial-boundary relief (para 91).

---

## PART 2: ESRS 2 — GENERAL DISCLOSURES (Always Mandatory)

**Full name:** ESRS 2 General Disclosures
**Applicability:** Cross-cutting. The undertaking applies these DRs when providing information on material IROs and the related topics (ESRS 2 paras 1–2). BP/GOV/SBM/IRO and GDR-P/A/M/T are the always-present block.
**Source:** Commission Delegated Regulation C(2026) 5010 (adopted 3 July 2026), ESRS 2.

### Basis for Preparation

| DR Code | DR Name | Disclosure Objective | Key Content |
|---------|---------|---------------------|-------------|
| BP-1 | Basis for preparation of the sustainability statement | Enable understanding of the basis on which the sustainability statement is prepared | Statement of compliance with ESRS; any reliefs or options applied; scope of consolidation; list of DRs complied with; list of supplementary information |
| BP-2 | Specific information if phasing-in options are used | Enable understanding of which phase-in provisions have been applied | Which DRs have been omitted under phase-in; whether omitted topics have been assessed as material |

### Governance

| DR Code | DR Name | Disclosure Objective | Key Content |
|---------|---------|---------------------|-------------|
| GOV-1 | Role of administrative, management and supervisory bodies (AMSB) in relation to sustainability | Enable understanding of governance roles and responsibilities | Composition of AMSB incl. % independent members, % by gender; roles and responsibilities of AMSB re sustainability; oversight processes |
| GOV-2 | Integration of sustainability-related performance in incentive schemes | Enable understanding of how sustainability is integrated in remuneration | Whether and how sustainability performance is linked to incentive schemes for AMSB members and senior executives |
| GOV-3 | Statement on due diligence | Enable understanding of due diligence processes | Description of due diligence process related to material sustainability matters |
| GOV-4 | Risk management and internal controls over sustainability reporting | Enable understanding of risk management and internal controls | How risk management and internal control processes relate to sustainability reporting |

### Strategy

| DR Code | DR Name | Disclosure Objective | Key Content |
|---------|---------|---------------------|-------------|
| SBM-1 | Strategy, business model and value chain | Enable understanding of business model and value chain | Description of business model; key products/services; markets; value chain; how strategy relates to sustainability |
| SBM-2 | Interests and views of stakeholders | Enable understanding of how stakeholder views are considered | Key stakeholders identified; engagement approach; how interests and views are reflected in strategy |
| SBM-3 | Interaction of material impacts, risks and opportunities with strategy and business model, and financial effects | Enable understanding of how material IROs interact with strategy and business model, and related financial effects | High-level description of how material impacts originate from strategy/business model and effects of risks and opportunities on business model and value chain (para 24); **current** financial effects on financial position, performance and cash flows (para 25); risks/opportunities with significant risk of material adjustment to carrying amounts next period (para 26); **anticipated** financial effects over short/medium/long term (para 27); qualitative resilience of strategy/business model (para 33). Quantitative AFE may be omitted where not separately identifiable, measurement uncertainty too high, or the undertaking lacks skills/capabilities/resources (paras 28–29) — but qualitative info and combined-effects info must then be given (para 31). Commercial-prejudice omission (ESRS 1 Ch. 7.7) also applies to AFE (AR 17(d)). |
| IRO-1 | Description of the process to identify and assess material impacts, risks and opportunities and material information to be reported | Enable understanding of the DMA process | Concise description of the process and decision-making steps, including approach to own operations and value chain, key methodologies, inputs, assumptions, qualitative considerations / quantitative thresholds (para 35(a)); how impacts prioritised by severity and likelihood and how mitigating actions and heightened-risk areas were considered (35(b)); whether informed by due diligence and how stakeholder/expert consultation informs it (35(c)); significant changes vs prior period (35(d)); **when it last updated its materiality assessment** (35(e)). |
| IRO-2 | Material impacts, risks and opportunities and disclosure requirements included in the sustainability statement | Enable understanding of the materiality-assessment outcome and what is reported | Concise description of material IROs and where connected in own operations / value chain (37(a)); **the basis for concluding climate change is not material, if E1 is entirely omitted (37(b))**; changes vs prior period (37(c)); list of DRs complied with, indicating incorporation by reference (37(d)); list of supplementary (Ch. 8.2) information (37(e)); exposure to heightened risk of forced/compulsory labour and child labour by operations/geography where connected to such material impacts (37(f)); table of datapoints deriving from other EU legislation, indicating location or "not material" (37(g)). |

### General Disclosure Requirements for Policies, Actions, Metrics and Targets

The undertaking applies GDR-P/A/M/T when disclosing PAT information either under a topical standard **or on an entity-specific basis** (ESRS 2 para 40).

| DR Code | DR Name | When it Applies | Notable content in adopted text |
|---------|---------|----------------|--------------------------------|
| GDR-P | General Disclosure Requirement for Policies | Policies adopted to manage material IROs | Key contents, scope, third-party standards referenced; for social topics, consideration of affected stakeholders (para 42). Para 43: disclose whether there is an overarching human-rights policy committing to UNGPs / ILO / OECD Guidelines, and which stakeholder groups it covers. |
| GDR-A | General Disclosure Requirement for Actions and Resources | Key actions taken/planned to manage material IROs | Actions, scope, expected outcomes (para 45); **significant financial resources**: type, amount allocated in the reporting period with line-item references, and an **indicative range of future resources expected** (para 46). |
| GDR-M | General Disclosure Requirement for Metrics | Metrics required by topical standards for material IROs, plus entity-specific metrics (para 48) | Per metric: unit, calculation/estimation methodology, sources, assumptions/limitations; value-chain proxy reliance; contextual info; significant performance changes vs prior periods (para 49). For S2–S4, which prescribe no specific metrics, entity-specific metrics are used (AR 42). |
| GDR-T | General Disclosure Requirement for Targets | Measurable, time-bound, outcome-oriented targets for material IROs (para 51) | Target value, scope, baseline/base year, target year, methodology/assumptions, whether required by law, scenarios, and whether environmental targets are based on conclusive scientific evidence. Para 52: if no measurable outcome-oriented targets, disclose whether/how effectiveness is otherwise tracked. |

**Note when no policy/action/target exists (ESRS 2 para 39):** if the undertaking has no policies, actions or targets for a material topic, it shall disclose this fact.

---

## PART 3: TOPICAL STANDARDS — DISCLOSURE REQUIREMENTS

> **Structural note (applies to every topical standard):** the adopted standards set their DRs at the **standard level**, not per sub-topic. Each standard's objective para 1 states that if not all sub-topics are to be reported following the materiality assessment, ESRS 1 para 30 applies — i.e. sub-topic reporting is routed through the materiality assessment, and DRs are **not** partitioned into per-sub-topic bundles. The "Sub-topics" column below therefore lists the standard's sub-topics as **routing labels** (which sub-topic(s) a given DR speaks to), not as a claim that the DR is gated on one sub-topic. The mapping matrix collapses to standard-level DR lists accordingly.
>
> **AFE routing:** there is no standalone "anticipated financial effects" DR in E2/E3/E4/E5. Anticipated financial effects are consolidated in ESRS 2 SBM-3 (paras 25–27) plus E1-11 (climate only). Adopted topical DR ranges: E2 → E2-1..E2-5; E3 → E3-1..E3-4; E4 → E4-1..E4-5; E5 → E5-1..E5-5.
>
> Sub-topic labels below are sourced from each standard's own **objective paragraph** (the operative, binding sub-topic list, para 5 or 6 of each standard); this is the engine-routing source of truth. ESRS 1 Appendix A provides a **non-binding** overview list of topics and sub-topics (para 10) that is broadly consistent with these labels; Appendix A presents S1 and S2 as a single combined topic row, "Own Workforce and Workers in the Value Chain (ESRS S1/S2)," with a footnote on materiality-assessment depth captured in the S2 section below.

### E1 — Climate Change

**Full name:** ESRS E1 Climate Change
**Applicability:** When climate change relates to material impacts, risks or opportunities (E1 obj. para 2)
**Sub-topics (E1 obj. para 6):** Climate change mitigation | Climate change adaptation | Energy
**Source:** Adopted ESRS E1, Commission Delegated Regulation C(2026) 5010.

E1-2 ("Identification of climate-related risks and scenario analysis", paras 14–17) is a materiality-gated risk-identification DR, not an always-mandatory metrics DR. Scenario analysis within it is explicitly conditional: para 17 opens "If climate-related scenario analysis is used…". The only climate-specific always-present obligation is ESRS 2 IRO-2 para 37(b): if the undertaking concludes climate is not material and omits all of E1, it must disclose the basis for that conclusion.

#### E1 Strategy DRs

| DR Code | DR Name | Sub-topics | Disclosure Objective | Key Content |
|---------|---------|-----------|---------------------|-------------|
| E1-1 | Transition plan for climate change mitigation | Mitigation | Enable understanding of past, current and future mitigation efforts for compatibility with 1.5°C transition | Key features of transition plan: GHG reduction targets, decarbonisation levers, key actions and investments, board approval, alignment with business strategy; CapEx related to coal/oil/gas if applicable; key assumptions and dependencies; locked-in GHG emission assessment; progress in implementing the plan. If no transition plan, disclose this fact and indicate if/when one is expected. |
| E1-2 | Identification of climate-related risks and scenario analysis | Mitigation, Adaptation, Energy | Enable understanding of how the undertaking identifies and assesses climate-related risks and opportunities for financial materiality (paras 14–17) | For each material climate-related risk (per ESRS 2 IRO-2 para 37): whether classified as physical or transition risk (para 15); key elements of the methodology used to assess how assets/business activities in own operations and value chain are exposed and sensitive over short/medium/long term to climate-related hazards and to transition events/trends (para 16). **If** climate-related scenario analysis is used (para 17): ranges of scenarios applied (incl. whether a high-emission scenario and a 1.5°C-aligned scenario were used, and the associated temperature projections), scope of operations, key assumptions, and the time period. Materiality-conditional; scenario analysis is conditional per para 17. |
| E1-3 | Resilience in relation to climate change | Mitigation, Adaptation | Enable understanding of resilience of strategy and business model to climate-related risks | Results of climate resilience analysis; how scenario analysis informs potential responses; how transition plan and mitigation/adaptation actions contribute to resilience; capacity to adjust strategy and business model |

#### E1 Impact, Risk and Opportunity Management DRs

| DR Code | DR Name | Sub-topics | Disclosure Objective | Key Content |
|---------|---------|-----------|---------------------|-------------|
| E1-4 | Policies related to climate change mitigation and adaptation | Mitigation, Adaptation, Energy | Per GDR-P | Climate change mitigation and adaptation policies; energy efficiency and renewable energy policies. References GDR-P for core structure. |
| E1-5 | Actions and resources in relation to climate change mitigation and adaptation | Mitigation, Adaptation, Energy | Per GDR-A | Key actions and resources allocated for climate change mitigation and adaptation, including energy efficiency and renewable energy deployment. References GDR-A for core structure. |

#### E1 Metrics and Targets DRs

| DR Code | DR Name | Sub-topics | Disclosure Objective | Key Content |
|---------|---------|-----------|---------------------|-------------|
| E1-6 | Targets related to climate change | Mitigation, Adaptation, Energy | Enable understanding of targets set for managing climate-related IROs | GHG emission reduction targets (absolute and/or intensity); energy efficiency and renewable energy targets; adaptation targets; whether targets are science-based |
| E1-7 | Energy consumption and mix | Mitigation, Energy | Enable understanding of the undertaking's energy consumption and mix (paras 25–28) | Total energy consumption in MWh related to own operations, disaggregated into fossil / nuclear / renewable sources (para 26). Undertakings in high climate impact sectors further disaggregate fossil consumption (coal, crude oil/petroleum, natural gas, other fossil, purchased fossil electricity/heat/steam/cooling) (para 27). If the undertaking produces energy, disaggregate and disclose non-renewable and renewable energy production separately (para 28). |
| E1-8 | Gross Scope 1, 2, 3 GHG emissions | Mitigation | Enable understanding of direct and indirect climate impacts from own operations and value chain (paras 29–31) | Absolute gross GHG emissions in tCO2eq: Scope 1 (incl. % from EU ETS if applicable); Scope 2 (both location-based and market-based); Scope 3 by each significant category, as total and per category (para 30(a)). Approach used to measure emissions per ESRS 2 GDR-M (para 30(b)). For Scope 1 & 2, disaggregate between the consolidated accounting group and other emissions (para 30(c)). Direct biogenic CO2 from combustion/biodegradation of biomass disclosed separately from Scope 1 (para 31). **Boundary (adopted E1 AR 19):** the reporting boundary is **financial control** as per the GHG Protocol Corporate Accounting and Reporting Standard (2004). The undertaking **may alternatively** use the **equity share** or **operational control** approach (also per the GHG Protocol 2004). ESRS 1 paras 71 (leased assets), 72 (benefit schemes) and AR 36 (joint operations) **prevail** over E1-8; the para 30(c) disaggregation must reflect the chosen boundary. Where ESRS requirements and the GHG Protocol conflict, ESRS takes precedence (AR 20(a)). E1-8 is carved out of the ESRS 1 partial-boundary/value-chain relief (para 91). |
| E1-9 | GHG removals and GHG mitigation projects financed through carbon credits | Mitigation | Enable understanding of use of carbon credits | GHG removals and storage; carbon credits used; mitigation projects financed. Only applies if the undertaking uses such mechanisms. |
| E1-10 | Internal carbon pricing | Mitigation | Enable understanding of internal carbon pricing mechanisms | Description of internal carbon pricing mechanism and price applied. Only applies if the undertaking has implemented such a mechanism. |
| E1-11 | Anticipated financial effects from material physical and transition risks and material climate-related opportunities | Mitigation, Adaptation, Energy | Enable understanding of how material physical risks, transition risks and climate-related opportunities affect financial position and future performance (paras 38–42) | Anticipated financial effects from material **physical** risks: carrying amount of assets at risk before adaptation actions (with time horizons); % addressed by adaptation actions; net revenue at risk (para 39). From material **transition** risks: carrying amount of assets at risk incl. estimated potential stranded assets on a 1.5°C scenario; % addressed by mitigation; real-estate collateral by energy-efficiency class; potential liabilities not yet recognised; net revenue at risk incl. from coal/oil/gas customers (para 40). Methodology, scope, assumptions, limitations (para 41). Assets/revenue tied to identified opportunities (para 42). This information forms part of the ESRS 2 SBM-3 anticipated-financial-effects information. **Phase-in:** for "Other undertakings" (Wave 2), AFE may be omitted for the first two reporting years and quantitative AFE for the first four, **except** paras 39(a)(b) and 40(a)(b) (ESRS 1 para 127). See §1.3. |

---

### E2 — Pollution

**Full name:** ESRS E2 Pollution
**Applicability:** When pollution relates to material impacts, risks or opportunities (E2 obj. para 2)
**Sub-topics (E2 obj. para 6):** Pollution of air | Pollution of water | Pollution of soil | Substances of concern (SoC), including substances of very high concern (SVHC) | Microplastics
**Adopted DR range:** E2-1 .. E2-5
**Source:** Adopted ESRS E2, Commission Delegated Regulation C(2026) 5010.

| DR Code | DR Name | Sub-topics (routing labels) | Disclosure Objective | Key Content |
|---------|---------|-----------|---------------------|-------------|
| E2-1 | Policies related to pollution | All E2 sub-topics | Per GDR-P (para 11) | Pollution-related policies. References ESRS 2 GDR-P. |
| E2-2 | Actions and resources related to pollution | All E2 sub-topics | Per GDR-A (para 12) | Key pollution-related actions and resources allocated to their implementation. References ESRS 2 GDR-A. |
| E2-3 | Targets related to pollution | All E2 sub-topics | Per GDR-T (para 13) | Pollution-related targets. References ESRS 2 GDR-T. |
| E2-4 | Pollution of air, water and soil | Air, Water, Soil, Microplastics | Enable understanding of emissions of pollutants and of microplastics (paras 14–16) | Amounts of material emissions of pollutants to air, water and soil from own operations, incl. from environmental accidents, in the reporting period (para 15). Amounts of primary microplastics manufactured or used, and separately those directly released into the environment (para 16). Presented in relevant mass units (AR 1). |
| E2-5 | Substances of concern and substances of very high concern | SoC / SVHC | Enable understanding of IROs linked to manufacturing, trading or use of SoC/SVHC, incl. regulatory-change risk (paras 17–20) | Chemical-sector manufacturers/formulators/importers (para 18): total weight of SoC, and separately SVHC, procured / manufactured / placed on market / directly released. Users of substances outside para 18 scope (para 19): total weight of SVHC used and directly released. Manufacturers/importers/users of articles containing SVHC (para 20): names of substances present above 0.1% w/w (REACH Art. 33). SVHC grouped by hazard class per CLP (AR 5). **Phase-in (Other undertakings / Wave 2):** quantitative SoC information may be omitted for the **first three financial years of reporting** (ESRS 1 para 127(d)). SVHC information (para 20) has its own, separate phase-in: first financial year only (ESRS 1 para 127(e)). |

---

### E3 — Water and Marine Resources

**Full name:** ESRS E3 Water
**Applicability:** When water relates to material impacts, risks or opportunities (E3 obj. para 2)
**Sub-topics (E3 obj. para 6):** Water use, which includes: water withdrawal | water consumption | water discharge | water stored
**Adopted DR range:** E3-1 .. E3-4
**Note on scope:** the adopted standard is named "Water". Water encompasses freshwater and other types of water incl. seawater (para 7). "Marine resources" is not a listed E3 sub-topic in the adopted objective paragraph; marine-resource inflows are addressed in E5.
**Source:** Adopted ESRS E3, Commission Delegated Regulation C(2026) 5010.

**Note on context-specificity (para 8):** where material IROs are connected to specific geographies, consider appropriate aggregation/disaggregation by site, basin, or area with water stress (ESRS 1 Ch. 3.3.2).

| DR Code | DR Name | Sub-topics (routing labels) | Disclosure Objective | Key Content |
|---------|---------|-----------|---------------------|-------------|
| E3-1 | Policies related to water | Water use | Per GDR-P (paras 10–11) | Water-related policies (para 10, references ESRS 2 GDR-P). If the undertaking has sites in areas with water stress not covered by its water policies, it shall disclose this fact (para 11). |
| E3-2 | Actions and resources related to water | Water use | Per GDR-A (paras 12–13) | Key water-related actions and resources (para 12, references ESRS 2 GDR-A). Specify key actions/resources related to areas with water stress (para 13). |
| E3-3 | Targets related to water | Water use | Per GDR-T (para 14) | Water-related targets (references ESRS 2 GDR-T). Where relevant, express targets with reference to areas with water stress (AR 3). |
| E3-4 | Water metrics | Water use (withdrawal, consumption, discharge, stored) | Enable understanding of the undertaking's water performance (paras 15–16) | For own operations: total water consumption; total water consumption in areas with water stress; total water withdrawal; total water discharge; total water recycled and reused; total water stored (para 16(a)–(f)). Consumption may be derived C = W − D (AR 4). Presented in m³ or multiples (AR 5). |

---

### E4 — Biodiversity and Ecosystems

**Full name:** ESRS E4 Biodiversity and Ecosystems
**Applicability:** When biodiversity and ecosystems relate to material impacts, risks or opportunities (E4 obj. para 2)
**Sub-topics (E4 obj. para 6):** Drivers of biodiversity and ecosystem change | State of species | Condition and extent of terrestrial, freshwater and marine ecosystems | Ecosystem services
**Adopted DR range:** E4-1 .. E4-5
**Phase-in:** for "Other undertakings" (Wave 2), all E4 DRs may be omitted for the **first two reporting years** (ESRS 1 para 127). See §1.3.
**Source:** Adopted ESRS E4, Commission Delegated Regulation C(2026) 5010.

**Architecture note:** E4-5 metrics are reported for material biodiversity/ecosystem-change IROs across whichever E4 sub-topic(s) are material (para 20, AR 10 menu), rather than as fixed per-sub-topic bundles.

| DR Code | DR Name | Sub-topics (routing labels) | Disclosure Objective | Key Content |
|---------|---------|-----------|---------------------|-------------|
| E4-1 | Biodiversity and ecosystems transition plan | All E4 sub-topics | Enable understanding of the undertaking's response/contribution to the Kunming-Montreal GBF transition, **if** it has a transition plan and has made its key features public (paras 10–11) | If the undertaking has a biodiversity and ecosystems transition plan and has made its key features public, disclose those features (para 11). Conditional DR — disclosure is triggered only where such a plan exists and is public (AR 2). Biodiversity may also form part of a broader (e.g. climate) transition plan (AR 1). |
| E4-2 | Policies related to biodiversity and ecosystems | All E4 sub-topics | Per GDR-P (paras 12–13) | Biodiversity/ecosystems policies (references ESRS 2 GDR-P). Describe policy content re: traceability of products/components/raw materials with material biodiversity impacts in the value chain; and sites in own operations in or near biodiversity-sensitive areas (para 13). |
| E4-3 | Actions and resources related to biodiversity and ecosystems | All E4 sub-topics | Per GDR-A (paras 14–15) | Key biodiversity/ecosystems actions and resources (references ESRS 2 GDR-A). Describe any biodiversity offsets used (aim, financing effects, area, type, quality criteria, standards) (para 15). |
| E4-4 | Targets related to biodiversity and ecosystems | All E4 sub-topics | Per GDR-T (paras 16–17) | Biodiversity/ecosystems targets (references ESRS 2 GDR-T). If offsets are used in setting targets, disclose how (para 17). Targets typically follow the mitigation hierarchy (AR 7). |
| E4-5 | Metrics related to biodiversity and ecosystems change | All E4 sub-topics | Enable understanding of performance against material biodiversity/ecosystem-change IROs (paras 18–20) | For material biodiversity/ecosystem-change IROs: locations in own operations to which they relate; list of biodiversity-sensitive area(s) (name and type) related to material negative impacts; the undertaking's activities related to those material negative impacts (para 19). Plus metrics per ESRS 2 GDR-M, drawn as relevant from: drivers of change; state of species; condition/extent of ecosystems; ecosystem services (para 20, AR 10). |

---

### E5 — Resource Use and Circular Economy

**Full name:** ESRS E5 Resource Use and Circular Economy
**Applicability:** When resource use or circular economy relates to material impacts, risks or opportunities (E5 obj. para 2)
**Sub-topics (E5 obj. para 6):** Resource inflows | Resource outflows related to products and services | Resource outflows related to waste
**Adopted DR range:** E5-1 .. E5-5
**Source:** Adopted ESRS E5, Commission Delegated Regulation C(2026) 5010.

| DR Code | DR Name | Sub-topics (routing labels) | Disclosure Objective | Key Content |
|---------|---------|-----------|---------------------|-------------|
| E5-1 | Policies related to resource use and circular economy | All E5 sub-topics | Per GDR-P (paras 8–9) | Resource-use/circular-economy policies (references ESRS 2 GDR-P). If the undertaking integrates circular-economy principles or eco-design in key products/services, explain how (para 9). |
| E5-2 | Actions and resources related to resource use and circular economy | All E5 sub-topics | Per GDR-A (para 10) | Key resource-use/circular-economy actions and resources. References ESRS 2 GDR-A. |
| E5-3 | Targets related to resource use and circular economy | All E5 sub-topics | Per GDR-T (para 11) | Resource-use/circular-economy targets. References ESRS 2 GDR-T. |
| E5-4 | Resource inflows | Resource inflows | Enable understanding of resource inflows (paras 12–13) | Key materials used, with concise description and any critical/strategic raw materials they contain (para 13(a)); total weight of all key materials (13(b)); breakdown of each key material by weight or % of total (13(c)); secondary resources used by weight or % (13(d)). |
| E5-5 | Resource outflows | Resource outflows (products & services), Resource outflows (waste) | Enable understanding of circular design of products/services and of waste management (paras 14–17) | **Products** (para 15): expected durability of key products; extent to which repairable; designed recyclability rate of key products and packaging. **Waste** (para 16): description of waste streams; total weight generated; proportion diverted from disposal (hazardous/non-hazardous, split reuse/recycling/other recovery); proportion directed to disposal (incineration/landfill/other); proportion with unknown destination. Total radioactive waste generated (para 17). |

---

### S1 — Own Workforce

**Full name:** ESRS S1 Own Workforce
**Applicability:** When own workforce relates to material impacts, risks or opportunities (S1 obj. para 2)
**Sub-topics (S1 obj. para 6):** Working conditions (incl. adequate wages, work-life balance, working time, secure employment) and social protection | Social dialogue and collective bargaining, freedom of association, information and consultation rights of workers, including through works councils | Health and safety | Training and skills development | Diversity and equal treatment | Other labour-related human rights (child labour, forced labour, privacy, adequate housing)
**Source:** Adopted ESRS S1, Commission Delegated Regulation C(2026) 5010.

**Coverage (para 7):** Own workforce = (i) employees (in an employment relationship) and (ii) "non-employees" — self-employed people who supply labour, or people provided by undertakings engaged in employment activities (e.g. agency/posted workers). Does not cover upstream/downstream value-chain workers (those are ESRS S2).

**Special rules (adopted S1 obj. para 1):**
- S1-5 (employee characteristics) applies if the undertaking concludes its **own workforce is material** — an obj. para 1(a) exception to the para-30 sub-topic routing; the para-23 information-materiality filter still applies.
- S1-6 (non-employee characteristics) applies if the undertaking concludes **non-employees in its own workforce are connected to material IROs** (obj. para 1(b); AR 12).
- Phase-in for "Other undertakings" (Wave 2) is keyed to the **first reporting year(s)** per ESRS 1 para 127 (e.g. S1-7 non-EEA datapoints, S1-10, S1-11, S1-12, S1-13 para 37(d)(e) and non-employee datapoints, S1-14 in the first year). See §1.3.

#### S1 Impact, Risk and Opportunity Management DRs

| DR Code | DR Name | Sub-topics (routing labels) | Disclosure Objective | Key Content |
|---------|---------|-----------|---------------------|-------------|
| S1-1 | Policies related to own workforce | All S1 sub-topics | Per GDR-P (paras 10–11) | Policies for managing material own-workforce IROs (references ESRS 2 GDR-P); whether they cover specific groups or all of own workforce (para 10). Whether policies address trafficking in human beings, forced/compulsory labour, and child labour (para 11). |
| S1-2 | Engagement with own workforce and workers' representatives, existence of channels for own workforce to raise concerns or needs and approaches to remedy | All S1 sub-topics | Enable understanding of engagement approach, channels and remedy (paras 12–15) | How the undertaking engages with own workforce/representatives and how their perspectives inform decisions (para 13, incl. insight into vulnerable/marginalised workers and any Global Framework Agreements); channels to raise concerns incl. whether a grievance mechanism exists, and effectiveness (para 14); general approach to remediation of material negative impacts (para 15). |
| S1-3 | Actions and resources related to own workforce | All S1 sub-topics | Per GDR-A (paras 16–17) | Key actions to prevent/mitigate/remediate material negative impacts on own workforce and resources allocated (references ESRS 2 GDR-A); how effectiveness of actions is tracked (para 17). |

#### S1 Metrics and Targets DRs

| DR Code | DR Name | Sub-topics (routing labels) | Disclosure Objective | Key Content |
|---------|---------|-----------|---------------------|-------------|
| S1-4 | Targets related to own workforce | All S1 sub-topics | Per GDR-T (para 18) | Qualitative and/or quantitative targets related to own workforce. References ESRS 2 GDR-T. |
| S1-5 | Characteristics of the undertaking's employees | Working conditions (applies when own workforce material — obj. para 1(a)) | Enable understanding of employment practices incl. security of employment (paras 19–20) | Total employees by headcount, broken down by gender; headcount for each country with ≥50 employees among the ten largest; permanent / temporary (by gender) and non-guaranteed-hours employees (headcount or FTE); permanent-employee turnover rate; explanation of any inconsistency with the financial statements (para 20). |
| S1-6 | Characteristics of non-employees in the undertaking's own workforce | Working conditions (applies when non-employees connected to material IROs — obj. para 1(b)) | Enable understanding of reliance on non-employees (paras 21–22) | Total number of non-employees in the undertaking's own workforce (para 22); headcount or FTE, period-end or average (AR 13). |
| S1-7 | Collective bargaining coverage and social dialogue | Social dialogue / collective bargaining | Enable understanding of collective-bargaining coverage and social dialogue (paras 23–25) | % of total employees covered by collective bargaining agreements; EEA country-level coverage for significant-employment countries; non-EEA coverage by region (para 24). Social dialogue: % of employees covered by workers' representatives at EEA country level; existence of EWC/SE/SCE works-council agreements (para 25). |
| S1-8 | Gender diversity in top management | Diversity and equal treatment | Enable understanding of gender diversity at top-management level (paras 26–27) | Gender distribution in number (headcount) and percentage at top-management level (para 27). "Top management" = two levels below the administrative/supervisory bodies, or the undertaking's own disclosed definition (AR 17). |
| S1-9 | Adequate wages | Working conditions | Enable understanding of whether employees are paid an adequate wage (paras 28–29) | Whether employees are paid an adequate wage against a benchmark; the benchmark(s) used and countries; if not all are paid an adequate wage, the countries and % of employees concerned (para 29). |
| S1-10 | Social protection | Working conditions | Enable understanding of social-protection coverage (paras 30–31) | If employees lack social protection (public or undertaking-provided), the countries where these major life events are not covered: sickness; unemployment; employment injury and acquired disability; maternity leave (para 31). Phase-in per ESRS 1 para 127 (first year for Wave 2) — see §1.3. |
| S1-11 | Persons with disabilities | Diversity and equal treatment | Enable understanding of inclusion of persons with disabilities (paras 32–33) | % of persons with disabilities among employees, subject to legal restrictions on data collection (para 33). Phase-in per ESRS 1 para 127 — see §1.3. |
| S1-12 | Training and skills development metrics | Training and skills development | Enable understanding of training and development (paras 34–35) | % of employees that participated in formalised performance and career development reviews; average number of training hours per employee (para 35). Phase-in per ESRS 1 para 127 — see §1.3. |
| S1-13 | Health and safety metrics | Health and safety | Enable understanding of OSH management coverage and performance (paras 36–37) | % of own workforce covered by the OSH management system; fatalities from work-related accidents/ill health; number and rate of recordable work-related accidents; recordable work-related ill-health cases; days lost (para 37). Certain datapoints (para 37(d)(e)) phase-in per ESRS 1 para 127 — see §1.3. |
| S1-14 | Work-life balance metrics | Working conditions | Enable understanding of entitlement to family-related leave (paras 38–39) | % of employees entitled to take family-related leave (maternity, paternity, parental, carers') during the reporting period (para 39). Phase-in per ESRS 1 para 127 — see §1.3. |
| S1-15 | Remuneration metrics | Diversity and equal treatment | Enable understanding of gender pay gap and remuneration inequality (paras 40–41) | Gender pay gap (difference in average pay between female and male employees, as % of male average) (para 41(a)); annual total remuneration ratio of the highest-paid individual to the median for all employees excluding that individual (para 41(b)). |
| S1-16 | Incidents of discrimination and other human rights incidents | Diversity and equal treatment; Other labour-related human rights | Enable understanding of discrimination and human-rights incidents affecting own workforce (paras 42–43) | For material sub-topics: number of substantiated incidents of discrimination incl. harassment (para 43(a)); number of substantiated human-rights incidents connected to own workforce, excluding those under 43(a) (para 43(b)); total fines, penalties and compensation for those incidents recognised in the financial statements (para 43(c)). |

---

### S2 — Workers in the Value Chain

**Full name:** ESRS S2 Workers in the Value Chain
**Applicability:** When workers in the value chain relate to material impacts, risks or opportunities (S2 obj. para 2)
**Sub-topics (S2 obj. para 6):** Working conditions (incl. adequate wages, work-life balance, working time, secure employment) and social protection | Social dialogue and collective bargaining, freedom of association, information and consultation rights of workers, including through works councils | Health and safety | Training and skills development | Diversity and equal treatment | Other labour-related human rights (child labour, forced labour, privacy, adequate housing, water and sanitation) — the same six as S1
**Phase-in:** for "Other undertakings" (Wave 2), all S2 DRs may be omitted for the **first two reporting years** (ESRS 1 para 127). See §1.3.
**Source:** Adopted ESRS S2, Commission Delegated Regulation C(2026) 5010.

**Coverage (paras 7–8):** all upstream/downstream value-chain workers who are or can be materially impacted by the undertaking, and who are not in its own workforce (e.g. supplier workers, outsourced on-site services, workers deeper in the supply chain).

**Architecture note:** S2 prescribes no standardised quantitative metrics DR. Entity-specific metrics are used via ESRS 2 GDR-M where relevant (ESRS 2 AR 42).

**Materiality-assessment depth (ESRS 1 Appendix A, S1/S2 note):** although S1 and S2 sub-topics are aligned, the *depth and granularity* of the materiality assessment is not expected to be the same for both. The level of detail depends on the type and quality of data available, which can lead to different levels of depth/granularity — especially for upstream/downstream impacts and risks — so the way the undertaking considers and assesses negative impacts and risks may legitimately differ between S1 and S2. S2 materiality write-ups are not expected to match S1's in rigor; a shallower value-chain assessment (e.g. relying on sector/regional data per ESRS 1 para 33) is expected and does not itself indicate an incomplete DMA.

| DR Code | DR Name | Sub-topics (routing labels) | Disclosure Objective | Key Content |
|---------|---------|-----------|---------------------|-------------|
| S2-1 | Policies related to workers in the value chain | All S2 sub-topics | Per GDR-P (paras 10–12) | Policies for managing material value-chain-worker IROs, and whether they cover specific groups or all (para 10); whether policies address trafficking, forced/compulsory labour and child labour (para 11); whether the undertaking has a supplier code of conduct (para 12). References ESRS 2 GDR-P. |
| S2-2 | Engagement with workers in the value chain, existence of channels for value chain workers to raise concerns or needs and approaches to remedy | All S2 sub-topics | Enable understanding of engagement approach, channels and remedy (paras 13–16) | How the undertaking engages with value-chain workers/representatives and how their perspectives inform decisions, incl. vulnerable/marginalised workers and any Global Framework Agreements (para 14); channels to raise concerns incl. whether a grievance mechanism exists (para 15); general approach to remediation (para 16). |
| S2-3 | Actions and resources related to workers in the value chain | All S2 sub-topics | Per GDR-A (paras 17–19) | Key actions to prevent/mitigate/remediate material negative impacts on value-chain workers and resources (references ESRS 2 GDR-A); how effectiveness is tracked (para 18); human-rights incidents connected to value-chain workers (para 19). |
| S2-4 | Targets related to workers in the value chain | All S2 sub-topics | Per GDR-T (para 20) | Qualitative or quantitative targets related to value-chain workers. References ESRS 2 GDR-T. |

---

### S3 — Affected Communities

**Full name:** ESRS S3 Affected Communities
**Applicability:** When affected communities relate to material impacts, risks or opportunities (S3 obj. para 2)
**Sub-topics (S3 obj. para 6):** Communities' economic, social and cultural rights (incl. land-related impacts, security-related impacts, adequate housing and food, water and sanitation) | Communities' civil and political rights (incl. freedom of expression, freedom of assembly, impacts on human rights defenders) | Rights of indigenous peoples (incl. free, prior and informed consent (FPIC), self-determination, cultural rights)
**Phase-in:** for "Other undertakings" (Wave 2), all S3 DRs may be omitted for the **first two reporting years** (ESRS 1 para 127). See §1.3.
**Source:** Adopted ESRS S3, Commission Delegated Regulation C(2026) 5010.

**Architecture note:** S3 prescribes no standardised quantitative metrics DR. Entity-specific metrics are used via ESRS 2 GDR-M where relevant.

| DR Code | DR Name | Sub-topics (routing labels) | Disclosure Objective | Key Content |
|---------|---------|-----------|---------------------|-------------|
| S3-1 | Policies related to affected communities | All S3 sub-topics | Per GDR-P (paras 8–9) | Policies for managing material affected-communities IROs, and whether they cover specific communities or all (para 8); any particular policy provisions for preventing/addressing impacts on indigenous peoples (para 9). References ESRS 2 GDR-P. |
| S3-2 | Engagement with affected communities, existence of channels for affected communities to raise concerns or needs and approaches to remedy | All S3 sub-topics | Enable understanding of engagement approach, channels and remedy (paras 10–14) | Whether/how the undertaking engages with affected communities and how their perspectives inform decisions, incl. vulnerable/marginalised groups (para 11); where communities are indigenous peoples, how their particular rights incl. FPIC are respected (para 12); channels to raise concerns incl. grievance mechanism (para 13); approach to remediation (para 14). |
| S3-3 | Actions and resources related to affected communities | All S3 sub-topics | Per GDR-A (paras 15–17) | Key actions to prevent/mitigate/remediate material negative impacts on communities and resources (references ESRS 2 GDR-A); how effectiveness is tracked (para 16); human-rights incidents connected to affected communities (para 17). |
| S3-4 | Targets related to affected communities | All S3 sub-topics | Per GDR-T (para 18) | Qualitative and/or quantitative targets related to affected communities. References ESRS 2 GDR-T. |

---

### S4 — Consumers and End-Users

**Full name:** ESRS S4 Consumers and End-users
**Applicability:** When consumers and/or end-users relate to material impacts, risks or opportunities (S4 obj. para 2)
**Sub-topics (S4 obj. para 6):** Information-related impacts (incl. privacy, access to information, freedom of expression) | Personal safety (incl. health and safety, protection of children, security of a person) | Social inclusion (incl. access to products and services, responsible marketing practices, non-discrimination)
**Scope note (para 7):** misuse or unlawful use of the undertaking's products/services by consumers/end-users is outside scope.
**Phase-in:** for "Other undertakings" (Wave 2), all S4 DRs may be omitted for the **first two reporting years** (ESRS 1 para 127). See §1.3.
**Source:** Adopted ESRS S4, Commission Delegated Regulation C(2026) 5010.

**Architecture note:** S4 prescribes no standardised quantitative metrics DR. Entity-specific metrics are used via ESRS 2 GDR-M where relevant.

| DR Code | DR Name | Sub-topics (routing labels) | Disclosure Objective | Key Content |
|---------|---------|-----------|---------------------|-------------|
| S4-1 | Policies related to consumers and end-users | All S4 sub-topics | Per GDR-P (para 9) | Policies for managing material consumer/end-user IROs; whether they cover specific groups (e.g. particular age groups) or all consumers/end-users (para 9). References ESRS 2 GDR-P. |
| S4-2 | Engagement with consumers and end-users, existence of channels for consumers and end-users to raise concerns or needs and approaches to remedy | All S4 sub-topics | Enable understanding of engagement approach, channels and remedy (paras 10–13) | How the undertaking engages with consumers/end-users and how their perspectives inform decisions, incl. vulnerable/marginalised groups (para 11); channels to raise concerns incl. grievance mechanism and its effectiveness (para 12); approach to remediation of material negative impacts (para 13). |
| S4-3 | Actions and resources related to consumers and end-users | All S4 sub-topics | Per GDR-A (paras 14–16) | Key actions to manage material positive/negative impacts and resources (references ESRS 2 GDR-A); for material negative impacts, actions to prevent/mitigate/remediate and how effectiveness is tracked (para 15); human-rights incidents connected to consumers/end-users (para 16). |
| S4-4 | Targets related to consumers and end-users | All S4 sub-topics | Per GDR-T (para 17) | Qualitative and/or quantitative targets related to consumers and end-users. References ESRS 2 GDR-T. |

---

### G1 — Business Conduct

**Full name:** ESRS G1 Business Conduct
**Applicability:** When business conduct relates to material impacts, risks or opportunities (G1 obj. para 2)
**Sub-topics (G1 obj. para 5):** Corporate culture (incl. anti-corruption and anti-bribery, protection of whistleblowers, animal welfare) | Management of relationships with suppliers (incl. payment practices, especially late payment to SMEs) | Political influence (incl. lobbying activities)
**Source:** Adopted ESRS G1, Commission Delegated Regulation C(2026) 5010.

| DR Code | DR Name | Sub-topics (routing labels) | Disclosure Objective | Key Content |
|---------|---------|-----------|---------------------|-------------|
| G1-1 | Policies related to business conduct | Corporate culture | Per GDR-P (paras 6–7) | Business-conduct policies (references ESRS 2 GDR-P). In addition: whether the undertaking has anti-corruption/anti-bribery policies consistent with the UN Convention against Corruption; whether it has whistleblower-protection policies; and the functions or roles most at risk of corruption or bribery (para 7). |
| G1-2 | Actions related to business conduct | Corporate culture; Management of relationships with suppliers | Per GDR-A (paras 8–9) | Business-conduct actions (references ESRS 2 GDR-A). In addition: management of supplier relationships incl. whether sustainability performance features in supplier selection, procurement-team sustainability training, and supplier engagement (para 9(a)); procedures to prevent/detect/investigate/respond to corruption or bribery incl. training of at-risk functions and actions on breaches (para 9(b)). |
| G1-3 | Targets related to business conduct | Corporate culture | Per GDR-T (para 10) | Business-conduct targets. References ESRS 2 GDR-T. |
| G1-4 | Metrics related to corruption or bribery | Corporate culture (anti-corruption/anti-bribery) | Enable transparency on convictions and sanctions related to corruption/bribery (paras 11–12) | Number of convictions and the total amount of fines for violation of anti-corruption and anti-bribery laws during the reporting period (para 12). Convictions/sanctions/fines defined per AR 5–6. |
| G1-5 | Metrics related to political influence, including lobbying activities | Political influence, including lobbying | Enable understanding of activities/commitments related to political influence (paras 13–16) | Total monetary value of financial and in-kind political contributions made directly and indirectly, disaggregated by country/geography and recipient type where relevant (para 14); main issues covered by lobbying and main positions taken, incl. how they interact with material IROs (para 15); appointment of any AMSB members during the reporting period who held a comparable public-administration/regulator position in the two preceding years (para 16). |
| G1-6 | Metrics related to payment practices | Management of relationships with suppliers (payment practices) | Enable understanding of standard payment terms and performance, esp. late payment to SMEs (paras 17–18) | Standard payment terms in number of days by main supplier category, specifying those applying to SMEs if different (para 18(a)); percentage of payments aligned with those standard terms (18(b)); number of legal proceedings currently outstanding for late payment (18(c)). |

---

## PART 4: TOPIC → DR MAPPING LOGIC (Topic-to-DR Cascade — Simplified ESRS)

### 4.1 Cascading approach

When an IRO is identified as material, the mapping follows this cascade:

**Step 1 — ESRS Standard:** Which topical standard(s) does this IRO map to? An IRO may map to multiple standards (primary and secondary).

**Step 2 — Sub-topic:** Within each triggered standard, which sub-topic(s) does this IRO relate to? Only the material sub-topic needs to be reported — if an IRO relates only to climate change mitigation, the undertaking need not report on adaptation or energy sub-topics unless separately material.

**Step 3 — DRs triggered:** For the triggered standard and sub-topic(s):
- Strategy DRs (if the IRO informs strategy or transition planning)
- PAT DRs (GDR-P, GDR-A, GDR-T via topical references — always apply to material topics)
- Metrics DRs (apply if the specific metric is material for understanding performance)
- Anticipated financial effects DR (if financial effects are material and quantifiable)

**Step 4 — ESRS 2 DRs always apply:** IRO-1 (DMA process), IRO-2 (list of material matters), SBM-3 (IRO interaction with strategy and financial effects) are always triggered alongside any material topical DR.

**Step 5 — Phase-in check:** Confirm whether any triggered DRs are subject to phase-in provisions given the undertaking's reporting timeline.

### 4.2 Sub-topic to DR cross-reference

> The adopted standards set DRs at the **standard level**; sub-topics are routed through ESRS 1 para 30, not partitioned into DR bundles. The tables below therefore show, for each standard, the **full adopted DR set** available once the standard is material, with sub-topics as routing labels. Which DRs are actually *in scope* for a given IRO is decided by the materiality assessment and the para-24 filter — not by picking a sub-topic row. Anticipated financial effects live in ESRS 2 SBM-3 (+ E1-11 for climate); there is no standalone AFE DR in E2/E3/E4/E5.

#### E1 Climate Change (DRs E1-1..E1-11)
| Sub-topic (routing label) | Full standard DR set (materiality-filtered) |
|-----------|---------------------|
| Climate change mitigation | E1-1, E1-2, E1-3, E1-4, E1-5, E1-6, E1-7, E1-8, E1-9, E1-10, E1-11 |
| Climate change adaptation | E1-2, E1-3, E1-4, E1-5, E1-11 (+ others where material) |
| Energy | E1-4, E1-5, E1-7 (+ others where material) |
| Any E1 sub-topic | ESRS 2: SBM-3, IRO-1, IRO-2, GDR-P, GDR-A, GDR-M, GDR-T (+ GOV-2 where applicable) |

#### E2 Pollution (DRs E2-1..E2-5)
| Sub-topic (routing label) | Full standard DR set (materiality-filtered) |
|-----------|---------------------|
| Pollution of air | E2-1, E2-2, E2-3, E2-4 |
| Pollution of water | E2-1, E2-2, E2-3, E2-4 |
| Pollution of soil | E2-1, E2-2, E2-3, E2-4 |
| Microplastics | E2-1, E2-2, E2-3, E2-4 (microplastics datapoints, para 16) |
| Substances of concern (incl. SVHC) | E2-1, E2-2, E2-3, E2-5 |

#### E3 Water (DRs E3-1..E3-4)
| Sub-topic (routing label) | Full standard DR set (materiality-filtered) |
|-----------|---------------------|
| Water use (withdrawal, consumption, discharge, stored) | E3-1, E3-2, E3-3, E3-4 |

#### E4 Biodiversity and Ecosystems (DRs E4-1..E4-5)
| Sub-topic (routing label) | Full standard DR set (materiality-filtered) |
|-----------|---------------------|
| Any E4 sub-topic (drivers of change / state of species / condition & extent of ecosystems / ecosystem services) | E4-1 (if transition plan exists & public), E4-2, E4-3, E4-4, E4-5 (metrics drawn from the para-20/AR-10 menu; not sub-topic-partitioned) |

#### E5 Resource Use and Circular Economy (DRs E5-1..E5-5)
| Sub-topic (routing label) | Full standard DR set (materiality-filtered) |
|-----------|---------------------|
| Resource inflows | E5-1, E5-2, E5-3, E5-4 |
| Resource outflows related to products and services | E5-1, E5-2, E5-3, E5-5 (products datapoints, para 15) |
| Resource outflows related to waste | E5-1, E5-2, E5-3, E5-5 (waste datapoints, paras 16–17) |

#### S1 Own Workforce (DRs S1-1..S1-16)
| Sub-topic (routing label) | Full standard DR set (materiality-filtered) |
|-----------|---------------------|
| Working conditions | S1-1..S1-4 (PAT), S1-5, S1-9, S1-10, S1-14 (+ S1-16 where discrimination/HR incidents material) |
| Social dialogue / collective bargaining | S1-1..S1-4, S1-7 |
| Health and safety | S1-1..S1-4, S1-13 |
| Training and skills development | S1-1..S1-4, S1-12 |
| Diversity and equal treatment | S1-1..S1-4, S1-8, S1-11, S1-15, S1-16 |
| Other labour-related human rights | S1-1..S1-4, S1-16 |
| Applies when own workforce material / non-employees material | S1-5 (own workforce material); S1-6 (non-employees connected to material IROs) |

*Note: S1 sub-topic→metric associations above are routing conveniences for the engine; the adopted standard does not gate these DRs on a single sub-topic. Confirm materiality per DR against the para-24 filter.*

#### S2 Workers in the Value Chain (DRs S2-1..S2-4)
| Sub-topic (routing label) | Full standard DR set (materiality-filtered) |
|-----------|---------------------|
| Any S2 sub-topic (six, same as S1) | S2-1, S2-2, S2-3, S2-4 (entity-specific metrics via GDR-M) |

#### S3 Affected Communities (DRs S3-1..S3-4)
| Sub-topic (routing label) | Full standard DR set (materiality-filtered) |
|-----------|---------------------|
| Any S3 sub-topic (economic/social/cultural rights; civil/political rights; indigenous peoples' rights) | S3-1, S3-2, S3-3, S3-4 (entity-specific metrics via GDR-M) |

#### S4 Consumers and End-users (DRs S4-1..S4-4)
| Sub-topic (routing label) | Full standard DR set (materiality-filtered) |
|-----------|---------------------|
| Any S4 sub-topic (information-related impacts; personal safety; social inclusion) | S4-1, S4-2, S4-3, S4-4 (entity-specific metrics via GDR-M) |

#### G1 Business Conduct (DRs G1-1..G1-6)
| Sub-topic (routing label) | Full standard DR set (materiality-filtered) |
|-----------|---------------------|
| Corporate culture (incl. anti-corruption/anti-bribery, whistleblowers, animal welfare) | G1-1, G1-2, G1-3, G1-4 |
| Management of relationships with suppliers (incl. payment practices) | G1-1, G1-2, G1-3, G1-6 |
| Political influence, including lobbying | G1-1, G1-2, G1-3, G1-5 |

---

## PART 5: IRO TYPE → ESRS STANDARD ROUTING RULES

These rules are used to auto-match an IRO to candidate ESRS standards before human review. Rules are additive — an IRO may match multiple standards.

### 5.1 Keyword routing rules

| If IRO topic/description contains... | Route to standard(s) |
|--------------------------------------|---------------------|
| climate, GHG, emissions, carbon, net zero, decarbonisation, transition risk, physical risk, extreme weather, flooding, drought, sea level, temperature | E1 |
| supply chain disruption + climate/weather | E1 (primary), S2 (secondary if workforce impacts) |
| pollution, contamination, toxic, hazardous substance, chemical, PFAS, microplastic | E2 |
| water, water stress, water scarcity, drought, marine, ocean, aquatic | E3 |
| biodiversity, ecosystem, habitat, species, land use, deforestation, nature | E4 |
| waste, circular economy, resource efficiency, packaging, recycling, materials | E5 |
| employees, workforce, workers, labour, health and safety, pay equity, gender pay, diversity, DEI, belonging, talent, remuneration, collective bargaining, working conditions | S1 |
| supply chain workers, value chain workers, supplier labour, contractor workers, forced labour, child labour, CSDDD, human rights due diligence in supply chain | S2 |
| communities, affected communities, indigenous peoples, land rights, community rights, water and sanitation access, security impacts on communities | S3 |
| consumers, customers, end users, data privacy, personal data, product safety, consumer rights, information-related impacts | S4 |
| governance, business conduct, corruption, bribery, anti-corruption, whistleblower, political lobbying, payment practices, supplier relationships, responsible product, export compliance | G1 |
| AI regulations, ESG regulations, regulatory compliance | G1 (governance/conduct primary) |
| data breach, cybersecurity | S4 (consumer data impact primary), G1 (governance secondary) |

### 5.2 IRO type routing modifiers

| IRO Type | Routing modifier |
|----------|-----------------|
| Impact | Prioritise social and environmental standards (E1–E5, S1–S4) where impact is on people or environment; ESRS 2 SBM-3 always triggered |
| Risk (financial) | Prioritise anticipated-financial-effects disclosure in **ESRS 2 SBM-3** (paras 25–27) for all topics, plus **E1-11** for climate specifically (the only topical AFE DR in the adopted text — E2/E3/E4/E5 have none); governance DRs (GOV-4 for risk management) |
| Opportunity | Prioritise strategy DRs (SBM-1, SBM-3); transition plan DRs if climate-related (E1-1); financial effects DRs |

### 5.3 Entity-specific IRO handling

When an IRO does not map cleanly to any topic in ESRS 1 Appendix A (i.e., the topic is not covered, or not covered with sufficient granularity, by an ESRS — ESRS 1 para 11), the following apply. This branch is also triggered where a *mapped* IRO's topical DR lacks sufficient granularity — sufficiency is a fair-presentation test, not just an "unmapped" test.
- ESRS 1 paragraph 11: The undertaking shall provide entity-specific disclosures
- ESRS 2 BP-1: The list of entity-specific disclosures must be documented in the basis for preparation
- ESRS 2 IRO-2: Entity-specific material matters must be listed alongside standard-based DRs
- GDR-P, GDR-A, GDR-M, GDR-T still apply — policies, actions, metrics and targets should be described for entity-specific material topics using the same narrative GDR framework
- Examples from this DMA: "Emerging Tech & AI" is not covered by any standard at the topic level. G1 provides partial coverage for governance of AI conduct risks, but environmental/social impacts of AI products are entity-specific territory. "Regulatory Compliance" (broad) maps partially to G1 but may have entity-specific dimensions requiring additional disclosure.

### 5.4 Conditional DRs

- **E1-9** (GHG removals/carbon credits) is only triggered if the undertaking uses carbon credits or GHG removal projects; otherwise not applicable.
- **E1-10** (internal carbon pricing) is only triggered if the undertaking has implemented an internal carbon pricing mechanism; otherwise not applicable.

---

## PART 6: RATIONALE TEMPLATES

These templates are used to pre-populate the rationale field for each IRO × DR combination. They are starting points — the sustainability specialist should edit to reflect the specific facts of the undertaking.

### 6.1 Template structure

**Format:** "[IRO type] relating to [IRO topic/sub-topic]. This [DR code] is triggered because [reason based on DR objective and IRO content]. Under the simplified ESRS, this DR requires [brief description of key content]. [Phase-in note if applicable.]"

### 6.2 E1 rationale templates by DR

| DR | IRO Type | Template |
|----|----------|----------|
| E1-1 | Risk/Opportunity | "Material climate-related [risk/opportunity] identified. E1-1 is triggered to enable disclosure of the undertaking's transition plan for climate change mitigation, including GHG reduction targets, decarbonisation levers, and key actions. If no transition plan is in place, the undertaking shall disclose this fact." |
| E1-2 | Risk | "Material climate-related risk identified. E1-2 is triggered to disclose, for each material climate-related risk, whether it is a physical or transition risk and the key elements of the methodology used to assess exposure and sensitivity over the short/medium/long term. Where climate-related scenario analysis is used, the scenario ranges, scope, assumptions and time period are disclosed. E1-2 is materiality-conditional — it is not always-mandatory." |
| E1-3 | Risk | "Material climate-related physical risk identified. E1-3 is triggered to enable disclosure of the undertaking's resilience analysis, including results of scenario analysis and how current and planned actions contribute to resilience." |
| E1-4 | Any | "E1-4 is triggered via GDR-P for this material climate-related [topic]. The undertaking shall describe its climate change mitigation and adaptation policies under the GDR-P narrative framework." |
| E1-5 | Any | "E1-5 is triggered via GDR-A for this material climate-related [topic]. The undertaking shall describe key actions and resources allocated to climate change mitigation and adaptation." |
| E1-6 | Any | "E1-6 is triggered for this material climate-related topic. The undertaking shall disclose GHG emission reduction targets, including whether targets are science-based." |
| E1-7 | Any | "E1-7 is triggered for this material climate-related topic. The undertaking shall disclose total energy consumption in MWh disaggregated into fossil, nuclear and renewable sources, with further fossil disaggregation for high climate impact sectors, and self-generated energy production if applicable." |
| E1-8 | Risk/Impact | "E1-8 is triggered for this material climate-related topic. The undertaking shall disclose gross Scope 1, 2 (location- and market-based) and 3 GHG emissions. The reporting boundary is financial control per the GHG Protocol (2004), with equity share or operational control permitted as alternatives (adopted E1 AR 19); ESRS 1 paras 71/72 and AR 36 prevail." |
| E1-11 | Risk/Opportunity | "Material climate-related [risk/opportunity] with financial implications identified. E1-11 is triggered to enable disclosure of anticipated financial effects from physical/transition risks and opportunities. Phase-in (Other undertakings/Wave 2): AFE may be omitted for the first two reporting years and quantitative AFE for the first four, except paras 39(a)(b) and 40(a)(b) (ESRS 1 para 127)." |

### 6.3 S1 rationale templates by DR

| DR | IRO Type | Template |
|----|----------|----------|
| S1-1 | Any | "Material own workforce [topic] identified. S1-1 is triggered via GDR-P. The undertaking shall describe its human rights and workforce policies, including whether they address forced/compulsory labour and child labour." |
| S1-2 | Impact | "Material negative impact on own workforce identified. S1-2 is triggered to disclose engagement processes, channels for workers to raise concerns, and grievance mechanisms." |
| S1-3 | Impact/Risk | "S1-3 is triggered via GDR-A. The undertaking shall describe key actions to prevent, mitigate and remediate material negative impacts on its own workforce." |
| S1-5 | Any | "S1-5 applies whenever the own workforce topic is assessed as material. The undertaking shall disclose employee characteristics: total headcount by gender; headcount for each country with 50+ employees among its ten largest; permanent/temporary/non-guaranteed-hours breakdowns; and permanent-employee turnover." |
| S1-13 | Any | "Health and safety material impact/risk identified. S1-13 is triggered to disclose OSH management-system coverage, recordable work-related accidents (number and rate), fatalities, ill-health cases and days lost. Phase-in for certain datapoints per ESRS 1 para 127 (first reporting year for Wave 2)." |
| S1-15 | Any | "Remuneration equity identified as material. S1-15 is triggered to disclose the gender pay gap and the ratio of the highest-paid individual's annual total remuneration to the median for all other employees." |

### 6.4 G1 rationale templates by DR

| DR | IRO Type | Template |
|----|----------|----------|
| G1-1 | Risk | "Material business conduct risk identified. G1-1 (Policies related to business conduct) is triggered via GDR-P to disclose business conduct policies, including whether the undertaking has anti-corruption/anti-bribery policies consistent with the UN Convention against Corruption, whistleblower-protection policies, and the functions/roles most at risk of corruption or bribery." |
| G1-2 | Risk | "Material business conduct risk identified. G1-2 (Actions related to business conduct) is triggered via GDR-A to disclose actions on supplier relationships (incl. sustainability in supplier selection, procurement training, supplier engagement) and procedures to prevent, detect, investigate and respond to corruption or bribery." |
| G1-3 | Risk | "Material business conduct risk identified. G1-3 (Targets related to business conduct) is triggered via GDR-T to disclose the undertaking's business-conduct targets." |
| G1-4 | Risk | "Material corruption/bribery risk identified. G1-4 (Metrics related to corruption or bribery) is triggered to disclose the number of convictions and the total amount of fines for violation of anti-corruption and anti-bribery laws during the reporting period." |
| G1-5 | Risk | "Material political-influence risk identified. G1-5 (Metrics related to political influence, including lobbying) is triggered to disclose the total monetary value of political contributions (direct and indirect), the main issues and positions of lobbying activities, and any revolving-door AMSB appointments from the preceding two years." |
| G1-6 | Risk | "Material supplier payment-practices risk identified. G1-6 (Metrics related to payment practices) is triggered to disclose standard payment terms in days by main supplier category (specifying SMEs), the percentage of payments aligned with those terms, and the number of outstanding late-payment legal proceedings." |

---

*Source (cross-cutting): Commission Delegated Regulation C(2026) 5010, adopted 3 July 2026 — ESRS 1 & ESRS 2.*
*Source (topical): ESRS E1–E5, S1–S4, G1, Commission Delegated Regulation C(2026) 5010. DR codes, names, sub-topics, objectives and content sourced to adopted paragraph/AR numbers cited throughout.*
