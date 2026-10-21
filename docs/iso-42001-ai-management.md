# ISO/IEC 42001: AI Management System, Mapped to a SIEM

> **Independence notice.** This project is not affiliated with or endorsed by ISO, IEC, PCI SSC, NIST, CIS, the EU or SEBI. It contains **no text from ISO/IEC standards**. Topic names come from ISO's public overview pages, and all mappings are the author's own paraphrase for orientation. Clause-level conformance work requires the licensed standards, which you can buy from [iso.org](https://www.iso.org/).

## Status

| Item | Value |
|---|---|
| Title | Information technology, Artificial intelligence, Management system |
| Edition / date | ISO/IEC 42001:**2023**, Edition 1, December 2023 |
| Nature | **Certifiable requirements** for an AI management system (AIMS). Certification is voluntary and performed by independent certification bodies, not by ISO. |
| Relationship | 42001 manages AI across the organisation. 27090 covers security threats to AI systems. Security is part of responsible AI management but not all of it. |

## The honest position

**Most of ISO/IEC 42001 is governance**: policy, roles, impact assessment, objectives, management review. A SIEM cannot prove those. A SIEM *can* provide the **operational evidence** that the management system is working: monitoring, logging, access control, usage control, lifecycle events, incident detection.

## Coverage map: 16 items, **42%** coverage by design

| Group | Area | SIEM evidence | Telemetry needed | Detection / panel idea |
|---|---|---|---|---|
| Clauses | 4  Context of the organization | **Partial** | AI system inventory and scope | Registered vs discovered AI systems |
| Clauses | 5  Leadership and AI policy | **Limited** | Policy-violation events only | Acceptable-use violations trend; policy itself is a document |
| Clauses | 6  Planning: AI risks, objectives | **Partial** | AI risk score per system | AI risk score by system and tier; risk register stays external |
| Clauses | 7  Support: resources, competence, documented information | **Limited** | Log retention conformance | Retention conformance for AI logs; training evidence is external |
| Clauses | 8  Operation: lifecycle and data controls | **Partial** | Pipeline, registry and dataset events | Lifecycle stage events with approval status |
| Clauses | 9  Performance evaluation: monitoring and measurement | **Strong** | All AI telemetry, KPIs | AI monitoring KPIs and audit-ready exports |
| Clauses | 10 Improvement: corrective action | **Partial** | Repeat-detection patterns, gap closure | Gap-closure trend; corrective actions kept in manual register |
| Annex A control areas | Policies related to AI | **Not SIEM-evidenceable** | None (documents) | Reference panel linking to policy documents |
| Annex A control areas | Internal organization (roles, reporting) | **Limited** | Owner field on AI inventory | Systems without an accountable owner |
| Annex A control areas | Resources for AI systems | **Partial** | Compute, data and tooling inventory | GPU / compute usage by AI system |
| Annex A control areas | Assessing impacts of AI systems | **Not SIEM-evidenceable** | None (assessment activity) | Manual register panel |
| Annex A control areas | AI system life cycle | **Partial** | Train / evaluate / deploy / retire events | Deployed model vs approved version |
| Annex A control areas | Data for AI systems | **Partial** | Dataset access, write and delete events | Dataset access by identity and purpose |
| Annex A control areas | Information for interested parties | **Not SIEM-evidenceable** | None (communication) | Not SIEM-evidenceable |
| Annex A control areas | Use of AI systems | **Strong** | AI gateway and proxy logs | Usage by team, acceptable-use blocks, shadow AI discovery |
| Annex A control areas | Third-party and customer relationships | **Partial** | External model/provider calls | Provider dependency map and unapproved providers |

Strong **2** · Partial **8** · Limited **3** · Not evidenceable **3**

> Clause numbers 4 to 10 follow the common ISO management-system structure. Annex A control-area names are paraphrased for orientation. Confirm against your licensed copy.

## Evidence you can export from D-14

| Auditor question | Evidence from the SIEM |
|---|---|
| "Which AI systems do you run or use?" | Registered vs discovered AI systems |
| "Who is accountable for each?" | Systems without an owner (target: 0) |
| "Is unapproved AI use happening?" | Shadow-AI discovery panel |
| "Is monitoring continuous?" | AI log-completeness and silence detection |
| "Are lifecycle changes controlled?" | Deploy / promote events with pipeline identity |
| "Are you improving?" | Gap-closure and repeat-detection trend |

What stays outside the SIEM: AI policy, impact assessments, roles and training records, management-review minutes. Keep them in a governance register and link them from a Markdown panel.
