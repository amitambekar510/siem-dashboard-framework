# Dashboard Catalog (D-01 to D-12)

Vendor-neutral reference. Field names follow ECS; translate to your SIEM using `siem-translation.md`.

| Ref | Dashboard | Audience | Purpose | Key panels | Data sources | Range / Refresh | Priority |
|---|---|---|---|---|---|---|---|
| D-01 | SOC Alert Triage / Overview | L1/L2 Analysts | Real-time triage of the day's alerts | Open alerts by severity; alerts over time; top rules; top hosts and users; alert-status funnel; MTTD/MTTR tiles | Detection alerts index | 24h / 30s | P1 |
| D-02 | Ingest & Source Health | SOC Eng / Platform | Confirm every source is alive | Events per source; last-seen (silence detection); EPS by source; parse failures; dropped events | Monitoring + per-source indices | 24h / 1m | P1 |
| D-03 | Cluster Health / Infrastructure | Platform Admin | Keep the stack healthy | Node status; JVM heap; CPU/mem; disk watermark; unassigned shards; indexing and search rate; pending tasks | Stack monitoring | 24h / 30s | P1 |
| D-04 | Authentication & Identity | Analysts | Spot identity-based attacks | Failed logons; brute force; impossible travel; privileged and new-admin logons; lockouts; MFA failures | AD/DC, VPN, cloud IdP | 24h / 1m | P1 |
| D-05 | Network Security | Analysts | Perimeter and network visibility | Denied traffic; top talkers; port scans; IDS/IPS hits; GeoIP map; DNS anomalies; egress volume | Firewall, IDS/IPS, proxy, DNS | 24h / 1m | P1 |
| D-06 | Endpoint / EDR | Analysts | Host-level threat visibility | Malware detections; suspicious parent-child; persistence; removable media; agent health | EDR, Sysmon/Winlogbeat | 24h / 1m | P2 |
| D-07 | MITRE ATT&CK Coverage | SOC Lead / Detection Eng | Detection coverage and gaps | ATT&CK heatmap; detections by tactic; coverage gaps; rule-to-technique map | Detection alerts + rule metadata | 30d / on demand | P2 |
| D-08 | Threat Intelligence | Analysts | Track IOC matches | Matches over time; IOC types; feed freshness; top indicators; match source | Threat-intel index | 7d / 5m | P2 |
| D-09 | Detection Rule Performance | Detection Eng | Tune the ruleset | Rules by volume; noisy rules (FP%); zero-hit/stale rules; execution errors; avg duration | Rule execution / monitoring | 7d / 15m | P2 |
| D-10 | Data Retention & ILM | Platform Admin | Storage governance | Index size by stream; ILM phases; retention vs policy; disk-growth forecast; snapshot status | ILM / monitoring | 7d / 1h | P2 |
| D-11 | Compliance & Audit | Compliance | Evidence for regulators | Log completeness; retention conformance; clock-sync; access-to-logs audit; PII-field access; framework scores | Multiple + audit index | 30d / 1h | P2 |
| D-12 | Executive / Client Report | Client | Business-level posture | Incident summary; alert trend; SLA attainment; top risks; coverage posture; period comparison | Aggregated | 30d / scheduled | P3 |

## Panels recommended in addition to the original list

| Dashboard | Added panel | Why |
|---|---|---|
| D-01 | Critical Events Single-Window feed | Removes pivoting during hunts |
| D-01 | Risk score gauge + top-10 risky hosts | Instant prioritisation |
| D-02 | Source-silence alert tile (no data > N min) | Blind spots are the worst failure |
| D-02 | Timestamp-skew panel (event vs ingest time) | Detects clock drift and lag |
| D-04 | Shared-account / unattributed-ID panel | PCI Req 8, ISO A.9 |
| D-04 | After-hours privileged logons | Common attacker pattern |
| D-05 | Beaconing detector (regular interval outbound) | C2 detection |
| D-06 | LOLBAS / living-off-the-land process panel | Evades AV |
| D-07 | Technique coverage % per tactic | Tells the lead where to invest |
| D-09 | Alert-to-case conversion rate | Measures rule quality |
| D-11 | Audit-log-cleared counter (must be 0) | Evidence by absence |
| D-11 | Framework gauges (ISO, PCI, GDPR, NIST, CIS, SEBI) | One-glance audit view |
| D-12 | Gap-closure trend | Shows posture improving over time |
