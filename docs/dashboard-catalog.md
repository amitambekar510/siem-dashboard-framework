# Dashboard Catalog (D-01 to D-14)  ·  v2.0

Vendor-neutral reference. Field names follow ECS; AI telemetry uses the proposed `ai.*` namespace ([schema](ai-telemetry-schema.md)). Translate queries with [siem-translation.md](siem-translation.md).

**14 dashboards · 101 panel designs.** D-13 and D-14 are new in v2.0.

| Ref | Dashboard | Audience | Purpose | Key panels | Data sources | Range / Refresh | Priority |
|---|---|---|---|---|---|---|---|
| D-01 | SOC Alert Triage / Overview | L1/L2 Analysts | Real-time triage of the day's alerts | Open alerts by severity; Alerts over time; Top rules triggered; Top hosts and users; Alert-status funnel; MTTD/MTTR tiles; Critical-events single-window feed; Risk score gauge | Detection alerts index | Last 24h / 30s | P1 |
| D-02 | Ingest & Source Health | SOC Eng / Platform | Confirm every source is alive and flowing | Events per source over time; Last-seen per source (silence detection); EPS by source; Parse-failure count; Dropped-event trend; Timestamp skew (event vs ingest); Expected vs reporting sources (coverage %) | Monitoring + per-source indices | Last 24h / 1m | P1 |
| D-03 | Cluster Health / Infrastructure | Platform Admin | Keep the stack healthy | Node status; JVM heap; CPU / memory; Disk watermark; Unassigned shards; Indexing and search rate; Pending tasks | Stack monitoring | Last 24h / 30s | P1 |
| D-04 | Authentication & Identity | Analysts | Spot identity-based attacks | Failed logons; Brute-force patterns; Impossible travel; Privileged and new-admin logons; Account lockouts; MFA failures; Shared / unattributed accounts; After-hours privileged logons | AD/DC, VPN, cloud IdP | Last 24h / 1m | P1 |
| D-05 | Network Security | Analysts | Perimeter and network visibility | Denied traffic; Top talkers; Port-scan detection; IDS/IPS signature hits; GeoIP map of denied connections; DNS anomalies; Egress volume; Beaconing detector | Firewall, IDS/IPS, proxy, DNS | Last 24h / 1m | P1 |
| D-06 | Endpoint / EDR | Analysts | Host-level threat visibility | Malware detections; Suspicious process / parent-child; Persistence events; Removable-media use; Endpoint agent health; LOLBAS process panel | EDR, Sysmon/Winlogbeat | Last 24h / 1m | P2 |
| D-07 | MITRE ATT&CK Coverage | SOC Lead / Detection Eng | See detection coverage and gaps | ATT&CK matrix heatmap; Detections by tactic; Coverage-gap list; Rule-to-technique mapping; Coverage % per tactic | Detection alerts + rule metadata | Last 30d / on demand | P2 |
| D-08 | Threat Intelligence | Analysts | Track IOC matches | Indicator matches over time; Matched IOC types; Feed freshness; Top matched indicators; Match source | Threat-intel index + enriched events | Last 7d / 5m | P2 |
| D-09 | Detection Rule Performance | Detection Eng | Tune the ruleset | Rules by fire volume; Noisy rules (FP%); Zero-hit / stale rules; Execution errors; Avg rule duration; Alert-to-case conversion rate | Rule execution / monitoring | Last 7d / 15m | P2 |
| D-10 | Data Retention & ILM | Platform Admin | Storage governance | Index size by data stream; ILM phase distribution; Days-of-retention vs policy; Disk-growth forecast; Snapshot status | ILM / monitoring | Last 7d / 1h | P2 |
| D-11 | Compliance & Audit | Compliance | Evidence for regulators across all 8 frameworks | Log completeness by mandated source; Retention conformance; Clock-sync status; Access-to-logs audit; PII-field access; Assured-zero tile: audit log cleared (value + window + last event + reporting vs expected + verdict); Framework score gauges (x8) | Multiple + audit index | Last 30d / 1h | P2 |
| D-12 | Executive / Client Report | Client | Business-level posture | Incident summary; Alert trend; SLA attainment; Top risks; Coverage posture; Period-over-period comparison; Gap-closure trend; AI risk summary | Aggregated | Last 30d / scheduled | P3 |
| D-13 **NEW** | AI Security & Threat Monitoring | SOC / AI Security Eng | Detect attacks on AI systems (ISO/IEC 27090 areas) | AI requests and guardrail actions over time; Prompt-injection and jailbreak attempts by client; Model-extraction suspects (volume + prompt diversity); Sensitive-output detections (PII / secrets); Training-data store writes by identity; Model artifact changes vs approved pipeline; AI agent tool-call denials; Quota and rate-limit hits; AI risk score by system; 27090 telemetry-coverage gauge; Assured-zero tiles for must-be-0 AI checks | AI gateway / LLM proxy, model registry, object-store audit, MLOps CI/CD, vector DB audit | Last 24h / 1m | P2 |
| D-14 **NEW** | AI Governance & Lifecycle | AI Governance / CISO / Compliance | Evidence for AI management (ISO/IEC 42001) | AI system inventory (registered vs discovered); Shadow AI discovery (unsanctioned services); AI systems by risk tier; Usage by owner and department; Third-party model/provider dependency map; Lifecycle stage events with approval status; Deployed vs approved model version; Acceptable-use violations; AI log completeness (silence detection); Corrective-action and gap-closure trend | AI gateway, proxy/DNS, model registry, CI/CD, inventory | Last 30d / 1h | P2 |

## Build order

1. P1 first: D-01 to D-05. They answer "is something wrong right now?"
2. Build **D-02 (source health)** early. A silent source defeats every other dashboard.
3. D-11 and D-12 once the data is trustworthy.
4. D-13 / D-14 once your AI gateway or proxy logs reach the SIEM. See [ai-telemetry-schema.md](ai-telemetry-schema.md).
5. Apply the [assured-zero pattern](assured-zero-pattern.md) to **every** must-be-0 tile before you show it to an auditor.
