# Threat Hunting Playbook: single-window approach

## Principle
An analyst should answer "is something wrong right now?" without leaving one screen.

## Hunt order
1. **Integrity**: was evidence tampered with? (`winlog.event_id: 1102`, auditd rule changes). A zero here counts only as an [assured zero](assured-zero-pattern.md).
2. **Privilege**: who has more power than yesterday? (`EUID 0`, 4732/4756)
3. **Persistence**: what will survive reboot? (7045, 4698, new accounts)
4. **Credentials**: who is guessing or reusing? (4625, auth failures, shared IDs)
5. **Data**: what sensitive paths were touched? (`auditd.log.key: DPIA_*`)
6. **Network**: who is talking out? (beaconing, DNS anomalies, egress)
7. **AI systems (new in v2.0)**: who is attacking or misusing AI? See [ai-threat-hunting-playbook.md](ai-threat-hunting-playbook.md)

## Time-saving tips
- Pin the single-window query as a saved search on D-01.
- Sort by `risk.score` descending instead of by time.
- Tag every hunt with an ATT&CK technique so findings feed D-07.
- Convert repeatable hunts into detection rules, then measure them in D-09.

## Single hunt query
```kql
(auditd.log.EUID: "0" AND auditd.log.success: "no")
OR winlog.event_id: (1102 OR 4732 OR 4756 OR 7045 OR 4698)
OR (event.outcome: "failure" AND rule.name: *)
OR auditd.log.AUID: "4294967295"
OR auditd.log.key: DPIA_*
OR (ai.artifact.operation: ("modify" OR "promote") AND NOT ai.pipeline.id: *)
OR ai.guardrail.category: ("pii" OR "secrets")
```
