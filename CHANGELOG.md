# Changelog

## [2.0.0] - 2026-10-20

### Added
- **ISO/IEC 27090:2026** (security threats to AI systems): 21-area SIEM coverage map, hunting playbook, queries, Sigma rules.
- **ISO/IEC 42001:2023** (AI management system): 16-item SIEM coverage map and audit-evidence guide.
- **D-13 AI Security & Threat Monitoring** and **D-14 AI Governance & Lifecycle** dashboards (14 dashboards, 99 panel designs in total).
- Proposed vendor-neutral `ai.*` telemetry schema (27 fields), component template, gateway ingest pipeline with data minimisation, shadow-AI pipeline, client-usage transform, AI risk-score runtime field.
- 5 portable **Sigma** rules for AI detections (`sigma/`).
- New visuals: animated hero, AI attack-chain GIF, 14-dashboard tour, D-13 / D-14 demos, 27090 / 42001 coverage maps, social-preview image.
- `LICENSE` (MIT) and this changelog.
- Excel workbook v2: framework list, 27090 / 42001 coverage sheets, AI field schema, AI hunt signals.

### Changed
- Framework count **6 to 8**.
- PCI DSS label updated to **v4.0.1** (v4.0 was retired) and CIS Controls to **v8.1**.
- Hunt query, gap analysis, translation table and compliance queries extended with AI signals.
- D-11 now shows 8 framework gauges; D-12 gains an AI risk summary.

### Notes
- ISO/IEC 27090 and 42001 percentages are **coverage by design**, not audit results.
- No ISO text is reproduced. Mappings are the author's own.

## [1.0.0] - 2026-10-16
- Initial release: 12 dashboards, 6 frameworks, threat-hunting window, risk-score model, demo visuals.
