# Assured Zero: make every "must be 0" tile prove its zero

> Origin: community feedback on the v2.0 release pointed out that a bare `0` is ambiguous. This pattern is the fix.

## The problem

A tile reading **0** can mean two opposite things:

| What the tile shows | Possible meaning | Conclusion |
|---|---|---|
| `0` | Nothing happened | Good: integrity evidence |
| `0` | Nothing *arrived* (agent stopped, source silent, window too short, pipeline lag) | Unknown: you are blind |

An analyst or auditor cannot tell them apart from a bare number. Worse, an attacker who clears logs often stops the forwarder too, so **silence can be the attack**.

## The pattern: five things beside every must-be-0 tile

| # | Context item | Why it matters |
|---|---|---|
| 1 | **Query window** (pinned on the tile, e.g. "Last 24h") | Screenshots lose the dashboard time picker |
| 2 | **Last event received** from the same source (canary) | Proves the pipe is alive, not just quiet |
| 3 | **Reporting vs expected sources** (e.g. 41 / 41) | Shows no host is missing from the count |
| 4 | **What was counted** (the query, in the tile description) | Auditors can reproduce it |
| 5 | **Verdict** | One word, so nobody has to interpret the numbers |

## Verdict logic

| Condition | Verdict | Colour |
|---|---|---|
| Target count > 0 | **ALERT** (regardless of coverage) | Red |
| Target count = 0 **and** last event is fresh **and** reporting sources = expected sources | **ASSURED ZERO** | Green |
| Target count = 0 **and** any of: stale canary, silent expected source, no inventory defined | **UNVERIFIED** (zero may mean no data) | Amber |

Default thresholds (tune them): freshness 15 minutes, coverage 100% of expected sources.

## Pick a canary for every check

The canary is an event that **should always be present** if collection works.

| Check (must be 0) | Target query | Canary | Expected sources |
|---|---|---|---|
| Windows Security log cleared | `winlog.event_id: 1102` | Any event with `winlog.channel: "Security"` from the host | All in-scope Windows hosts |
| Windows System log cleared | `winlog.event_id: 104` (System channel; verify in your environment) | Any `winlog.channel: "System"` event | Same |
| Linux audit tampering | Audit service stop or rule-change records | Any auditd record from the host | All in-scope Linux hosts |
| Privileged group change (expected rare) | `winlog.event_id: (4732 OR 4756)` | Any Security-channel event | Domain controllers and in-scope servers |
| Prompt-injection attempts | `ai.guardrail.category: ("prompt_injection" OR "jailbreak")` | Any request event from the same AI system | Each registered AI system |
| Model changed outside pipeline | `ai.artifact.operation: ("modify" OR "promote") AND NOT ai.pipeline.id: *` | Any registry audit event (even a pull) | Model registry |
| Dataset written outside pipeline | `ai.data.operation: ("write" OR "delete") AND NOT ai.pipeline.id: *` | Any object-store audit event | Training-data buckets |

Low-volume hosts may not emit a canary regularly. Give them a scheduled heartbeat event.

## Implementation

### Tier 1: no new infrastructure (10 minutes per tile)

Build the tile as a Lens table or metric group. Formulas (check the syntax your Kibana version supports):

```text
Target count     count(kql='winlog.event_id : 1102')
Canary count     count(kql='winlog.channel : "Security"')
Reporting hosts  unique_count(host.name, kql='winlog.channel : "Security"')
Last event       max(@timestamp, kql='winlog.channel : "Security"')
Verdict (0/1/2)  ifelse(count(kql='winlog.event_id : 1102') > 0, 2,
                   ifelse(unique_count(host.name, kql='winlog.channel : "Security"') < 41, 1, 0))
```

- Replace `41` with your expected host count. This is a stop-gap: a hard-coded number goes stale.
- Colour the verdict: 0 green, 1 amber, 2 red.
- Pin the window with the panel's **custom time range** so it shows on the tile and in screenshots.
- If your Kibana version supports `now()` in formulas, `now() - max(@timestamp, ...)` gives the age of the last event. Otherwise show the timestamp itself.

### Tier 2: recommended: inventory + transform (silent hosts become visible)

A silent host produces **no documents**, so it vanishes from any count. Fix that by indexing an *expected sources* inventory and letting a transform join it with the telemetry:

1. Index one document per expected source, refreshed daily: [`scripts/expected-sources-sample.ndjson`](../scripts/expected-sources-sample.ndjson).
2. Create the transform: [`scripts/transform-assured-zero.json`](../scripts/transform-assured-zero.json). It groups by host and day and counts `expected`, `canary_events` and `clear_events`.
3. Build the tile on the `assured-zero-clear-events` index: a host with `expected = 1` and `canary_events = 0` is **UNVERIFIED** and appears as a row instead of disappearing.
4. Verify the exact output field names with `GET assured-zero-clear-events/_mapping` before building Lens formulas.

Keep host names normalised (same case and format) in the inventory and the telemetry.

### Tier 3: alert on the gap itself

Alert when an expected source reports nothing for N minutes, and when a tile stays UNVERIFIED longer than your tolerance. A silent source is a detection, not just a data-quality issue.

## Wording auditors accept

> "Between 2026-10-01 and 2026-10-27, **0** Event 1102 across **41 of 41** in-scope Windows hosts. Most recent Security-channel event was received 12 seconds before this report. No expected source was silent for more than 15 minutes."

Compare with "0 audit logs cleared", which an auditor should reject.

## Anti-patterns

- A bare `0` or a green tile by default.
- A zero over a window shorter than the retention or reporting delay.
- Counting only the hosts that reported, then calling it complete.
- Screenshots of "No results found" as evidence.
- Ignoring ingest lag: compare `event.ingested` with `@timestamp` (see D-02).

## Where it applies in this project

D-02 (expected vs reporting sources), D-11 (audit-log-cleared tile), D-13 (must-be-0 AI checks), and any tile that says "must be 0" or "target 0".
