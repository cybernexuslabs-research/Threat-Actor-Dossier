# Threat Actor Profile: Qilin

**Generated:** 2026-04-10 14:00:00 UTC
**Search Window:** 2022-07-01 → 2026-04-10
**Sources Analyzed:** 30+ articles across vendor research, government advisories, and trade press
**Confidence:** 🟢 High — Corroborated reporting from multiple reputable vendors (Trend Micro, Talos, Sophos, Huntress, Group-IB, KELA, Check Point, SANS, HHS HC3), plus named, verifiable victim disclosures.

---

## Overview

Qilin (formerly "Agenda") is a Russian-speaking, financially motivated Ransomware-as-a-Service (RaaS) operation first observed in early July 2022 and rebranded from Agenda to Qilin in September 2022. The operation runs a mature affiliate program that provides customizable Go- and Rust-based encryptors for Windows, Linux, and ESXi, a negotiation/affiliate panel, and a Tor-hosted data leak site (also mirrored on a clear-web domain known as "WikiLeaksV2"). Qilin practices double extortion, combining encryption with data theft and public shaming.

Over the search window, Qilin evolved from a mid-tier newcomer in late 2022 into what multiple trackers ranked as the most prolific ransomware brand of 2025, claiming over 1,000 victims that year and more than 50 additional victims by early 2026. The group's trajectory accelerated sharply after the RansomHub RaaS shutdown in 2025, when Qilin absorbed displaced affiliates and aggressively courted operators from other closed programs. It has also been linked to collaboration with Scattered Spider affiliates and to reported use by actors aligned with North Korean interests. By Q1 2026, Qilin was among the first RaaS brands publicly confirmed to have integrated AI agents into parts of its attack pipeline.

---

## Identity

| Field | Value |
|---|---|
| **Primary Name** | Qilin |
| **Known Aliases** | Agenda, Agenda Crypt, Water Galura (Trend Micro tracking name) |
| **Suspected Origin** | Russia / Commonwealth of Independent States (Russian-speaking operators; code excludes CIS victims; affiliate recruitment on Russian-language forums) |
| **Active Since** | July 2022 (as Agenda); September 2022 rebrand to Qilin |
| **Primary Motivation** | 💰 Financial (ransomware + data-theft extortion) |

---

## Targeting

Qilin is opportunistic and cross-sector. Because it runs a RaaS with an 80–85% affiliate cut on payments under USD 3 million (and 85% above that threshold), actual targeting is driven by whichever affiliates are active at a given time, which produces a broad, globally distributed victim pool. Analysis of its leak site and vendor telemetry over 2022–2026 shows a consistent emphasis on critical-infrastructure-adjacent sectors where downtime creates maximum pressure to pay, particularly healthcare, manufacturing, local/state government, and media. U.S. organizations account for roughly half of publicly named victims, with Europe, APAC (notably Japan and South Korea), Australia, Africa, and Latin America also represented. Qilin's affiliate rules explicitly prohibit attacks inside Russia and other CIS states.

### Sectors

- Aerospace and defense
- Education (K–12 and higher ed)
- Energy and utilities
- Financial services
- Government (national, state, local, tribal, and territorial)
- Healthcare and pathology services
- Hospitality
- Legal and court services
- Manufacturing (including automotive design and consumer goods)
- Media and publishing
- Non-profits
- Professional services and MSPs
- Retail
- Telecommunications
- Transportation (including airports)

### Regions

- Africa (notably Kenya)
- Asia (notably Japan and South Korea)
- Australia
- Europe (notably the United Kingdom and Italy)
- Latin America
- North America (United States and Canada — over half of named victims)

---

## Capabilities

Qilin is a professionally run, well-resourced RaaS with the full stack of modern intrusion capability: polished affiliate tooling, cross-platform encryptors, active R&D on the core payload, and integration of commodity and custom tooling. Its Rust-based "Qilin.B" variant (released around October 2024) adds stronger encryption, anti-recovery features, and improved defense evasion over the original Golang build. Affiliates have demonstrated the full kill-chain including initial access, credential theft, AD reconnaissance, privilege escalation (including process-token impersonation), lateral movement via PowerShell and remote-admin tools, data exfiltration, and destructive cleanup prior to encryption. 2025 campaigns introduced hybrid Windows/Linux intrusions against VMware vCenter and ESXi, including a Bring-Your-Own-Vulnerable-Driver (BYOVD) technique to disable endpoint protection before deploying the Linux encryptor. Vendor reporting attributes aggressive ransom demands (e.g., a reported USD 50 million demand against Synnovis) and persistent follow-through on data leaks when victims refuse payment.

