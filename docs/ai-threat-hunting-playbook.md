# AI Threat Hunting Playbook (ISO/IEC 27090 aligned)

Hunt the **footprint** of the attack, because the attack itself is rarely visible.

| # | Hunt | Signal | Query idea | Dashboard |
|---|---|---|---|---|
| 1 | Prompt injection / jailbreak | Guardrail blocks and flags per client | `ai.guardrail.category: ("prompt_injection" OR "jailbreak")` | D-13 |
| 2 | Model extraction | High volume + high unique-prompt ratio per key | Transform `ai-client-usage`, then ratio > threshold | D-13 |
| 3 | Data poisoning footprint | Dataset write or delete outside approved pipeline | `ai.data.operation: ("write" OR "delete") AND NOT ai.pipeline.id: *` | D-13 |
| 4 | Model tampering | Artifact modified or promoted outside pipeline, hash change | `ai.artifact.operation: ("modify" OR "promote") AND NOT ai.pipeline.id: *` | D-13 |
| 5 | Sensitive output | PII or secrets in model output | `ai.guardrail.category: ("pii" OR "secrets")` | D-13 |
| 6 | Agent misuse | Denied tool calls, unusual tool sequences | `ai.tool.outcome: "denied"` | D-13 |
| 7 | Shadow AI | Traffic to unsanctioned AI services | `ai.shadow.detected: true` | D-14 |
| 8 | Silent AI system | No logs from a registered system | Source-silence tile | D-02, D-14 |

## Correlation that works

- **Injection attempts followed by sensitive output from the same client** within 15 minutes. Treat as P0.
- **Dataset write + model promote** by the same identity with no pipeline id. Treat as P0.
- **Extraction pattern + quota hits** from one key. Block and review.

## Be honest about limits

Evasion, membership inference and model inversion are hard to detect from logs. Track query-pattern anomalies, and say in your report that specialist ML tooling is needed for confirmation.

## Zero needs proof

"No prompt-injection attempts" means nothing if the AI gateway was silent. Pair every must-be-0 AI tile with a canary (any request event from the same system) and an expected-systems count. See [assured-zero-pattern.md](assured-zero-pattern.md).
