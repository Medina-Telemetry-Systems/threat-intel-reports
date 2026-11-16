# INC Ransom

| | |
|:--|:--|
| **Also known as** | GOLD IONIC, Tarnished Scorpion; MITRE ATT&CK [G1032](https://attack.mitre.org/groups/G1032/) |
| **Type** | Ransomware-as-a-Service (RaaS) with double extortion |
| **Motivation** | Financial |
| **Active since** | Mid-2023 |
| **Main targets** | Health care, legal and professional services, manufacturing, technology, construction; mostly the United States, increasingly Australia, New Zealand and the Pacific |
| **Profile date** | 16 November 2026 |

## Bottom line
INC Ransom is one of the most active ransomware operations of 2026. Its affiliates usually get in with stolen credentials
or by exploiting unpatched internet-facing devices (VPNs, Citrix NetScaler, Fortinet, SimpleHelp). They then move through
the network using ordinary admin tools, steal data with rclone, and encrypt Windows and VMware ESXi systems. Health care is a
recurring target. Organizations with exposed edge devices, VPNs without multi-factor authentication (MFA), or Veeam backup
servers reachable from user networks are most at risk.

## Key judgments
1. **INC Ransom will very likely remain one of the most active ransomware operations through early 2027** (high confidence).
   Acronis ranks it in the top five groups of 2026 with more than 800 claimed victims since 2023, and it posted more victims
   than any other group in our own collection this week (see [Activity this week](#activity-this-week)).
2. **Compromised credentials and unpatched edge devices will remain its main ways in** (high confidence). The Australian,
   New Zealand and Tonga governments' joint advisory and Acronis's incident reporting independently describe the same entry
   methods.
3. **Health care organizations face elevated risk** (moderate confidence). The joint advisory describes a trend toward
   disproportionately targeting health care providers worldwide and documents health-sector incidents in all three
   countries. Acronis's 2026 data puts legal services first, so health care is one of several priority sectors rather than
   the only one.
4. **Defenses built for INC will probably also help against Lynx and Sinobi** (moderate confidence). Acronis reports that
   INC's source code was offered for sale in May 2024 and that both families show significant code overlap with it.
5. **The operators are probably based in the Commonwealth of Independent States (CIS)** (low confidence). This rests only on
   Acronis's observation that no CIS victims appear on the leak site, a common pattern for Russian-speaking ransomware
   groups but not proof of location.

## Background
- **Emergence and model.** INC Ransom appeared in mid-2023 as a RaaS operation. Affiliates carry out the intrusions and
  deploy the ransomware; INC handles extortion and payments. Victims face double extortion: encrypted systems plus the threat
  of publishing stolen data on INC's Tor leak site.
- **Ecosystem.** Acronis reports that a forum user listed INC's Windows and Linux/ESXi code for sale in May 2024, after which
  Lynx and later Sinobi appeared with significant code overlap. It also reports that Vice Society deployed INC ransomware
  against health care in late 2024, and that affiliates moved to INC after the LockBit disruption and the BlackCat shutdown.
  The joint advisory notes similar tactics in Lynx and in the older Nemty, Nemty X, Karma and Nokoyawa operations.
- **Tooling in 2026.** Acronis reports that both encryptors have been rewritten in Rust, and that a recent intrusion used a
  modified credential dumper that can decrypt credentials from newer Veeam backup installations.

## Victims
- **Scale:** more than 800 claimed victims since 2023 (Acronis, June 2026, using ransomware.live data). These are leak-site
  claims, not confirmed incidents.
- **Geography:** about 65% of listed victims are in the United States, followed by Australia, Canada, Germany and Taiwan
  (Acronis). The joint advisory says the group has been increasingly active against Australia, New Zealand and the Pacific
  since early 2025.
- **Sectors:** legal services, manufacturing, technology, health care and construction lead in 2026 (Acronis). Education was
  an earlier focus.
- **Government-confirmed incidents** (joint advisory):
  - **Australia:** 11 INC-related incidents reported to the Australian Cyber Security Centre (ACSC) between July 2024 and
    December 2025, mostly professional services and health care, some with theft of medical records.
  - **Tonga:** the Ministry of Health was hit on 15 June 2025, disrupting the national health network. INC claimed it on 26 June.
  - **New Zealand:** a health-sector organization was encrypted and had data stolen in May 2025, and INC published the data.

## How an attack unfolds
Techniques reported in the joint advisory (incidents from July 2024 to December 2025), the Acronis report, and the MITRE
ATT&CK group page. Each row names the source.

| Stage | Technique | What has been reported | Source |
|:--|:--|:--|:--|
| Initial access | [T1078](https://attack.mitre.org/techniques/T1078/) Valid Accounts, [T1133](https://attack.mitre.org/techniques/T1133/) External Remote Services | Logging in to VPNs without MFA with stolen or purchased credentials, including legacy and service accounts | Joint advisory |
| Initial access | [T1190](https://attack.mitre.org/techniques/T1190/) Exploit Public-Facing Application | Citrix NetScaler (CVE-2023-3519, CVE-2025-5777), Fortinet FortiClient EMS (CVE-2023-48788), SimpleHelp (CVE-2024-57727); edge devices such as routers | Acronis; joint advisory |
| Initial access | [T1566](https://attack.mitre.org/techniques/T1566/) Phishing | Spear-phishing | Joint advisory; Acronis |
| Persistence and privilege escalation | [T1136](https://attack.mitre.org/techniques/T1136/) Create Account, [T1098](https://attack.mitre.org/techniques/T1098/) Account Manipulation | Creating new administrator accounts | Joint advisory |
| Privilege escalation | [T1068](https://attack.mitre.org/techniques/T1068/) Exploitation for Privilege Escalation | Bring-your-own-vulnerable-driver (BYOVD) attacks | Joint advisory |
| Defense evasion | [T1685](https://attack.mitre.org/techniques/T1685/) Disable or Modify Tools | Killing security tools with PsKill or a custom process terminator that installs vulnerable drivers (`filwfp.sys`, `filnk.sys`, `fildds.sys`) | Acronis |
| Defense evasion | [T1036](https://attack.mitre.org/techniques/T1036/) Masquerading | Renaming tools to look legitimate; a PsExec copy named `winupd`; payloads named `win.exe` | Joint advisory; MITRE |
| Credential access | [T1003](https://attack.mitre.org/techniques/T1003/) OS Credential Dumping | Dumping cached credentials and NTLM hashes; a PowerShell script that decrypts credentials stored by Veeam Backup | Joint advisory; Acronis |
| Discovery | [T1046](https://attack.mitre.org/techniques/T1046/) Network Service Discovery, [T1135](https://attack.mitre.org/techniques/T1135/) Network Share Discovery | NetScan, Advanced IP Scanner, Angry IP Scanner, `net` and `ping` | Joint advisory; Acronis |
| Lateral movement | [T1021.001](https://attack.mitre.org/techniques/T1021/001/) Remote Desktop Protocol, [T1570](https://attack.mitre.org/techniques/T1570/) Lateral Tool Transfer | RDP and PsExec | Acronis; MITRE |
| Command and control | [T1219](https://attack.mitre.org/techniques/T1219/) Remote Access Tools | AnyDesk, ScreenConnect, TeamViewer, Cobalt Strike | Acronis |
| Collection | [T1074](https://attack.mitre.org/techniques/T1074/) Data Staged, [T1560](https://attack.mitre.org/techniques/T1560/) Archive Collected Data | Staging data and packing it into password-protected 7-Zip or WinRAR archives | Joint advisory; Acronis |
| Exfiltration | [T1567](https://attack.mitre.org/techniques/T1567/) Exfiltration Over Web Service, [T1537](https://attack.mitre.org/techniques/T1537/) Transfer Data to Cloud Account | rclone to attacker-controlled cloud storage; MegaSync in earlier cases | Acronis; joint advisory; MITRE |
| Impact | [T1486](https://attack.mitre.org/techniques/T1486/) Data Encrypted for Impact | Rust encryptors for Windows and ESXi with partial, multithreaded encryption; `INC-README.txt` and `.html` ransom notes; ransom notes sent to network printers | Acronis |

## Detection and hunting ideas
These behaviors are specific enough to hunt for. They are leads, not tested rules.
- **Remote access:** VPN logins without MFA from legacy or service accounts, or from unusual locations.
- **New administrator accounts** created outside your change process.
- **A service named `winupd`**, or `PsExec`, `pskill.exe` or `win.exe` showing up on servers (Windows event 7045 records new services).
- **Vulnerable drivers** named `filwfp.sys`, `filnk.sys` or `fildds.sys` being installed as services.
- **Queries against the Veeam configuration database's credentials table**, or encoded PowerShell launched with `cmd.exe /Q /c powershell.exe -e`.
- **Scanners** (NetScan, Advanced IP Scanner, Angry IP Scanner) running on servers.
- **rclone, MegaSync, or large 7-Zip archives** followed by large uploads to cloud storage.
- **Remote-access tools** (AnyDesk, ScreenConnect, TeamViewer) that IT didn't install.
- **Files named `INC-README.txt` or `INC-README.html`,** or unexpected print jobs containing ransom text.

Our published detection rules focus on the first step of an intrusion: exploitation of specific vulnerabilities (see the
[coverage map](https://github.com/Medina-Telemetry-Systems/detections-public/blob/main/coverage/README.md)). None of them yet
covers INC's post-access behavior. The list above is the starting point for that work.

## Mitigations
Based on the joint advisory, ordered by relevance to this actor:
1. **Patch internet-facing systems first,** especially VPNs, Citrix, Fortinet and remote-support tools, and replace
   end-of-life edge devices.
2. **Require phishing-resistant MFA** on VPNs, remote access and all administrator accounts.
3. **Restrict and monitor remote-management tools** such as AnyDesk and TeamViewer.
4. **Control privileged access:** separate admin accounts, least privilege, and disabled shared or built-in admin accounts.
5. **Keep tested, protected backups.** Isolate backup servers such as Veeam, since INC targets their stored credentials.
6. **Centralize and protect logs,** and monitor for internal scanning and unusual remote access.

## Activity this week
INC Ransom posted **53 victims** on its leak site in our collection runs of 12–16 November 2026, more than any other
group. Of those, 49 appeared in a single daily collection, which points to a batch upload rather than 49 new attacks that
day. Leak-site postings often trail the actual intrusion by weeks. See the
[weekly summary](../weekly/2026-W46.md) for the wider picture.

## Intelligence gaps
- **Which affiliates are active now,** and whether specific access brokers supply them, isn't public.
- **Current initial-access vulnerabilities:** the named CVEs date from 2023 to 2025. It isn't known which new edge-device
  flaws affiliates are using in late 2026.
- **Victim counts** come from the group's own leak site and can't be independently confirmed.

## Sources
| Source | Type | Notes |
|:--|:--|:--|
| [ACSC, CERT Tonga and NCSC New Zealand joint advisory](https://www.cyber.gov.au/advisory/inc-ransom-affiliate-model-enabling-targeting-of-critical-networks), 6 March 2026 | Government | Primary source for incidents, techniques and mitigations. Its appendix lists Data Staged as T1075; the current ATT&CK ID is T1074. |
| [Acronis Threat Research Unit](https://www.acronis.com/en/tru/posts/from-emerging-threat-to-top-tier-ransomware-as-a-service-the-evolution-of-inc-ransomware/), 17 June 2026 | Security vendor, incident response | Source for 2026 tooling, CVEs, victim statistics and code lineage. |
| [MITRE ATT&CK G1032](https://attack.mitre.org/groups/G1032/) | Reference | Aliases and earlier incident reporting (Huntress, Secureworks, SentinelOne, SOCRadar). |
| [ransomware.live](https://www.ransomware.live/), via our daily collection | Leak-site tracker | Victim postings are the group's own claims. |