### Signature TTPs

1. Initial access via phishing and spear-phishing, exploitation of exposed Citrix/RDP and edge devices, and abuse of valid credentials purchased or brokered from initial access brokers.
2. Exploitation of known vulnerabilities in internet-facing software, notably Veeam Backup & Replication (CVE-2023-27532) to harvest stored credentials, plus Microsoft Office (CVE-2021-40444, CVE-2022-30190 "Follina") and Fortinet FortiOS flaws.
3. Credential theft with Mimikatz and LSASS dumping, followed by process-token impersonation and pass-the-hash for privilege escalation.
4. Loader/post-exploitation via Cobalt Strike, SystemBC (for proxying and Tor tunnels), SmokeLoader, and the .NET-based NETXLOADER packed with .NET Reactor to complicate reverse engineering.
5. Lateral movement using PsExec, NetExec (formerly CrackMapExec), WinRM, RDP, and PowerShell remoting, including targeted pivoting into VMware vCenter/ESXi management planes.
6. Defense evasion through BYOVD to disable EDR, tampering with Windows Defender/AV, clearing event logs, and deleting Volume Shadow Copies before encryption.
7. Double extortion: large-scale data exfiltration prior to encryption, with victim naming and staged data leaks on Qilin's Tor DLS and clear-web "WikiLeaksV2" mirror.
8. Cross-platform deployment of Rust-based encryptors targeting Windows, Linux, and ESXi, with victim-specific configuration and selective file exclusion.

### Known Tools & Malware

- Agenda (original Go-based ransomware, July–September 2022)
- BYOVD loaders / vulnerable signed drivers (used to kill EDR in 2025 Linux campaigns)
- Cobalt Strike Beacon (command and control, lateral movement)
- Mimikatz (credential dumping)
- NetExec (formerly CrackMapExec — AD enumeration and lateral movement)
- NETXLOADER (.NET loader packed with .NET Reactor 6)
- PsExec (lateral execution)
- Qilin Go-based ransomware (post-rebrand, from September 2022)
- Qilin Rust-based ransomware / "Qilin.B" (from October 2024)
- Qilin Linux/ESXi encryptor
- PowerShell tooling for lateral movement and ESXi deployment
- RClone / WinSCP (data exfiltration, observed in incident response reports)
- SmokeLoader (staging and delivery)
- SystemBC / Coroxy (SOCKS proxy and Tor tunneling)
- WinRM (remote execution)

---

## MITRE ATT&CK Mapping

