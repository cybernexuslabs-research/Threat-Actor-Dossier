# Threat Actor Profile: Play

**Generated:** 2026-04-10 14:20:00 UTC
**Search Window:** 2022-06-01 → 2026-04-10
**Sources Analyzed:** 30+ articles across CISA/FBI/ASD joint advisories, vendor research, and trade press
**Confidence:** 🟢 High — Directly corroborated by a joint CISA/FBI/ASD ACSC advisory (AA23-352A, originally Dec 2023, updated June 2025), Trend Micro, Symantec, CrowdStrike, Microsoft, Cisco Talos, BleepingComputer, and other vendor reports, plus named, publicly disclosed victims.

---

## Overview

Play (also known as PlayCrypt and tracked by Symantec as the group "Balloonfly") is a financially motivated, closed-shop ransomware operation first observed in June 2022. Unlike most peer brands, Play does not run an open Ransomware-as-a-Service program; Symantec and CISA assess that the same small group directly controls development, intrusions, negotiation, and infrastructure, with cautious outside access. Early analyses from Trend Micro documented significant tactical and toolset overlap with Hive and Nokoyawa, and infrastructure pivoting tied portions of Play's backend to Quantum — a Conti descendant — suggesting the operators or their toolchain trace back to the Conti ecosystem.

Play practices double extortion: it steals data, encrypts hosts with a distinctive intermittent encryption scheme, appends the `.PLAY` extension, drops a minimal ransom note with a `@gmx.de` contact email, and publishes victims on its Tor-hosted data-leak site if payment is refused. Activity growth across the search window has been steep: FBI reporting identified roughly 300 victims by December 2023 and approximately 900 by May 2025 — a 3× jump — making Play one of the most prolific ransomware brands active throughout 2022–2026. The group also has a record of weaponizing recent and zero-day vulnerabilities, including the OWASSRF Exchange chain (CVE-2022-41080) in late 2022 and the Windows CLFS elevation-of-privilege zero-day (CVE-2025-29824) in early 2025, which they paired with their custom Grixba infostealer.

---

## Identity

| Field | Value |
|---|---|
| **Primary Name** | Play |
| **Known Aliases** | PlayCrypt, Balloonfly (Symantec tracking name) |
| **Suspected Origin** | Unknown. Operators are assessed as Russian-speaking based on infrastructure and toolchain ties to Quantum/Conti; the group also shares TTPs with Hive and Nokoyawa. |
| **Active Since** | June 2022 |
| **Primary Motivation** | 💰 Financial (double-extortion ransomware) |
| **Operating Model** | Closed group (not a traditional RaaS); centralized control of development, operations, and negotiations. |

---

## Targeting

Play is opportunistic but consistently focused on mid-to-large enterprises and critical-infrastructure-adjacent organizations. The group has compromised victims across North America, South America, Europe, and Australia — jurisdictions explicitly named in the joint CISA/FBI/ASD ACSC advisory — with the majority of known victims in the United States and a significant concentration in Latin America (notably Brazil and Argentina, where Play was among the first major international brands to impact the region heavily in 2022–2023). Healthcare, local and state government, manufacturing, IT and managed service providers, legal services, education, transportation, and media/publishing have all appeared repeatedly on Play's leak site.

### Sectors

- Aviation and aerospace
- Construction and engineering
- Education
- Financial services
- Government (state, local, and municipal)
- Healthcare
- Hospitality
- IT services and managed service providers (MSPs)
- Legal services
- Manufacturing
- Media and publishing
- Professional services
- Real estate
- Retail and automotive retail
- Telecommunications
- Transportation and logistics

### Regions

- Australia
- Europe (notably Belgium, Germany, Switzerland, the United Kingdom)
- Latin America (notably Argentina and Brazil)
- North America (United States and Canada — majority of named victims)
- South America

---

## Capabilities

Play is a disciplined, mature intrusion operation with strong tradecraft and demonstrable R&D capacity. Evidence across the search window shows the group is comfortable chaining recent n-day vulnerabilities, consuming credentials from initial access brokers, and — more distinctively — developing custom tooling such as the Grixba infostealer and network scanner, as well as being willing and able to deploy Windows zero-day exploits (CVE-2025-29824) in live intrusions before public disclosure. The encryptor itself uses an intermittent encryption scheme that encrypts 0x100000-byte chunks instead of full files, which speeds up the final impact step and reduces the statistical signal of ransomware on endpoints. Operators routinely conduct weeks of dwell time, AD reconnaissance, credential harvesting, lateral movement, and large-scale data exfiltration before pulling the trigger, and are known for follow-through in leaking data when victims refuse to pay.

