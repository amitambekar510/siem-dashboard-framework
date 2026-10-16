# Honest Gap Analysis

A SIEM can only evidence what logs contain. Document the rest.

| Gap | Needs | Interim control |
|---|---|---|
| File integrity monitoring | FIM agent (Wazuh, Tripwire) | Formal risk acceptance, selective FIM on critical hosts |
| Consent / DSR tracking | Consent platform, ITSM | Manual register |
| PII data-flow | DLP / classification | Tag known PII paths with auditd keys |
| Vulnerability data | Scanner feed (Nessus, OpenVAS) | Weekly CSV import |
| Backup / DR evidence | Backup tool feed | Manual DR test note panel |
| Training metrics | LMS feed | Manual register |

**Tip:** a panel that reads "must be 0 and currently is 0" is valid evidence.
