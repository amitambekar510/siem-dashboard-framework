# Framework & Regulation Coverage  ·  v2.0

## Count

**8 frameworks and regulations are mapped in v2.0** (6 in v1.0 + 2 new).

| # | Framework / regulation | Type | Scope | Region | Added |
|---|---|---|---|---|---|
| 1 | **ISO/IEC 27001:2022** | International standard | Information security management system | Global | v1.0 |
| 2 | **ISO/IEC 27090:2026** | International standard (guidance) | Security threats and compromises to AI systems | Global | v2.0 (new) |
| 3 | **ISO/IEC 42001:2023** | International standard | AI management system | Global | v2.0 (new) |
| 4 | **PCI DSS v4.0.1** | Industry standard | Payment card data security | Global | v1.0 |
| 5 | **NIST CSF 2.0** | Framework | Cybersecurity risk management (Govern, Identify, Protect, Detect, Respond, Recover) | Global / US | v1.0 |
| 6 | **CIS Controls v8.1** | Framework | Prioritised defensive safeguards (18 controls) | Global | v1.0 |
| 7 | **GDPR** | Regulation (law) | Personal data protection | EU / EEA | v1.0 |
| 8 | **SEBI CSCRF** | Regulatory framework | Cyber resilience for SEBI-regulated entities | India | v1.0 |

Breakdown: **3** ISO/IEC standards · **3** industry standards / frameworks (PCI DSS, NIST CSF, CIS Controls) · **2** regulatory (GDPR, SEBI CSCRF).

Used as references but **not counted** as compliance frameworks: MITRE ATT&CK (threat-mapping reference, not a compliance framework); Elastic Common Schema (field naming reference).

Absence-based evidence (for example "audit log cleared = 0") is counted only when it passes the [assured-zero](assured-zero-pattern.md) check.

## Two kinds of numbers: read this before quoting a score

| Framework group | What the percentage means |
|---|---|
| ISO 27001, PCI DSS, NIST CSF, CIS, GDPR, SEBI CSCRF | **Example scorecard** from one real engagement (weighted control compliance). Your numbers will differ. |
| ISO/IEC 27090 and ISO/IEC 42001 | **Coverage by design**: how much of the standard's guidance a SIEM can *evidence* given the right telemetry. This is **not** an audit result. |

### Coverage-by-design method

Each area is rated for what a SIEM can prove from logs, then scored:

| Rating | Meaning | Weight |
|---|---|---|
| Strong | Directly evidenced by SIEM telemetry | 1.0 |
| Partial | Evidenced in part, or needs an extra feed | 0.5 |
| Limited | Weak signals only; needs specialist tooling | 0.25 |
| Not SIEM-evidenceable | Document, assessment or process activity | 0.0 |

| Standard | Areas rated | Strong | Partial | Limited | Not evidenceable | Coverage by design |
|---|---|---|---|---|---|---|
| ISO/IEC 27090:2026 | 21 | 7 | 9 | 4 | 1 | **60%** |
| ISO/IEC 42001:2023 | 16 | 2 | 8 | 3 | 3 | **42%** |

Per-area detail: [ISO 27090](iso-27090-ai-security.md) · [ISO 42001](iso-42001-ai-management.md).

> **Independence notice.** This project is not affiliated with or endorsed by ISO, IEC, PCI SSC, NIST, CIS, the EU or SEBI. It contains **no text from ISO/IEC standards**. Topic names come from ISO's public overview pages, and all mappings are the author's own paraphrase for orientation. Clause-level conformance work requires the licensed standards, which you can buy from [iso.org](https://www.iso.org/).
