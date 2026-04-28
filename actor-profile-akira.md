# Threat Actor Profile: Akira

**Generated:** 2026-04-28 10:45:00 UTC
**Search Window:** 2023-03-01 → 2026-04-28
**Sources Analyzed:** 18 articles / advisories
**Confidence:** 🟢 High — 10+ corroborating sources including FBI, CISA, MITRE ATT&CK, Palo Alto Unit42, Cisco Talos, IBM X-Force, TRM Labs, and independent security firms

---

## Overview

Akira is one of the most prolific and financially devastating ransomware-as-a-service (RaaS) operations active since March 2023. In under three years, the group has claimed over 1,600 victims across North America, Europe, and Australia, and extracted at least $245 million USD in ransom payments through September 2025. Operating a closed affiliate model built on double extortion — simultaneous data theft and encryption — Akira has distinguished itself through relentless technical evolution, a multi-platform malware arsenal (Windows, Linux/ESXi, Nutanix AHV), and an unusually compressed attack tempo: affiliates have been observed initiating lateral movement within six minutes of initial access and completing full data exfiltration in under two hours. Attribution analysis by TRM Labs, Palo Alto Unit42, and others places the developer core in Russia or the broader post-Soviet region, with code and operational overlaps pointing to lineage from the defunct Conti ecosystem. Despite its scale, Akira has not been subject to OFAC sanctions or criminal indictments as of the profile date.

---

## Identity