| ID | Technique | Tactic |
|---|---|---|
| [T1566](https://attack.mitre.org/techniques/T1566/) | Phishing | Initial Access |
| [T1190](https://attack.mitre.org/techniques/T1190/) | Exploit Public-Facing Application | Initial Access |
| [T1133](https://attack.mitre.org/techniques/T1133/) | External Remote Services | Initial Access |
| [T1078](https://attack.mitre.org/techniques/T1078/) | Valid Accounts | Initial Access / Privilege Escalation / Defense Evasion |
| [T1059](https://attack.mitre.org/techniques/T1059/) | Command and Scripting Interpreter (PowerShell) | Execution |
| [T1053](https://attack.mitre.org/techniques/T1053/) | Scheduled Task/Job | Execution / Persistence |
| [T1037](https://attack.mitre.org/techniques/T1037/) | Boot or Logon Initialization Scripts | Persistence |
| [T1547](https://attack.mitre.org/techniques/T1547/) | Boot or Logon Autostart Execution | Persistence |
| [T1134](https://attack.mitre.org/techniques/T1134/) | Access Token Manipulation | Privilege Escalation |
| [T1562](https://attack.mitre.org/techniques/T1562/) | Impair Defenses (incl. BYOVD) | Defense Evasion |
| [T1070](https://attack.mitre.org/techniques/T1070/) | Indicator Removal (log clearing) | Defense Evasion |
| [T1003](https://attack.mitre.org/techniques/T1003/) | OS Credential Dumping (LSASS / Mimikatz) | Credential Access |
| [T1021](https://attack.mitre.org/techniques/T1021/) | Remote Services (RDP, SMB, WinRM) | Lateral Movement |
| [T1210](https://attack.mitre.org/techniques/T1210/) | Exploitation of Remote Services | Lateral Movement |
| [T1570](https://attack.mitre.org/techniques/T1570/) | Lateral Tool Transfer | Lateral Movement |
| [T1091](https://attack.mitre.org/techniques/T1091/) | Replication Through Removable Media | Lateral Movement |
| [T1560](https://attack.mitre.org/techniques/T1560/) | Archive Collected Data | Collection |
| [T1071](https://attack.mitre.org/techniques/T1071/) | Application Layer Protocol | Command and Control |
| [T1090](https://attack.mitre.org/techniques/T1090/) | Proxy (SystemBC / Tor) | Command and Control |
| [T1048](https://attack.mitre.org/techniques/T1048/) | Exfiltration Over Alternative Protocol | Exfiltration |
| [T1490](https://attack.mitre.org/techniques/T1490/) | Inhibit System Recovery (Volume Shadow deletion) | Impact |
| [T1486](https://attack.mitre.org/techniques/T1486/) | Data Encrypted for Impact | Impact |
| [T1657](https://attack.mitre.org/techniques/T1657/) | Financial Theft (extortion) | Impact |

---

## Recent Activity

Qilin's activity curve is sharply upward over the full 2022–2026 window and particularly since early 2025. Vendor trackers recorded the group at roughly 84 victims in August–September 2025 alone, and Comparitech and Cybernews each tallied more than 700 victims during 2025, with Qilin listing its 700th attack of the year ahead of the fourth quarter and ultimately exceeding 1,000 confirmed victim posts by year-end — surpassing RansomHub's 547 from 2024 and making Qilin the most prolific ransomware brand of the year. Public reporting estimated Qilin-linked ransom payments of more than USD 50 million during 2024, with individual demands reaching as high as USD 50 million (Synnovis). By Q1 2026, Qilin had already named more than 50 additional victims and was publicly confirmed by threat trackers to be integrating AI agents into portions of its attack pipeline.

### Notable Campaigns and Incidents

- December 2023: Ransomware attack on Court Services Victoria (Australia), exposing video and audio recordings of court hearings and affecting records dating back to November 2023.
- June 3, 2024: Attack on Synnovis, the UK pathology services provider to NHS London trusts. Qilin reportedly demanded USD 50 million, leaked roughly 400 GB of data when unpaid, disrupted King's College Hospital and Guy's and St Thomas' Foundation Trusts, forced the cancellation of 1,134 planned operations and 2,194 outpatient appointments in the first 13 days, triggered a national call for O-negative/O-positive blood donations, exposed data on more than 900,000 patients, and was later officially named by Synnovis as a contributing factor in a patient death.
- October 2024: Release of "Qilin.B" Rust-based variant with improved encryption and anti-recovery.
- November 2024: Campaigns introducing NETXLOADER and expanded use of SmokeLoader.
- February 3, 2025: Cyberattack on Lee Enterprises disrupting more than 75 U.S. local newspapers; Qilin claimed the attack on February 27, 2025, and leaked roughly 350 GB of stolen data.
- Mid-2025: Hybrid Windows/Linux campaign featuring BYOVD and targeted ESXi/vCenter encryption; Kenya's Office of the Registrar of Political Parties listed; U.S. state/local/tribal/territorial (SLTT) share of MS-ISAC ransomware reporting jumped from 9% (Q1) to 24% (Q2), with Qilin overtaking RansomHub as the #1 SLTT threat.
- June 2025: Attack on Shinko Plastics (Japan).
- August 2025: Attacks on Nissan Creative Box and Osaki Medical (Japan); Qilin claimed theft of over 4 TB of design and strategy data from Nissan Creative Box.
- September 29, 2025: Attack on Asahi Group Holdings (Japan) disrupted domestic ordering, shipping, and customer service; Qilin listed Asahi on its DLS claiming exfiltration of 27 GB of financial, contractual, and employee data; subsequent reporting cited exposure of roughly 1.5 million records.
- 2025 ("Korean Leaks"): South Korean MSP breach used as a beachhead to compromise approximately 28 financial organizations.
- January 2026: Attacks claimed on Moen (U.S. faucet manufacturer) and Cressi (Italian dive-gear manufacturer).
- 2026: Tulsa International Airport listed on Qilin's DLS.
- Q1 2026: Vendor reporting confirms Qilin among the first RaaS brands to integrate AI agents into portions of their attack workflow (alongside Akira and Scattered Spider).

---

## Indicators of Compromise

Qilin affiliates rotate infrastructure frequently, so IP and domain indicators are of limited durable value; vendor threat-intel feeds should be used for live pivots. The most durable indicators from the search window are CVEs known to be exploited and the file-artifact families listed in tools.

### IP Addresses
_None identified in this collection run (see vendor-specific IOC bundles from Talos, Trend Micro, Huntress, Group-IB, Sophos)._

### Domains
Tor leak site and a clear-web mirror historically identified as "WikiLeaksV2" are referenced in vendor reporting but are rotated — _no current operational domain published here._

### File Hashes
_None collected in this run; defenders should pull hash sets from Talos, Trend Micro, Sophos, Huntress, and Group-IB Qilin/Agenda reports for both Go and Rust builds including "Qilin.B."_

### CVEs
- CVE-2021-40444 (Microsoft MSHTML remote code execution)
- CVE-2022-30190 ("Follina" — Microsoft Support Diagnostic Tool)
- CVE-2023-27532 (Veeam Backup & Replication credential disclosure)
- Fortinet FortiOS authentication bypass and out-of-bounds write vulnerabilities (multiple, as cited by vendor reporting; defenders should treat all unpatched FortiOS CVEs as in-scope)

---

## Source Articles

| # | Title | Source | Published | Recency |
|---|---|---|---|---|
| 1 | [Qilin Ransomware: Attack Methods and 2026 Status](https://cybelangel.com/blog/qilin-ransomware-tactics-attack/) | CybelAngel | 2026 | Recent |
| 2 | [Ransomware Gangs Now Use AI Agents — 3 Groups Named](https://aiautomationglobal.com/blog/ransomware-ai-agents-enterprise-cybersecurity-2026) | AI Automation Global | 2026 Q1 | Recent |
| 3 | [Tulsa International Airport listed in Qilin ransomware attack](https://cybernews.com/news/qilin-ransomware-tulsa-airport-cyberattack-2026/) | Cybernews | 2026 | Recent |
| 4 | [2025 was a record year for ransomware, with Qilin beating them all](https://cybernews.com/cybercrime/qilin-ransomware-record-year/) | Cybernews | 2026 | Recent |
| 5 | [Qilin Ransomware Cartel Strategic Threat Assessment (2022–2026)](https://falconfeeds.io/blogs/qilin-ransomware-cartel-strategic-threat-assessment-2022-2026) | FalconFeeds | 2026 | Recent |
| 6 | [Qilin Ransomware Explained](https://blog.qualys.com/vulnerabilities-threat-research/2025/06/18/qilin-ransomware-explained-threats-risks-defenses) | Qualys | 2025-06-18 | 2025 |
| 7 | [Qilin: Top Ransomware Threat to SLTTs in Q2 2025](https://www.cisecurity.org/insights/blog/qilin-top-ransomware-threat-to-sltts-in-q2-2025) | CIS / MS-ISAC | 2025 | 2025 |
| 8 | [Ransomware Threat Actor Profile: Qilin](https://www.kelacyber.com/blog/ransomware-threat-actor-profile-qilin/) | KELA Cyber | 2025 | 2025 |
| 9 | [Who Is the Qilin Ransomware Group?](https://www.kelacyber.com/blog/who-is-the-qilin-ransomware-group-and-how-do-they-operate/) | KELA Cyber | 2025 | 2025 |
| 10 | [Qilin Threat Actor Profile: TTPs, IOCs & Attacks](https://www.huntress.com/threat-library/threat-actors/qilin) | Huntress | 2025 | 2025 |
| 11 | [Threat Actor Profile: Qilin Ransomware Group](https://cyble.com/threat-actor-profiles/qilin-ransomware-group/) | Cyble | 2025 | 2025 |
| 12 | [Qilin Ransomware Analysis: Critical TTPs and Defense](https://www.picussecurity.com/resource/blog/qilin-ransomware) | Picus Security | 2025 | 2025 |
| 13 | [Qilin Ransomware (Agenda): A Deep Dive](https://www.checkpoint.com/cyber-hub/threat-prevention/ransomware/qilin-ransomware/) | Check Point | 2025 | 2025 |
| 14 | [Qilin Ransomware: Tactics, Attack Methods & Mitigation Strategies](https://www.group-ib.com/blog/qilin-ransomware/) | Group-IB | 2025 | 2025 |
| 15 | [The Evolution of Qilin RaaS](https://www.sans.org/blog/evolution-qilin-raas) | SANS Institute | 2025 | 2025 |
| 16 | [Qilin Threat Profile (TLP:CLEAR)](https://www.hhs.gov/sites/default/files/qilin-threat-profile-tlpclear.pdf) | HHS HC3 | 2025 | 2025 |
| 17 | [Qilin Ransomware Threat Profile](https://blackpointcyber.com/wp-content/uploads/2025/10/Qilin.pdf) | Blackpoint Cyber | 2025-10 | 2025 |
| 18 | [Qilin Ransomware: Analysis, Impact and Defense](https://www.blackfog.com/qilin-ransomware-analysis-impact-and-defense-2025/) | BlackFog | 2025 | 2025 |
| 19 | [Qilin Ransomware 2025: Hybrid Linux Attack Exposes BYOVD Risk](https://aviatrix.ai/threat-research-center/qilin-ransomware-2025-linux-byovd-hybrid-attack/) | Aviatrix | 2025 | 2025 |
| 20 | [Agenda Ransomware Group Adds SmokeLoader and NETXLOADER to Their Arsenal](https://www.trendmicro.com/en_us/research/25/e/agenda-ransomware-group-adds-smokeloader-and-netxloader-to-their.html) | Trend Micro | 2025 | 2025 |
| 21 | [Uncovering Qilin attack methods exposed through multiple cases](https://blog.talosintelligence.com/uncovering-qilin-attack-methods-exposed-through-multiple-cases/) | Cisco Talos | 2025 | 2025 |
| 22 | [Qilin Ransomware Activity Surges as Attacks Target Small Businesses](https://www.infosecurity-magazine.com/news/qilin-ransomware-activity-surges/) | Infosecurity Magazine | 2025 | 2025 |
| 23 | [Qilin ransomware gang claimed responsibility for the Lee Enterprises attack](https://securityaffairs.com/174831/data-breach/qilin-ransomware-group-claims-responsibility-lee-enterprises-attack.html) | Security Affairs | 2025-02 | 2025 |
| 24 | [Qilin ransomware claims attack at Lee Enterprises, leaks stolen data](https://www.bleepingcomputer.com/news/security/qilin-ransomware-claims-attack-at-lee-enterprises-leaks-stolen-data/) | BleepingComputer | 2025-02 | 2025 |
| 25 | [Qilin Ransomware Gang Claims Asahi Cyber-Attack](https://www.infosecurity-magazine.com/news/qilin-ransomware-asahi-cyber-attack/) | Infosecurity Magazine | 2025-10 | 2025 |
| 26 | [Qilin ransomware escalates rapidly in 2025, targeting critical sectors with 700 attacks](https://industrialcyber.co/ransomware/qilin-ransomware-escalates-rapidly-in-2025-targeting-critical-sectors-with-700-attacks-amid-ransomhub-shutdown/) | Industrial Cyber | 2025 | 2025 |
| 27 | [Asahi Group Holdings Ransomware Attack: Qilin Breach](https://www.rescana.com/post/asahi-group-holdings-ransomware-attack-qilin-breach-disrupts-japanese-operations-and-exposes-1-5-mi) | Rescana | 2025 | 2025 |
| 28 | [Qilin Ransomware Exploits South Korean MSP Breach](https://www.rescana.com/post/qilin-ransomware-exploits-south-korean-msp-breach-in-korean-leaks-attack-impacting-28-financial-org) | Rescana | 2025 | 2025 |
| 29 | [Synnovis notifies of data breach after 2024 ransomware attack](https://www.bleepingcomputer.com/news/security/synnovis-notifies-of-data-breach-after-2024-ransomware-attack/) | BleepingComputer | 2025 | 2025 |
| 30 | [Patient Death Linked to Ransomware Attack on Pathology Services Provider](https://www.hipaajournal.com/patient-death-linked-to-ransomware-attack/) | HIPAA Journal | 2025 | 2025 |
| 31 | [Victoria court recordings exposed in reported ransomware attack](https://www.bleepingcomputer.com/news/security/victoria-court-recordings-exposed-in-reported-ransomware-attack/) | BleepingComputer | 2024-01 | 2024 |
| 32 | [Victorian court systems allegedly breached by Qilin ransomware gang](https://www.cyberdaily.au/security/9983-victorian-court-systems-allegedly-breached-by-qilin-ransomware-gang) | Cyber Daily | 2024-01 | 2024 |
| 33 | [Ransomware Spotlight: Agenda](https://www.trendmicro.com/vinfo/us/security/news/ransomware-spotlight/ransomware-spotlight-agenda) | Trend Micro | 2022–2023 | Historical |
| 34 | [Qilin (cybercrime group) — Wikipedia](https://en.wikipedia.org/wiki/Qilin_(cybercrime_group)) | Wikipedia | Continuously updated | Reference |

---

## Analyst Notes

Qilin's three-year arc from Agenda's July 2022 debut through its dominant 2025 and rapidly accelerating early 2026 is an unusually clean illustration of how modern RaaS ecosystems consolidate. The brand invested visibly in the affiliate experience — a polished panel, aggressive 80–85% payouts, cross-platform payloads, and continual R&D (Qilin.B in late 2024, NETXLOADER tooling, the 2025 BYOVD Linux push) — and those investments paid off in affiliate migration whenever a rival RaaS collapsed, most notably the RansomHub shutdown in 2025. Reporting that Scattered Spider affiliates and actors aligned with North Korean operators have adopted Qilin suggests the brand is on its way to becoming a cross-community "tool of choice," which increases the diversity of intrusion styles defenders should expect under the Qilin banner.

The Synnovis case is the most important single data point in the dossier because it marks Qilin's formal association with directly attributable patient harm, including a patient death. Expect this to drive more aggressive law-enforcement and regulator attention, especially in the UK and EU, and more willingness by healthcare victims to disclose publicly. The Lee Enterprises, Asahi, Nissan Creative Box, Moen, Cressi, and Tulsa Airport cases collectively show the cross-sector and cross-region reach, with Japan and Australia noticeably more visible in the 2024–2026 window than they were in early Qilin activity.

Defenders should prioritize: patching the CVEs listed above (particularly Veeam CVE-2023-27532 and the FortiOS family); hardening VMware vCenter/ESXi management planes, offline immutable backups, and driver allowlisting against BYOVD; detecting SystemBC/Cobalt Strike/SmokeLoader/NETXLOADER staging; restricting WinRM/PsExec/NetExec lateral-movement paths; enforcing phishing-resistant MFA on all external remote services (Citrix, RDP gateways, VPN); and monitoring for large outbound transfers to cloud storage and RClone/WinSCP usage ahead of encryption events. Given confirmed AI-agent integration reporting in Q1 2026, defenders should also assume that future Qilin intrusions may compress the dwell-time between initial access and encryption compared to historical baselines.

This dossier was built primarily from open-source reporting because the RSS trawler used by the profiler skill could not reach vendor feeds from the current sandbox; direct web searches against Google were used instead. Rotating infrastructure and fast-turnover affiliate tradecraft mean volatile indicators (IPs, domains, hashes) should be pulled from live vendor feeds rather than from this document.

---

## Profile Metadata

| Field | Value |
|---|---|
| **Skill Version** | threat-actor-profiler v1.1 |
| **Queries Run** | 6 (web search, multi-angle) |
| **Articles Retrieved** | 60+ |
| **Articles Analyzed** | 34 |
| **Articles Skipped** | 0 |
| **Profile Generated** | 2026-04-10 14:00:00 UTC |
| **Last Consolidated** | _Not yet consolidated_ |
| **Updates Merged** | 0 |
| **Collection Method Note** | RSS trawler blocked by sandbox egress; switched to authenticated web search against reputable vendor and press sources. |
