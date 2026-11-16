# The Gentlemen

| | |
|:--|:--|
| **Also known as** | Storm-2697 (Microsoft), GOLD SHERWOOD; MITRE ATT&CK [G1058](https://attack.mitre.org/groups/G1058/). Group-IB links its origins to ArmCorp, a former Qilin affiliate group |
| **Type** | Ransomware-as-a-Service (RaaS) with double extortion |
| **Motivation** | Financial |
| **Active since** | Mid-2025; open to affiliates since September 2025 |
| **Main targets** | Manufacturing first, then construction, health care, insurance and others; victims in at least 77 countries |
| **Profile date** | 16 November 2026 |

## Bottom line
The Gentlemen grew from a closed operation in mid-2025 into one of the most active ransomware programs of 2026. Its
affiliates mostly get in through Fortinet FortiGate firewalls and VPNs, using an old authentication-bypass flaw, brute-forced
VPN logins or stolen credentials. Once they hold domain admin rights, they disable security tools with vulnerable drivers and
can encrypt the whole domain quickly: the encryptor can spread itself to every reachable machine. Organizations with
internet-facing FortiGate devices, VPNs without multi-factor authentication (MFA), or weak control over Group Policy are
most at risk.

## Key judgments
1. **The Gentlemen will very likely remain one of the three most active ransomware programs into 2027** (moderate
   confidence). Unit 42 ranks it second by victims in 2026, behind only Qilin and Akira. Its victim count rose about six-fold
   from the second half of 2025 to the first half of 2026, and its 90% affiliate payout is well above the usual 70–80%.
2. **FortiGate devices and stolen VPN credentials will remain its main ways in** (high confidence). Group-IB, Unit 42 and
   Trend Micro all describe edge-device exploitation and credential abuse, and Group-IB reports lists of thousands of
   compromised FortiGate devices plus 969 brute-forced FortiGate VPN credentials offered to affiliates.
3. **Once an affiliate has domain admin rights, defenders have very little time** (high confidence). The encryptor's
   `--spread` option makes 21 separate remote-execution attempts per target, using several different methods (Microsoft), and affiliates also push the payload
   through Group Policy and the NETLOGON share (Group-IB, Trend Micro). Detection has to happen earlier, at the VPN login,
   the driver load or the Defender change.
4. **Disabling security tools with vulnerable drivers is a standard step, not an occasional one** (high confidence). Trend
   Micro, Group-IB, ESET and Unit 42 all document bring-your-own-vulnerable-driver (BYOVD) tools, including the group's own
   "GentleKiller" framework.
5. **The May 2026 leak of the group's internal data did not slow it down** (moderate confidence). Unit 42 reports that June
   2026 was its busiest month, with 117 victims.

## Background
- **Origins.** MITRE ATT&CK describes the group as first seen in mid-2025, running its ransomware as a closed operation
  before opening it to affiliates in September 2025. Group-IB reports that it grew out of ArmCorp, a Qilin affiliate group,
  after a payment dispute with Qilin. It describes a Russian-speaking operator using the name "hastalamuerte" and a team of
  about 20 members.
- **Affiliate model.** Unit 42 reports a 90% payout to affiliates. Group-IB reports that the operator also gives affiliates
  resources such as lists of compromised Fortinet devices, EDR-killing tools and scripts.
- **May 2026 leak.** Unit 42 and Barracuda report that the group's internal communications were leaked in May 2026,
  allegedly by an insider.
- **Encryptors.** Windows, Linux and VMware ESXi versions exist. Microsoft analyzed a self-propagating Windows encryptor
  written in Go in May 2026, and Unit 42 reports variants written in both C and Go.

## Victims
- **Scale:** 580 claimed victims in 77 countries through 7 July 2026 (Unit 42), and more than 750 by mid-August 2026
  (Barracuda), both using ransomware.live data. These are leak-site claims, not confirmed incidents.
- **Sectors:** manufacturing leads with about 18% of victims (Unit 42). Trend Micro's first report also named construction,
  health care and insurance.
- **Geography:** victims are worldwide. Trend Micro's 2025 report named Thailand and the United States as key targets.

## How an attack unfolds
Each row names the source. "Microsoft" refers to its May 2026 encryptor analysis.

| Stage | Technique | What has been reported | Source |
|:--|:--|:--|:--|
| Initial access | [T1190](https://attack.mitre.org/techniques/T1190/) Exploit Public-Facing Application | FortiOS/FortiProxy authentication bypass CVE-2024-55591 as the primary vector; edge devices such as firewalls and VPNs | Group-IB; Unit 42 |
| Initial access | [T1078](https://attack.mitre.org/techniques/T1078/) Valid Accounts, [T1110](https://attack.mitre.org/techniques/T1110/) Brute Force | Brute-forced FortiGate VPN credentials; stolen credentials; initial access brokers | Group-IB; Unit 42 |
| Discovery | [T1018](https://attack.mitre.org/techniques/T1018/) Remote System Discovery, [T1087](https://attack.mitre.org/techniques/T1087/) Account Discovery | Advanced IP Scanner, Nmap and NetExec; a batch script (`1.bat`) checking more than 60 accounts; PowerShell lookup of the domain's PDC emulator | Trend Micro; Group-IB |
| Credential access | [T1555](https://attack.mitre.org/techniques/T1555/) Credentials from Password Stores | Credentials pulled from Fortinet configurations (`userpassfort.py`); DPAPI backup keys via Impacket; a custom Chrome credential stealer | Group-IB |
| Defense evasion | [T1068](https://attack.mitre.org/techniques/T1068/) Exploitation for Privilege Escalation, [T1685](https://attack.mitre.org/techniques/T1685/) Disable or Modify Tools | Vulnerable drivers `ThrottleBlood.sys` and `viragt64.sys` to kill security processes; tools `All.exe`, `Allpatch2.exe`, `PowerRun.exe`; the GentleKiller framework | Trend Micro; Group-IB; ESET; MITRE |
| Defense evasion | [T1685](https://attack.mitre.org/techniques/T1685/) Disable or Modify Tools | `Set-MpPreference -DisableRealtimeMonitoring` and a Defender exclusion for `C:\`; Group Policy used to disable Defender across the domain | Trend Micro; Microsoft; Group-IB |
| Defense evasion | [T1036](https://attack.mitre.org/techniques/T1036/) Masquerading | Payloads named and iconed like known vendors, including security vendors; copied code signatures; rclone renamed `avastrclone.exe` | MITRE (citing ESET); Group-IB |
| Lateral movement | [T1021](https://attack.mitre.org/techniques/T1021/) Remote Services, [T1484.001](https://attack.mitre.org/techniques/T1484/001/) Group Policy Modification | PsExec and RDP (enabled through registry and firewall changes); payload staged in the NETLOGON share and pushed by Group Policy | Trend Micro; Group-IB |
| Execution and spread | [T1053.005](https://attack.mitre.org/techniques/T1053/005/) Scheduled Task, [T1543.003](https://attack.mitre.org/techniques/T1543/003/) Windows Service, [T1047](https://attack.mitre.org/techniques/T1047/) WMI | Encryptor `--spread`: copies itself to `C$`, then runs through PsExec, WMI, PowerShell remoting, scheduled tasks (`DefU`, `UpdateGU`, `UpdateGU2`) and services (`DefSvc`, `UpdateSvc`, `UpdateSvc2`) | Microsoft |
| Persistence | [T1547.001](https://attack.mitre.org/techniques/T1547/001/) Registry Run Keys, [T1219](https://attack.mitre.org/techniques/T1219/) Remote Access Tools | Run values `GupdateS` and `GupdateU`; scheduled tasks `UpdateSystem`, `UpdateUser`, `gentlemen_system`; AnyDesk | Microsoft; Trend Micro |
| Command and control | [T1090](https://attack.mitre.org/techniques/T1090/) Proxy | SystemBC proxy malware | Check Point (via MITRE); Unit 42 |
| Exfiltration | [T1048](https://attack.mitre.org/techniques/T1048/) Exfiltration Over Alternative Protocol, [T1567](https://attack.mitre.org/techniques/T1567/) Exfiltration Over Web Service | WinSCP; rclone launched by a scheduled task named `SafeSync` | Trend Micro; Group-IB |
| Impact | [T1486](https://attack.mitre.org/techniques/T1486/) Data Encrypted for Impact, [T1490](https://attack.mitre.org/techniques/T1490/) Inhibit System Recovery | Ransom note `README-GENTLEMEN.txt`; extension varies by build (`.7mtzhh` in 2025, `.umc16h` in Microsoft's sample); shadow copies deleted with `vssadmin` and `wmic`; event logs cleared with `wevtutil`; wallpaper `gentlemen.bmp`; ESXi VMs powered off before encryption | Trend Micro; Microsoft; Group-IB |

## Detection and hunting ideas
These are leads, not tested rules.
- **FortiGate:** admin logins or configuration changes from unfamiliar addresses, and many failed VPN logins followed by a
  success for the same account.
- **Vulnerable drivers** `ThrottleBlood.sys` or `viragt64.sys` being loaded or installed as a service.
- **Defender changes:** `Set-MpPreference -DisableRealtimeMonitoring`, or an exclusion for the whole `C:\` drive.
- **New scheduled tasks** named `DefU`, `UpdateGU`, `UpdateGU2`, `UpdateSystem`, `UpdateUser`, `gentlemen_system` or `SafeSync`,
  and **new services** named `DefSvc`, `UpdateSvc` or `UpdateSvc2`.
- **Run values** `GupdateS` (HKLM) or `GupdateU` (HKCU).
- **Executables appearing in the NETLOGON share,** or Group Policy changes made outside your change process.
- **RDP being opened up:** registry changes to `RestrictSendingNTLMTraffic` or `DisableRestrictedAdmin`, and
  `netsh firewall set service type remotedesktop mode enable`.
- **`avastrclone.exe`, `WinSCP.exe` in `C:\ProgramData`,** or large uploads from servers.
- **`README-GENTLEMEN.txt`** or `gentlemen.bmp` appearing on disk, and `wevtutil cl` or `vssadmin delete shadows` on servers.

Our published rules don't yet cover these behaviors (see the
[coverage map](https://github.com/Medina-Telemetry-Systems/detections-public/blob/main/coverage/README.md)).

## Mitigations
1. **Patch FortiGate and FortiProxy** against CVE-2024-55591
   ([Fortinet FG-IR-24-535](https://fortiguard.fortinet.com/psirt/FG-IR-24-535)) and keep their admin interfaces off the internet.
2. **Require MFA on every VPN account** and lock out repeated failed logins.
3. **Turn on Microsoft's vulnerable driver blocklist and Defender tamper protection** to blunt driver-based EDR killing.
4. **Block PsExec- and WMI-launched processes** with attack surface reduction rules where possible (Microsoft).
5. **Control and monitor Group Policy and the NETLOGON share:** limit who can edit Group Policy and alert on changes.
6. **Keep offline, tested backups,** and separate vCenter and ESXi management from user networks.

## Activity this week
The Gentlemen posted **22 victims** in our collection runs of 12–16 November 2026, all in a single daily collection. It was
the third most active group this week. See the [weekly summary](../weekly/2026-W46.md).

## Intelligence gaps
- **Which FortiGate or other edge-device vulnerabilities** affiliates are using now, beyond CVE-2024-55591.
- **Whether the May 2026 leak changed the affiliate base** or the group's tooling.
- **Victim counts** come from the group's own leak site and can't be independently confirmed.

## Sources
| Source | Type | Notes |
|:--|:--|:--|
| [MITRE ATT&CK G1058](https://attack.mitre.org/groups/G1058/) | Reference | Aliases, timeline and techniques; cites Check Point, ESET, Huntress, Intel 471, Sophos and others |
| [Microsoft Threat Intelligence](https://www.microsoft.com/en-us/security/blog/2026/05/28/the-gentlemen-ransomware-dissecting-a-self-propagating-go-encryptor/), 28 May 2026 | Security vendor | Self-propagating Go encryptor, task, service and registry names, mitigations |
| [Unit 42](https://unit42.paloaltonetworks.com/the-gentlemen-ransomware/), 10 July 2026 | Security vendor | Victim statistics, affiliate payout, 2026 ranking |
| [Group-IB](https://www.group-ib.com/blog/hastalamuerte-gentlemen-raas-ttps/), 19 March 2026 | Security vendor | Origins, FortiGate access, tooling, drivers, Group Policy deployment |
| [Trend Micro](https://www.trendmicro.com/en_gb/research/25/i/unmasking-the-gentlemen-ransomware.html), 9 September 2025 | Security vendor, incident response | First public analysis; tools, registry changes, encryptor behavior |
| [Barracuda](https://blog.barracuda.com/2026/08/17/the-gentlemen-ransomware--inside-one-of-the-fastest-growing-rans), 17 August 2026 | Security vendor | Victim count to August 2026, May 2026 leak |
| [ransomware.live](https://www.ransomware.live/group/thegentlemen), via our daily collection | Leak-site tracker | Victim postings are the group's own claims |
