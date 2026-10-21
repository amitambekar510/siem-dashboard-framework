# AI Telemetry Schema (`ai.*`)  ·  proposed, vendor-neutral

ECS has no AI-specific fields. This is a **proposed** namespace so D-13 / D-14 work across SIEMs. Align field names with OpenTelemetry GenAI conventions or your vendor's schema where they overlap.

| Field | Type | Meaning | Used by |
|---|---|---|---|
| `ai.system.name` | keyword | Application / AI system name | D-13, D-14 |
| `ai.system.id` | keyword | Stable system identifier from inventory | D-14 |
| `ai.system.owner` | keyword | Accountable owner (team or person) | D-14 |
| `ai.system.risk_tier` | keyword | Your organisation's risk tier (e.g. high / limited / minimal) | D-13, D-14 |
| `ai.model.name` | keyword | Model name | D-13, D-14 |
| `ai.model.version` | keyword | Deployed model version | D-14 |
| `ai.model.provider` | keyword | Provider or 'self-hosted' | D-14 |
| `ai.request.id` | keyword | Request correlation id | D-13 |
| `ai.request.client_id` | keyword | API key or service identity (hashed) | D-13 |
| `ai.request.type` | keyword | inference | embedding | finetune | training | admin | D-13 |
| `ai.request.tokens_in` | long | Input tokens | D-13 |
| `ai.request.tokens_out` | long | Output tokens | D-13 |
| `ai.request.prompt_hash` | keyword | Hash of normalised prompt (never log raw prompts by default) | D-13 |
| `ai.guardrail.action` | keyword | allow | block | redact | flag | D-13, D-14 |
| `ai.guardrail.category` | keyword | prompt_injection | jailbreak | pii | secrets | output_injection | policy | D-13 |
| `ai.guardrail.score` | float | Detector confidence 0-1 | D-13 |
| `ai.data.dataset_id` | keyword | Dataset identifier | D-13, D-14 |
| `ai.data.operation` | keyword | read | write | delete | label | D-13 |
| `ai.artifact.type` | keyword | model | dataset | prompt_template | embedding_index | D-13 |
| `ai.artifact.operation` | keyword | push | pull | modify | delete | promote | D-13, D-14 |
| `ai.artifact.hash` | keyword | Content hash for integrity checks | D-13 |
| `ai.pipeline.id` | keyword | Approved pipeline identity (absent = manual change) | D-13, D-14 |
| `ai.pipeline.stage` | keyword | train | evaluate | deploy | retire | D-14 |
| `ai.tool.name` | keyword | Agent tool invoked | D-13 |
| `ai.tool.outcome` | keyword | allowed | denied | error | D-13 |
| `ai.shadow.detected` | boolean | True when traffic goes to an unsanctioned AI service | D-14 |
| `ai.shadow.service` | keyword | Matched unsanctioned service domain | D-14 |

## Where the data comes from

| Source | Gives you |
|---|---|
| AI gateway / LLM proxy | Requests, tokens, client identity, guardrail actions |
| Guardrail / DLP layer | Injection, jailbreak, PII and secret detections on inputs and outputs |
| Model registry | Artifact push / pull / modify / promote, hashes, lineage |
| Object store / data lake audit | Training-data reads, writes, deletes |
| MLOps CI/CD | Pipeline runs, approvals, stage changes |
| Vector DB / feature store audit | Index changes, bulk reads |
| Web proxy / DNS | Shadow-AI discovery |
| Host auditd / Windows logs | GPU and workspace host activity |

## Privacy by design (important for GDPR)

- **Do not log raw prompts or completions by default.** Log a salted hash (`ai.request.prompt_hash`) and token counts.
- Hash API keys before they reach the index (`ai.request.client_id`).
- If you must retain content for investigations, restrict it to a separate index with short retention and tight access.
- Retention for AI logs should follow your ILM policy (D-10).

Load the template with `PUT _component_template/ai-fields` (body = the file contents), then reference it from your index template.

Implementation files: [`scripts/index-template-ai-fields.json`](../scripts/index-template-ai-fields.json), [`scripts/ingest-pipeline-ai-gateway.json`](../scripts/ingest-pipeline-ai-gateway.json).
