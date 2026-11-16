# Threat Intelligence Reports

Threat actor profiles and weekly intelligence summaries from
**[Medina Telemetry Systems](https://github.com/Medina-Telemetry-Systems)**. Each report is written for defenders who need to
decide what to do: it opens with the bottom line, separates sourced facts from our assessments, and cites every claim.

The companion repository, **[detections-public](https://github.com/Medina-Telemetry-Systems/detections-public)**, holds the
detection rules and vulnerability advisories these reports refer to, plus an
[ATT&CK coverage map](https://github.com/Medina-Telemetry-Systems/detections-public/blob/main/coverage/README.md).

## Reports

### Threat actor profiles
| Actor | Type | Updated |
|:--|:--|:--|
| [INC Ransom](actors/inc-ransom.md) | Ransomware-as-a-Service | 2026-11-16 |
| [The Gentlemen](actors/the-gentlemen.md) | Ransomware-as-a-Service | 2026-11-16 |

### Weekly summaries
| Week | Highlights |
|:--|:--|
| [9–15 November 2026](weekly/2026-W46.md) | 12 KEV additions, 8 with three-day deadlines; 201 ransomware leak-site postings; INC Ransom most active |

## How these reports are written
- **Bottom line first.** Every report opens with what matters and who is affected.
- **Facts and judgments are kept apart.** Sourced facts name their source. Our own assessments are labeled as
  assessments and carry a confidence level.
- **Every claim is checked against the original source,** such as government advisories, vendor incident reports, the CVE
  record or the CISA KEV catalog. Claims we can't confirm are left out.
- **ATT&CK technique IDs** follow the current [MITRE ATT&CK](https://attack.mitre.org/) Enterprise matrix.

### Confidence levels
| Level | Meaning |
|:--|:--|
| **High** | Based on several independent, reliable sources that agree, or on direct evidence. Could still be wrong. |
| **Moderate** | Credible sourcing that is incomplete, or supports more than one explanation. |
| **Low** | Thin, indirect or single-source evidence. A lead worth tracking, not a conclusion. |

### Source types
| Type | Examples | How we use it |
|:--|:--|:--|
| Government | CISA, ACSC, NCSC advisories; the KEV catalog | Primary source for incidents, exploitation status and mitigations |
| Security vendor | Incident-response and threat-research reports | Primary source for tooling and recent techniques; we name the vendor |
| Reference | MITRE ATT&CK, NVD | Technique IDs, aliases, vulnerability details |
| Leak-site trackers | ransomware.live | Victim counts and trends. These are criminals' own claims, so we label them as such |

## License
Reports are licensed under [Creative Commons Attribution 4.0 (CC BY 4.0)](LICENSE.md). You may share and adapt them,
including commercially, with attribution to Medina Telemetry Systems and a link to the source.

Provided as-is for defensive use, without warranty.