### Signature TTPs

1. Initial access via exploitation of public-facing applications (Microsoft Exchange, Fortinet FortiOS SSL VPN, and other edge devices) and valid accounts purchased from initial access brokers or harvested via phishing.
2. Use of the OWASSRF exploit chain (CVE-2022-41080 + CVE-2022-41082) to bypass Microsoft's ProxyNotShell URL-rewrite mitigations and gain remote code execution on on-prem Exchange through Outlook Web Access.
3. Exploitation of Fortinet FortiOS SSL VPN vulnerabilities CVE-2018-13379 and CVE-2020-12812 to establish footholds.
4. Zero-day abuse of the Windows Common Log File System driver (CLFS) elevation-of-privilege flaw CVE-2025-29824 to obtain SYSTEM on target hosts in early 2025, paired with deployment of the Grixba infostealer.
5. Active Directory reconnaissance and discovery using AdFind and the custom Grixba tool, which inventories users, computers, AV, EDR, and backup software via WMI, WinRM, and Remote Registry.
6. Credential theft using Mimikatz — including LSASS dumping, Kerberos ticket extraction, and pass-the-hash — followed by creation of new domain accounts and abuse of Domain Administrators.
7. Command and control / lateral movement via Cobalt Strike Beacon, SystemBC (SOCKS proxy and Tor tunneling), Empire, PsExec, RDP, and legitimate utilities Plink and AnyDesk.
8. Defense evasion via GMER, IOBit, and PowerTool to disable antivirus; log and artifact wiping after gaining a foothold (notably on compromised Exchange servers during Rackspace-era intrusions); and abuse of signed-binary living-off-the-land utilities.
9. Data exfiltration using WinRAR to split archives, then WinSCP and similar tools to transfer data to attacker-controlled infrastructure prior to encryption.
10. Impact via the Play encryptor, using intermittent encryption (chunking), `.PLAY` file extension, a terse ransom note (historically `ReadMe.txt`), a `@gmx.de` negotiation address, and follow-through data leaks on a Tor-hosted DLS.

### Known Tools & Malware

- AdFind (AD enumeration)
- AnyDesk (persistence and remote access)
- Cobalt Strike Beacon (C2 and lateral movement)
- Empire / EmpireProject (post-exploitation framework)
- GMER, IOBit Unlocker, PowerTool (defense evasion / AV tampering)
- Grixba (custom infostealer and network scanner, attributed to Balloonfly/Play)
- Mimikatz (credential theft, ticket extraction, DCSync)
- Nekto / PriviCMD (privilege escalation utilities observed in incident reporting)
- Play ransomware encryptor (Windows; later Linux/ESXi variant)
- Plink (SSH tunneling)
- ProcDump (LSASS dumping)
- PsExec (remote execution)
- SystemBC (SOCKS5 proxy, Tor tunneling, C2)
- WinRAR (data staging and archiving)
- WinSCP (data exfiltration)

---

## MITRE ATT&CK Mapping