| Field | Value |
|---|---|
| **Primary Name** | Akira |
| **Known Aliases** | Storm-1567 (Microsoft), Howling Scorpius (Palo Alto Unit42), Punk Spider (CrowdStrike), Gold Sahara (Secureworks) |
| **MITRE ATT&CK ID** | [G1024](https://attack.mitre.org/groups/G1024/) |
| **Suspected Origin** | Russia / post-Soviet region |
| **Active Since** | March 2023 |
| **Primary Motivation** | 💰 Financial |
| **Operating Model** | Ransomware-as-a-Service (RaaS), closed affiliate program |
| **Ransomware Lineage** | Likely Conti ecosystem successor (code/TTP overlaps) |

---

## Targeting

Akira demonstrates broad sector targeting with a preference for mid-market organizations that lack mature security operations. Victims are concentrated in the United States and Canada, with meaningful secondary presence in Brazil, Australia, and Western Europe. No single vertical is exclusively targeted, though manufacturing, legal and professional services, and construction/engineering have dominated victim lists since mid-2024.

### Sectors

- Construction & Engineering
- Critical Infrastructure (Energy, Water, Transportation)
- Education
- Financial Services
- Healthcare
- Information Technology
- Legal & Professional Services
- Manufacturing
- Retail & Hospitality

### Regions

- Australia
- Brazil
- Canada
- Europe (UK, Germany, France, Netherlands, and others)
- United States (primary concentration)

---

## Capabilities

Akira operates with a high degree of technical sophistication, maintaining a multi-variant malware ecosystem and regularly rotating tools and techniques to stay ahead of detection and threat intelligence. The group has demonstrated capability against hypervisor environments — VMware ESXi (from April 2023), Microsoft Hyper-V, and Nutanix AHV (first confirmed June 2025) — indicating deliberate investment in virtualization attack research. Ransomware demands have ranged from $200,000 to $4 million per incident, calibrated to victim revenue. The group's speed of execution — lateral movement in under six minutes and exfiltration in under two hours in documented cases — suggests experienced affiliates with rehearsed playbooks and access to pre-positioned tooling.

### Signature TTPs

1. **Edge device exploitation for initial access** — Systematic targeting of unpatched VPN appliances (Cisco ASA/FTD, SonicWall SSL VPN) using known CVEs, often combined with credential stuffing
2. **Double extortion with leak site pressure** — Data exfiltration always precedes encryption; victims who refuse payment face publication on Akira's Tor-hosted leak site
3. **Lightning-fast dwell time** — Lateral movement documented within 6 minutes of initial access; full data exfiltration achieved in as little as 2 hours and 10 minutes (CISA, November 2025)
4. **Kerberoasting and LSASS dumping** — Primary credential harvesting methods post-foothold, enabling rapid privilege escalation to domain admin
5. **Multi-platform hypervisor targeting** — Dedicated ESXi, Hyper-V, and Nutanix AHV encryptors maximize organizational impact by targeting virtualized infrastructure directly
6. **Continuous malware evolution** — Four distinct malware generations in three years (original C++ → Megazord Rust → Akira_v2 Rust ESXi → return-to-C++ in late 2024) demonstrate active development capability
7. **Living-off-the-land exfiltration** — Legitimate tools (rclone, FileZilla, WinSCP) used for data staging and transfer to blend with normal network activity

### Known Tools & Malware

- **AdFind** — Active Directory enumeration
- **Advanced IP Scanner** — Internal network discovery
- **Akira (S1129)** — Primary Windows encryptor, C++, appends `.akira` extension, ransom note `akira_readme.txt`
- **Akira Linux/ESXi variant** — C++ ESXi-targeting encryptor, appends `.akiranew` extension, ransom note `akiranew.txt` (from April 2023)
- **Akira_v2 (S1194)** — Rust-based ESXi encryptor, emerged March 2024, uses rust-crypto library
- **FileZilla** — FTP-based data exfiltration
- **MASSCAN** — Rapid network port scanning
- **Megazord** — Rust-based Windows encryptor, appends `.powerranges` extension, active August 2023 – December 2024
- **Mimikatz** — Credential dumping from LSASS
- **Ngrok** — Encrypted C2 tunneling to bypass perimeter monitoring
- **Nltest** — Domain trust and domain controller enumeration
- **PowerTool** — Disabling AV/EDR processes
- **rclone** — Cloud-based data exfiltration (primary exfil tool)
- **WinRAR** — Data archiving prior to exfiltration
- **WinSCP** — SFTP-based data exfiltration

---

## MITRE ATT&CK Mapping

| ID | Technique | Tactic |
|---|---|---|
| [T1027.001](https://attack.mitre.org/techniques/T1027/001/) | Binary Padding | Defense Evasion |
| [T1036.005](https://attack.mitre.org/techniques/T1036/005/) | Masquerading: Match Legitimate Name or Location | Defense Evasion |
| [T1078](https://attack.mitre.org/techniques/T1078/) | Valid Accounts | Initial Access, Persistence, Privilege Escalation |
| [T1003.001](https://attack.mitre.org/techniques/T1003/001/) | OS Credential Dumping: LSASS Memory | Credential Access |
| [T1018](https://attack.mitre.org/techniques/T1018/) | Remote System Discovery | Discovery |
| [T1021.001](https://attack.mitre.org/techniques/T1021/001/) | Remote Services: Remote Desktop Protocol | Lateral Movement |
| [T1047](https://attack.mitre.org/techniques/T1047/) | Windows Management Instrumentation | Execution |
| [T1048](https://attack.mitre.org/techniques/T1048/) | Exfiltration Over Alternative Protocol (FTP/SFTP) | Exfiltration |
| [T1071.001](https://attack.mitre.org/techniques/T1071/001/) | Application Layer Protocol: Web Protocols | Command and Control |
| [T1110](https://attack.mitre.org/techniques/T1110/) | Brute Force | Credential Access |
| [T1133](https://attack.mitre.org/techniques/T1133/) | External Remote Services | Initial Access, Persistence |
| [T1136](https://attack.mitre.org/techniques/T1136/) | Create Account | Persistence |
| [T1190](https://attack.mitre.org/techniques/T1190/) | Exploit Public-Facing Application | Initial Access |
| [T1213.002](https://attack.mitre.org/techniques/T1213/002/) | Data from Information Repositories: SharePoint | Collection |
| [T1482](https://attack.mitre.org/techniques/T1482/) | Domain Trust Discovery | Discovery |
| [T1486](https://attack.mitre.org/techniques/T1486/) | Data Encrypted for Impact | Impact |
| [T1489](https://attack.mitre.org/techniques/T1489/) | Service Stop | Impact |
| [T1490](https://attack.mitre.org/techniques/T1490/) | Inhibit System Recovery | Impact |
| [T1531](https://attack.mitre.org/techniques/T1531/) | Account Access Removal | Impact |
| [T1558](https://attack.mitre.org/techniques/T1558/) | Steal or Forge Kerberos Tickets (Kerberoasting) | Credential Access |
| [T1560.001](https://attack.mitre.org/techniques/T1560/001/) | Archive Collected Data: Archive via Utility (WinRAR) | Collection |
| [T1562.001](https://attack.mitre.org/techniques/T1562/001/) | Impair Defenses: Disable or Modify Tools | Defense Evasion |
| [T1567.002](https://attack.mitre.org/techniques/T1567/002/) | Exfiltration Over Web Service: Exfiltration to Cloud Storage | Exfiltration |
| [T1572](https://attack.mitre.org/techniques/T1572/) | Protocol Tunneling (Ngrok) | Command and Control |
| [T1657](https://attack.mitre.org/techniques/T1657/) | Financial Theft (double extortion) | Impact |

---

## Recent Activity

Akira's operational tempo increased dramatically from 2024 into 2025, with the group claiming approximately twice as many victims in the first half of 2025 as it did in all of 2024. By year-end 2025, Akira had claimed 620 victims for the year and crossed $245 million USD in cumulative ransom proceeds — the highest ransom revenue of any ransomware strain in 2025, nearly double its nearest competitor. Activity in Q1 2026 showed a modest 22% decline from Q4 2025 (176 vs. 226 victims), though March 2026 saw a resurgence with 84 new victims, the group's second-highest single-month total on record. A November 2025 CISA advisory update explicitly flagged Akira as presenting an "imminent threat to critical infrastructure," marking an escalation in government concern.

### Notable Campaigns & Incidents

- **SonicWall SSL VPN campaign (CVE-2024-40766)** — Systematic exploitation of SonicWall SSLVPN appliances beginning late July 2025, ongoing through 2025
- **Nutanix AHV encryption (June 2025)** — First confirmed successful encryption of Nutanix AHV virtual disk files, expanding beyond VMware ESXi and Hyper-V
- **November 2025 surge** — 73 victims published in a single month; 35+ appeared on the leak site in a single day
- **RJS Corporation attack (January 7, 2026)** — Claimed by Akira on leak site
- **Los Angeles law firm (January 13, 2026)** — 10GB of sensitive legal data exfiltrated and threatened for publication
- **Apache OpenOffice claim (2025)** — Akira claimed a breach of the Apache OpenOffice project; disputed by Apache (no evidence found)
- **Cisco ASA/FTD campaign (September 2023)** — Widespread exploitation of CVE-2023-20269 zero-day; Cisco issued emergency advisory

---

## Indicators of Compromise

> **Note:** Akira IOCs rotate frequently across affiliates. The most comprehensive and current IOC tables (IP addresses, domains, file hashes) are maintained in the joint CISA/FBI advisory **AA24-109A** (last updated November 13, 2025) at https://www.cisa.gov/news-events/cybersecurity-advisories/aa24-109a and in the IC3/CISA joint release at https://www.ic3.gov/CSA/2025/251113.pdf

### CVEs Actively Exploited

- **CVE-2020-3259** — Cisco VPN information disclosure
- **CVE-2023-20269** — Cisco ASA/FTD remote access VPN zero-day (exploited Sept 2023)
- **CVE-2024-40766** — SonicWall SonicOS SSL VPN authentication bypass (active July 2025+)
- **Veeam backup server CVEs** — Unspecified, flagged in 2025 CISA advisory update

### File System Artifacts

- `.akira` — Encrypted file extension (Windows, original and 2024 C++ variant)
- `.akiranew` — Encrypted file extension (Linux/ESXi variant)
- `.powerranges` — Encrypted file extension (Megazord Rust variant, Aug 2023 – Dec 2024)
- `akira_readme.txt` — Ransom note (Windows variants)
- `akiranew.txt` — Ransom note (Linux/ESXi variants)

### IP Addresses
_See CISA Advisory AA24-109A Table 2 for confirmed C2 IP indicators_

### Domains
_See CISA Advisory AA24-109A Table 3 for confirmed C2 domains_

### File Hashes
_See CISA Advisory AA24-109A Tables 4–9 for confirmed sample hashes by variant_

---

## Source Articles

| # | Title | Source | Published | Recency |
|---|---|---|---|---|
| 1 | [#StopRansomware: Akira Ransomware (AA24-109A)](https://www.cisa.gov/news-events/cybersecurity-advisories/aa24-109a) | CISA/FBI/IC3 | April 2024; updated November 2025 | Recent |
| 2 | [Akira Ransomware Group: Threat Profile and TTPs](https://www.trmlabs.com/resources/intel-library/akira) | TRM Labs | 2024–2025 | Recent |
| 3 | [Akira Ransomware: The Conti Successor Targeting the West](https://cybelangel.com/blog/akira-ransomware-conti-successor-russian-threat-actor-profile/) | CybelAngel | 2024 | Recent |
| 4 | [Akira Threat Actor Profile](https://quorumcyber.com/threat-actors/akira-threat-actor-profile/) | Quorum Cyber | 2024 | Recent |
| 5 | [Akira Ransomware Threat Actor Profile](https://www.huntress.com/threat-library/threat-actors/akira) | Huntress | 2024–2025 | Recent |
| 6 | [A Spotlight on Akira Ransomware](https://www.ibm.com/think/x-force/spotlight-akira-ransomware-x-force) | IBM X-Force | 2024 | Recent |
| 7 | [Threat Assessment: Howling Scorpius (Akira Ransomware)](https://unit42.paloaltonetworks.com/threat-assessment-howling-scorpius-akira-ransomware/) | Palo Alto Unit42 | 2024 | Recent |
| 8 | [Akira Ransomware Continues to Evolve](https://blog.talosintelligence.com/akira-ransomware-continues-to-evolve/) | Cisco Talos | March 2024 | Recent |
| 9 | [Akira, Group G1024](https://attack.mitre.org/groups/G1024/) | MITRE ATT&CK | 2024–2025 | Recent |
| 10 | [Akira Ransomware: Attack Methods, IOCs and Defence 2026](https://cybelangel.com/blog/the-akira-ransomware-playbook-everything-you-need-to-know/) | CybelAngel | 2026 | Recent |
| 11 | [Ransomware Spotlight: Akira](https://www.trendmicro.com/vinfo/us/security/news/ransomware-spotlight/ransomware-spotlight-akira) | Trend Micro | 2024 | Recent |
| 12 | [Akira Ransomware: November 2025, an Alarming Escalation](https://sosransomware.com/en/ransomware-en/akira-ransomware-november-2025-an-alarming-escalation-in-attacks/) | SOS Ransomware | November 2025 | Recent |
| 13 | [Akira Becomes Second Most Active Ransomware Clan](https://cybernews.com/security/major-threat-akira-ransomware-crosses-250m-dollars/) | CyberNews | 2025 | Recent |
| 14 | [Akira SonicWall Campaign Uncovered](https://www.darktrace.com/blog/inside-akiras-sonicwall-campaign-darktraces-detection-and-response) | Darktrace | 2025 | Recent |
| 15 | [Akira Ransomware 2025: Updated CISA Advisory, TTPs, and Defense Strategies](https://www.picussecurity.com/resource/blog/akira-ransomware-analysis-simulation-and-mitigation-cisa-alert-aa24-109a) | Picus Security | 2025 | Recent |
| 16 | [Akira Ransomware: Attack Trends & Defense](https://blog.qualys.com/vulnerabilities-threat-research/2024/10/02/threat-brief-understanding-akira-ransomware) | Qualys | October 2024 | Recent |
| 17 | [Ransomware and Cyber Extortion in Q1 2026](https://reliaquest.com/blog/threat-spotlight-ransomware-and-cyber-extortion-in-q1-2026/) | ReliaQuest | Q1 2026 | Recent |
| 18 | [Akira Develops Rust-Based Ransomware to Target ESXi Servers](https://ransomwareattacks.halcyon.ai/news/akira-develops-rust-based-ransomware-to-target-esxi-servers) | Halcyon | 2024 | Recent |

---

## Analyst Notes

**Attribution confidence:** Medium-High. Multiple independent sources (TRM Labs, Palo Alto Unit42) assess Russian/post-Soviet origin based on language artifacts in code comments, dark web forum activity conducted in Russian, and non-VPN IP observations tied to Russia. However, definitive attribution to specific individuals or state nexus has not been publicly confirmed. Akira has not been linked to Russian state-directed campaigns; the consensus assessment is financially motivated criminality operating from a permissive jurisdiction.

**Conti lineage hypothesis:** Code-level similarities and operational overlaps (tooling, infrastructure patterns, affiliate recruitment tactics) strongly suggest former Conti members or contractors contributed to Akira's development. This hypothesis is widely held but not conclusively proven; it is possible shared tooling reflects marketplace purchases rather than direct personnel continuity.

**Affiliate heterogeneity:** As a RaaS operation, Akira's TTPs vary across affiliates. The "lightning fast" intrusion timelines may reflect a small number of elite affiliates rather than being representative. Defenders should not assume all Akira intrusions will move at this pace.

**Megazord retirement:** Megazord (the Rust-based Windows variant) appears to have fallen out of active use since approximately December 2024. Defenders should not rely on Megazord-specific detections as a primary indicator.

**IOC volatility:** Akira has undergone at least four distinct evolutions in post-payment laundering tactics and regularly rotates C2 infrastructure. Static IP/domain IOCs degrade rapidly. Behavioral detections (credential dumping, rclone activity, Ngrok tunneling, specific ransom note filenames) provide more durable coverage.

**Law enforcement status:** No OFAC sanctions, Europol disruption operations, or criminal indictments have been publicly announced against Akira operators as of April 2026, despite significant international cooperation (FBI, CISA, Europol EC3, German authorities, French OFAC, Dutch NCSC) on intelligence sharing.

**Intelligence gap:** Akira's exact affiliate count and recruitment pipeline are unknown. The closed RaaS model limits affiliate-side visibility that was available with more open operations (e.g., LockBit). Victim counts on the leak site are likely undercounts, as many organizations pay quietly.

---

## Profile Metadata

| Field | Value |
|---|---|
| **Skill Version** | threat-actor-profiler v1.1 |
| **Queries Run** | 6 (4 OSINT + 2 supplemental web searches) |
| **Articles Retrieved** | 18 |
| **Articles Analyzed** | 18 |
| **Articles Skipped** | 0 |
| **Profile Generated** | 2026-04-28 10:45:00 UTC |
| **Last Consolidated** | _Not yet consolidated_ |
| **Updates Merged** | 0 |
