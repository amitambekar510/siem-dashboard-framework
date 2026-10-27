# Translating the queries

| Intent | Elastic KQL | Splunk SPL | Sentinel KQL |
|---|---|---|---|
| Audit log cleared | `winlog.event_id: 1102` | `index=wineventlog EventCode=1102` | `SecurityEvent \| where EventID == 1102` |
| Failed logons | `winlog.event_id: 4625` | `EventCode=4625` | `SecurityEvent \| where EventID == 4625` |
| New service | `winlog.event_id: 7045` | `EventCode=7045` | `Event \| where EventID == 7045` |
| Group change | `winlog.event_id: (4732 OR 4756)` | `EventCode IN (4732,4756)` | `SecurityEvent \| where EventID in (4732,4756)` |
| Prompt injection flagged | `ai.guardrail.category: ("prompt_injection" OR "jailbreak")` | `index=ai ai.guardrail.category IN ("prompt_injection","jailbreak")` | `AIGateway_CL \| where guardrail_category_s in ("prompt_injection","jailbreak")` |
| Model changed outside pipeline | `ai.artifact.operation: ("modify" OR "promote") AND NOT ai.pipeline.id: *` | `index=ai ai.artifact.operation IN ("modify","promote") NOT ai.pipeline.id=*` | `AIRegistry_CL \| where operation_s in ("modify","promote") and isempty(pipeline_id_s)` |
| Shadow AI | `ai.shadow.detected: true` | `index=ai ai.shadow.detected=true` | `AIProxy_CL \| where shadow_detected_b == true` |

Map ECS fields to your SIEM's schema (Splunk CIM, Sentinel ASIM, QRadar custom properties, Chronicle UDM). The `ai.*` fields are custom everywhere, so ingest them into your own table or index. For portable detections use the Sigma rules in [`sigma/`](../sigma/).