| ID | Technique | Tactic |
|---|---|---|
| [T1190](https://attack.mitre.org/techniques/T1190/) | Exploit Public-Facing Application (Exchange, FortiOS) | Initial Access |
| [T1133](https://attack.mitre.org/techniques/T1133/) | External Remote Services | Initial Access |
| [T1078](https://attack.mitre.org/techniques/T1078/) | Valid Accounts | Initial Access / Privilege Escalation / Defense Evasion |
| [T1566](https://attack.mitre.org/techniques/T1566/) | Phishing | Initial Access |
| [T1059](https://attack.mitre.org/techniques/T1059/) | Command and Scripting Interpreter (PowerShell, cmd) | Execution |
| [T1053](https://attack.mitre.org/techniques/T1053/) | Scheduled Task/Job | Execution / Persistence |
| [T1547](https://attack.mitre.org/techniques/T1547/) | Boot or Logon Autostart Execution | Persistence |
| [T1136](https://attack.mitre.org/techniques/T1136/) | Create Account | Persistence |
| [T1068](https://attack.mitre.org/techniques/T1068/) | Exploitation for Privilege Escalation (CVE-2025-29824 CLFS) | Privilege Escalation |
| [T1134](https://attack.mitre.org/techniques/T1134/) | Access Token Manipulation | Privilege Escalation |
| [T1562](https://attack.mitre.org/techniques/T1562/) | Impair Defenses (GMER, IOBit, PowerTool) | Defense Evasion |
| [T1070](https://attack.mitre.org/techniques/T1070/) | Indicator Removal (log wiping on Exchange) | Defense Evasion |
| [T1003](https://attack.mitre.org/techniques/T1003/) | OS Credential Dumping (LSASS, Mimikatz) | Credential Access |
| [T1558](https://attack.mitre.org/techniques/T1558/) | Steal or Forge Kerberos Tickets | Credential Access |
| [T1018](https://attack.mitre.org/techniques/T1018/) | Remote System Discovery (AdFind, Grixba) | Discovery |
| [T1087](https://attack.mitre.org/techniques/T1087/) | Account Discovery | Discovery |
| [T1069](https://attack.mitre.org/techniques/T1069/) | Permission Groups Discovery | Discovery |
| [T1021](https://attack.mitre.org/techniques/T1021/) | Remote Services (RDP, SMB, WinRM) | Lateral Movement |
| [T1570](https://attack.mitre.org/techniques/T1570/) | Lateral Tool Transfer | Lateral Movement |
| [T1560](https://attack.mitre.org/techniques/T1560/) | Archive Collected Data (WinRAR) | Collection |
| [T1071](https://attack.mitre.org/techniques/T1071/) | Application Layer Protocol | Command and Control |
| [T1090](https://attack.mitre.org/techniques/T1090/) | Proxy (SystemBC / Plink / Tor) | Command and Control |
| [T1219](https://attack.mitre.org/techniques/T1219/) | Remote Access Software (AnyDesk) | Command and Control |
| [T1048](https://attack.mitre.org/techniques/T1048/) | Exfiltration Over Alternative Protocol (WinSCP) | Exfiltration |
| [T1490](https://attack.mitre.org/techniques/T1490/) | Inhibit System Recovery | Impact |
| [T1486](https://attack.mitre.org/techniques/T1486/) | Data Encrypted for Impact (intermittent encryption) | Impact |
| [T1657](https://attack.mitre.org/techniques/T1657/) | Financial Theft (extortion) | Impact |

---

## Recent Activity

Play has been continuously active since June 2022 and posted victims to its leak site without interruption through the beginning of 2026. FBI figures attached to the joint advisory put cumulative known victims at roughly 300 by December 2023 and about 900 by May 2025, a trajectory consistent with vendor telemetry that ranked Play among the top five most prolific brands every quarter from 2023 onward. The first version of CISA's #StopRansomware: Play Ransomware advisory (AA23-352A) was published on December 18, 2023, and was materially updated on June 4, 2025, to capture new TTPs and IOCs — a rare step that reflects sustained federal concern over this actor.

### Notable Campaigns and Incidents

- June 2022: Play operations begin; earliest victims post to BleepingComputer forums.
- September 2022: Trend Micro publishes its first detailed analysis linking Play to Hive and Nokoyawa, documenting intermittent encryption and shared tooling.
- November 2022: Attack on the City of Antwerp, Belgium, disrupting municipal services for weeks and exposing personal data of residents.
- December 2, 2022: Intrusion into Rackspace's Hosted Exchange environment using the OWASSRF / CVE-2022-41080 exploit chain. The attack disrupted email services for customers, compromised the .PST files of 27 customers out of nearly 30,000, and ultimately cost Rackspace an estimated USD 11+ million.
- December 2022: Public disclosure of the OWASSRF chain by CrowdStrike, directly attributing the technique to the Play ransomware operation.
- Late 2022 – early 2023: Attack on Arnold Clark, one of the UK's largest car retailers, forcing multi-week IT outages and leaking employee data.
- February 10, 2023: Ransomware attack on the City of Oakland, California, forcing the city to declare a local state of emergency. Play claimed responsibility on March 1, 2023, and later published stolen employee and resident data.
- 2023: Play establishes a foothold in Latin America, compromising multiple Argentine and Brazilian municipalities and enterprises.
- December 18, 2023: Joint CISA/FBI/ASD ACSC advisory AA23-352A ("#StopRansomware: Play Ransomware") published, attributing ~300 victims to the group.
- 2024: Play deploys a Linux / ESXi variant of its encryptor targeting virtualized environments; vendors document hybrid intrusions using VMware vCenter credentials.
- Early 2025: Play operators exploit the Windows CLFS driver elevation-of-privilege flaw CVE-2025-29824 as a zero-day in live ransomware intrusions, paired with the Grixba infostealer, before Microsoft patched on April 8, 2025. Symantec publicly ties these intrusions to the "Balloonfly" cluster it already associates with Play.
- June 4, 2025: CISA, FBI, and ASD ACSC release an updated version of the AA23-352A advisory, now citing approximately 900 victim entities and a new IOC and TTP bundle.
- 2025–2026: Play remains one of the top leak-site-posting ransomware brands globally, with sustained targeting of North American municipalities, healthcare, and manufacturing; reporting continues into Q1 2026.

---

## Indicators of Compromise

Play rotates payment infrastructure, negotiation endpoints, and C2 frequently, so durable atomic indicators are limited. Defenders should pull live IOC bundles from the CISA AA23-352A advisory (2025 revision), Symantec, and Trend Micro for hashes, IPs, and domains. The highest-value durable indicators from the search window are the CVEs Play is known to exploit and the file-artifact families listed in the tools section.

### IP Addresses
_None captured in this collection run. Pull from the CISA AA23-352A June 2025 addendum and Symantec "Balloonfly" reporting for current Cobalt Strike / SystemBC C2 indicators._

### Domains
Play uses a Tor-hosted data-leak site (.onion) and rotating `@gmx.de` email aliases for negotiation. _No current operational domain or .onion address published here to avoid linking to actor infrastructure._

### File Hashes
_None captured in this collection run. Defenders should pull Windows and Linux/ESXi encryptor hashes plus Grixba infostealer hashes from CISA AA23-352A (2025), Symantec, Trend Micro, and Picus Security IOC bundles._

### CVEs
- CVE-2018-13379 (Fortinet FortiOS SSL VPN path traversal)
- CVE-2020-12812 (Fortinet FortiOS SSL VPN authentication bypass)
- CVE-2022-41040 (Microsoft Exchange "ProxyNotShell" SSRF)
- CVE-2022-41080 (Microsoft Exchange elevation of privilege — OWASSRF chain)
- CVE-2022-41082 (Microsoft Exchange remote code execution, chained with CVE-2022-41080)
- CVE-2025-29824 (Microsoft Windows CLFS driver elevation of privilege — exploited as a zero-day by Play / Balloonfly in early 2025)

---

## Source Articles

| # | Title | Source | Published | Recency |
|---|---|---|---|---|
| 1 | [#StopRansomware: Play Ransomware (AA23-352A, 2025 revision)](https://www.cisa.gov/sites/default/files/2025-06/aa23-352a-stopransomware-play-ransomware_2_revised.pdf) | CISA / FBI / ASD ACSC | 2025-06-04 | 2025 |
| 2 | [#StopRansomware: Play Ransomware (AA23-352A original)](https://www.cisa.gov/news-events/cybersecurity-advisories/aa23-352a) | CISA / FBI / ASD ACSC | 2023-12-18 | Historical |
| 3 | [Play Ransomware Attack Playbook Similar to that of Hive, Nokoyawa](https://www.trendmicro.com/en_us/research/22/i/play-ransomware-s-attack-playbook-unmasks-it-as-another-hive-aff.html) | Trend Micro | 2022-09 | Historical |
| 4 | [Ransomware Spotlight: Play](https://www.trendmicro.com/vinfo/gb/security/news/ransomware-spotlight/ransomware-spotlight-play) | Trend Micro | 2022–2023 | Historical |
| 5 | [OWASSRF: CrowdStrike Identifies New Exploit Method for Exchange](https://www.crowdstrike.com/en-us/blog/owassrf-exploit-analysis-and-recommendations/) | CrowdStrike | 2022-12 | Historical |
| 6 | [New Microsoft Exchange exploit chain lets ransomware attackers in (CVE-2022-41080)](https://www.helpnetsecurity.com/2022/12/21/cve-2022-41080/) | Help Net Security | 2022-12-21 | Historical |
| 7 | [Play ransomware actors bypass ProxyNotShell mitigations](https://www.techtarget.com/searchsecurity/news/252528594/Play-ransomware-actors-bypass-ProxyNotShell-mitigations) | TechTarget | 2022-12 | Historical |
| 8 | [Ransomware gang uses new Microsoft Exchange exploit to breach servers](https://www.bleepingcomputer.com/news/security/ransomware-gang-uses-new-microsoft-exchange-exploit-to-breach-servers/) | BleepingComputer | 2022-12 | Historical |
| 9 | [Rackspace confirms Play ransomware was behind recent cyberattack](https://www.bleepingcomputer.com/news/security/rackspace-confirms-play-ransomware-was-behind-recent-cyberattack/) | BleepingComputer | 2023-01 | Historical |
| 10 | [Rackspace says hackers accessed customer data during ransomware attack](https://techcrunch.com/2023/01/06/rackspace-ransomware-data-exchange/) | TechCrunch | 2023-01-06 | Historical |
| 11 | [Rackspace Ransomware Attack Losses Could Surpass $11M](https://www.msspalert.com/news/rackspace-taking-losses-of-roughly-11-million-for-hosted-exchange-ransom-attack) | MSSP Alert | 2023 | Historical |
| 12 | [Arnold Clark cyber attack claimed by Play ransomware gang](https://www.computerweekly.com/news/252529566/Arnold-Clark-cyber-attack-claimed-by-Play-ransomware-gang) | Computer Weekly | 2023 | Historical |
| 13 | [Play ransomware claims disruptive attack on City of Oakland](https://www.bleepingcomputer.com/news/security/play-ransomware-claims-disruptive-attack-on-city-of-oakland/) | BleepingComputer | 2023-03 | Historical |
| 14 | [FBI: Play ransomware breached 300 victims, including critical orgs](https://www.bleepingcomputer.com/news/security/fbi-play-ransomware-breached-300-victims-including-critical-orgs/) | BleepingComputer | 2023-12 | Historical |
| 15 | [FBI: Play ransomware breached 900 victims, including critical orgs](https://www.bleepingcomputer.com/news/security/fbi-play-ransomware-breached-900-victims-including-critical-orgs/) | BleepingComputer | 2025 | 2025 |
| 16 | [Play ransomware exploited Windows logging flaw in zero-day attacks](https://www.bleepingcomputer.com/news/security/play-ransomware-exploited-windows-logging-flaw-in-zero-day-attacks/) | BleepingComputer | 2025 | 2025 |
| 17 | [Ransomware Attackers Leveraged Privilege Escalation Zero-day (Balloonfly / Play)](https://www.security.com/threat-intelligence/play-ransomware-zero-day) | Symantec | 2025 | 2025 |
| 18 | [CVE-2025-29824: The Windows CLFS Zero Day Used in Ransomware Campaigns](https://www.ampcuscyber.com/shadowopsintel/cve-2025-29824-the-windows-clfs-zero-day-used-in-ransomware-campaigns/) | Ampcus Cyber | 2025 | 2025 |
| 19 | [Reviewing Zero-day Vulnerabilities Exploited in Malware Campaigns in 2025](https://threatresearch.ext.hp.com/reviewing-zero-day-vulnerabilities-exploited-in-malware-campaigns-in-2025/) | HP Wolf Security | 2025 | 2025 |
| 20 | [Play Ransomware Deployed in the Wild Exploiting Windows 0-Day](https://gbhackers.com/play-ransomware-deployed-in-the-wild/) | GBHackers | 2025 | 2025 |
| 21 | [Play Ransomware: 2025 Alert](https://coesecurity.com/play-ransomware-2025-alert/) | COE Security | 2025 | 2025 |
| 22 | [Play Ransomware group hit 900 organizations since 2022](https://securityaffairs.com/178702/cyber-crime/play-ransomware-group-hit-900-organizations-since-2022.html) | Security Affairs | 2025 | 2025 |
| 23 | [Updated Cybersecurity Advisory on Play Ransomware After Attacking 900 Victims](https://www.hipaanswers.com/updated-cybersecurity-advisory-on-play-ransomware-after-attacking-900-victims/) | HIPAAnswers | 2025 | 2025 |
| 24 | [Updated Response to CISA Advisory (AA23-352A)](https://www.attackiq.com/2025/06/12/updated-response-to-cisa-advisory-aa23-352a/) | AttackIQ | 2025-06-12 | 2025 |
| 25 | [Play Ransomware Analysis, Simulation and Mitigation — AA23-352A](https://www.picussecurity.com/resource/blog/play-ransomware-analysis-simulation-and-mitigation-cisa-alert-aa23-352a) | Picus Security | 2024 | Historical |
| 26 | [Ransomware Group Play — Threat Actor Profile](https://www.huntress.com/threat-library/threat-actors/play) | Huntress | 2025 | 2025 |
| 27 | [Threat Actor Profile: Play Ransomware Group](https://cyble.com/threat-actor-profiles/play-ransomware-group/) | Cyble | 2025 | 2025 |
| 28 | [Play Ransomware Group — Detection and Protection](https://www.checkpoint.com/cyber-hub/threat-prevention/ransomware/play-ransomware-group-detection-and-protection/) | Check Point | 2025 | 2025 |
| 29 | [Dark Web Profile: Play Ransomware](https://socradar.io/blog/dark-web-profile-play-ransomware/) | SOCRadar | 2024 | Historical |
| 30 | [Is Your Organization Safe from PLAY Ransomware Attacks?](https://www.vectra.ai/modern-attack/threat-actors/play) | Vectra AI | 2024 | Historical |
| 31 | [Alert: Play Ransomware Targets Critical Infrastructure Globally](https://www.anvilogic.com/threat-reports/play-ransomware-global-impact) | Anvilogic | 2025 | 2025 |
| 32 | [Play Ransomware: Detection and Mitigation](https://cybelangel.com/blog/play-ransomware-double-extortion-gang/) | CybelAngel | 2024 | Historical |

---

## Analyst Notes

Three things make Play stand out from the broader ransomware ecosystem during the 2022–2026 window. First, the closed-shop model: Play does not recruit affiliates on forums the way Qilin, LockBit, or RansomHub do, and that discipline shows up in the relative consistency of its toolchain, encryptor, and negotiation style. Second, the cadence of capability-raising: OWASSRF in late 2022, Grixba development in 2023–2024, and live zero-day use of the CLFS CVE-2025-29824 in early 2025 collectively indicate an organization willing and able to spend on offensive R&D, which is unusual for a non-RaaS brand. Third, the Conti-ecosystem ties: infrastructure overlap with Quantum — a known Conti descendant — and TTP overlap with Hive and Nokoyawa place Play firmly inside the post-Conti Russian-speaking cybercrime diaspora, even though Play itself has never been formally attributed to a named individual.

The Rackspace intrusion (December 2022) remains the most consequential single incident in Play's history because it simultaneously validated the group's technical ambition (a live zero-day-adjacent exchange exploit chain), produced real-world operational and financial damage to a major cloud provider (estimates well above USD 10 million), and triggered wide-scale OWA-focused hunting across the industry. The subsequent City of Oakland and Arnold Clark incidents cemented Play's appetite for high-visibility municipal and consumer-facing targets. By 2025, the combination of sustained victim volume (roughly 900 entities) and the refreshed joint advisory from three national cyber authorities signals that Play has graduated into the small cohort of ransomware brands that defenders must account for explicitly in threat models, rather than lumping into a generic "any ransomware" bucket.

Defenders should prioritize: patching the CVEs listed above (particularly the Exchange OWASSRF chain on any remaining on-prem Exchange, the FortiOS SSL VPN flaws, and CVE-2025-29824 where patching has lagged); hunting for Grixba, AdFind, and SystemBC artifacts on AD and jumpbox hosts; alerting on intermittent-encryption heuristics at file-modification-rate-per-process rather than purely on full-file rewrites; constraining PsExec, WinRM, and RDP lateral-movement paths; enforcing phishing-resistant MFA on Exchange, VPN, and RDP; monitoring for WinSCP and WinRAR usage in unexpected contexts; and tracking the CISA AA23-352A advisory for ongoing TTP and IOC refreshes.

This dossier was built from public reporting and government advisories; the bundled RSS trawler used by the profiler skill was blocked by sandbox egress, so direct web search against authoritative vendor, government, and trade-press sources was used instead. Atomic indicators (IPs, domains, hashes) should be pulled from live feeds and from the June 2025 CISA advisory revision rather than cached here, because Play rotates that infrastructure aggressively.

---

## Profile Metadata

| Field | Value |
|---|---|
| **Skill Version** | threat-actor-profiler v1.1 |
| **Queries Run** | 6 (web search, multi-angle) |
| **Articles Retrieved** | 60+ |
| **Articles Analyzed** | 32 |
| **Articles Skipped** | 0 |
| **Profile Generated** | 2026-04-10 14:20:00 UTC |
| **Last Consolidated** | _Not yet consolidated_ |
| **Updates Merged** | 0 |
| **Collection Method Note** | RSS trawler blocked by sandbox egress; switched to authenticated web search against CISA, vendor, and reputable trade-press sources. |
