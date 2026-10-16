# Translating the queries

| Intent | Elastic KQL | Splunk SPL | Sentinel KQL |
|---|---|---|---|
| Audit log cleared | `winlog.event_id: 1102` | `index=wineventlog EventCode=1102` | `SecurityEvent \| where EventID == 1102` |
| Failed logons | `winlog.event_id: 4625` | `EventCode=4625` | `SecurityEvent \| where EventID == 4625` |
| New service | `winlog.event_id: 7045` | `EventCode=7045` | `Event \| where EventID == 7045` |
| Group change | `winlog.event_id: (4732 OR 4756)` | `EventCode IN (4732,4756)` | `SecurityEvent \| where EventID in (4732,4756)` |

Map ECS fields to your SIEM's schema (Splunk CIM, Sentinel ASIM, QRadar custom properties, Chronicle UDM).
