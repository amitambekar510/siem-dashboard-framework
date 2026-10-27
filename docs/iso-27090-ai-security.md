# ISO/IEC 27090: Security for AI Systems, Mapped to a SIEM

> **Independence notice.** This project is not affiliated with or endorsed by ISO, IEC, PCI SSC, NIST, CIS, the EU or SEBI. It contains **no text from ISO/IEC standards**. Topic names come from ISO's public overview pages, and all mappings are the author's own paraphrase for orientation. Clause-level conformance work requires the licensed standards, which you can buy from [iso.org](https://www.iso.org/).

## Status

| Item | Value |
|---|---|
| Title | Cybersecurity, Artificial intelligence (AI): Addressing security threats and compromises to AI systems |
| Edition / date | Edition 1, **published October 2026** (stage 60.60, 2026-10-14 per ISO's page) |
| Committee | ISO/IEC JTC 1/SC 27 |
| Nature | **Guidance**, not certifiable requirements |

> Earlier planning notes treated 27090 as a future change. ISO's catalogue page now lists it as **published**, so v2.0 treats it as current guidance.

## What it is about (author's summary)

- It helps organisations understand AI-specific attacks, their consequences, and how to detect and reduce them across the AI life cycle.
- It **complements** ISO/IEC 27001 and 27002 and does not replace them. Conventional security still applies.
- It focuses on **protecting AI systems**, not on using AI to defend other systems.
- ISO's own overview notes that detecting some AI threats remains difficult, so the project does not claim full SIEM coverage.

## Coverage map: 21 areas, **60%** coverage by design

| Group | Area | SIEM evidence | Telemetry needed | Detection / panel idea |
|---|---|---|---|---|
| Applying information security to AI | AI governance | **Partial** | AI system inventory, policy-violation events | Alert when an AI system without a registered owner generates traffic |
| Applying information security to AI | Zero trust principles | **Strong** | IdP, service-to-service auth, AI gateway auth logs | Alert on model API calls using shared or long-lived keys |
| Applying information security to AI | Software engineering practices | **Partial** | CI/CD and MLOps pipeline logs | Alert on pipeline runs without required approval stage |
| Applying information security to AI | Supply-chain security | **Partial** | Model registry pulls, package and artifact audit logs | Alert on model pulled from unapproved source |
| Applying information security to AI | Threat modelling | **Not SIEM-evidenceable** | None (design activity) | Track outputs in a manual register panel |
| Applying information security to AI | Red teaming | **Partial** | Tagged red-team events, detection test results | Report which red-team techniques were detected |
| Understanding AI-specific threats | Data poisoning | **Partial** | Training-data store writes, dataset change events | Alert on dataset write by identity outside the approved pipeline |
| Understanding AI-specific threats | Model exfiltration | **Strong** | AI gateway request logs per client and API key | High query volume plus high prompt diversity per key |
| Understanding AI-specific threats | Evasion attacks | **Limited** | Guardrail flags, anomaly scores | Trend of flagged inputs; true detection needs ML-side tooling |
| Understanding AI-specific threats | Membership inference | **Limited** | Query patterns against sensitive models | Repeated near-duplicate queries from one client |
| Understanding AI-specific threats | Model inversion | **Limited** | Query patterns and confidence-score access | Systematic probing of one output class |
| Understanding AI-specific threats | Direct model poisoning | **Partial** | Model artifact change events, hash mismatch | Alert on model artifact modified outside deployment pipeline |
| Understanding AI-specific threats | Prompt injection | **Strong** | Guardrail / gateway logs | Spike in blocked or flagged injection attempts per client |
| Understanding AI-specific threats | Sensitive model output | **Strong** | Output guardrail and DLP detections | Any PII or secret detected in model output |
| Understanding AI-specific threats | Outputs containing injection attacks | **Partial** | Output filtering events | Output flagged as containing script or command content |
| Maintaining protection over time | Logging and monitoring | **Strong** | All AI telemetry, source-health silence detection | Alert when an AI system stops sending logs |
| Maintaining protection over time | Protecting development environments | **Strong** | Auditd, IAM and notebook / workspace access logs | Alert on privileged access to ML workspaces outside change window |
| Maintaining protection over time | Detecting malicious inputs | **Partial** | Guardrail scores and block events | Trend of high-score inputs by client and source IP |
| Maintaining protection over time | Limiting model use | **Strong** | Rate-limit, quota and token-usage events | Quota-exceeded events and token spikes per client |
| Maintaining protection over time | Model and mitigation deterioration | **Limited** | Needs ML observability feed | Ingest drift metrics if available; otherwise manual note |
| Maintaining protection over time | Provenance and history of AI systems and data | **Partial** | Model registry lineage, dataset version events | Alert on deployed model version not in approved list |

Strong **7** · Partial **9** · Limited **4** · Not evidenceable **1**

## What this means in practice

1. **A SIEM is strongest at the "maintaining protection" layer**: logging, monitoring, access, rate limiting, input and output guardrail events.
2. **Poisoning and model theft are detectable by their *operational footprint***: who wrote to the dataset, who changed the artifact, who queried 40,000 times with 38,000 unique prompts. The attack itself is not visible, but its footprint is.
3. **Evasion, inversion and membership inference are weak SIEM signals.** Say so in your reports. Honest limits build auditor trust.
4. **Nothing here works without AI telemetry.** Start with an AI gateway or proxy log and the [`ai.*` schema](ai-telemetry-schema.md).

## Dashboards and assets

| Need | Where |
|---|---|
| Dashboard | **D-13 AI Security & Threat Monitoring** |
| Queries | [`kql/ai-security.kql`](../kql/ai-security.kql) |
| Portable detections | [`sigma/`](../sigma/) |
| Pipelines, template, transform, risk score | [`scripts/`](../scripts/) |
| Hunt steps | [`ai-threat-hunting-playbook.md`](ai-threat-hunting-playbook.md) |
