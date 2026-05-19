# Threat Actor Profile: Lynx

**Generated:** 2026-04-29 10:45:00 UTC
**Search Window:** 2024-07-01 → 2026-04-29
**Sources Analyzed:** 15+ articles across 7 search queries
**Confidence:** 🟢 High — Multiple authoritative sources (Unit42, Rapid7, FortiGuard, Group-IB, Darktrace, CIS, Blackpoint Cyber, DFIR Report) with consistent findings across technical and contextual dimensions.

---

## Overview

Lynx is a Ransomware-as-a-Service (RaaS) operation that emerged in July 2024 and rapidly established itself as a high-volume, financially motivated threat. The group is widely assessed with high confidence to be a direct rebrand of the INC ransomware operation, having acquired INC's source code — circulated on RAMP underground forums for approximately $300,000 in May 2024 — and repackaged it with minor enhancements under the Lynx brand. Lynx employs a classic double-extortion model: encrypting victims' data while simultaneously exfiltrating it and threatening publication on a dedicated dark-web leak site. The group claims to self-impose "ethical" constraints (avoiding government institutions, hospitals, and non-profits), though reported incidents in energy infrastructure and adjacent sectors demonstrate a willingness to target critical industries. From under 25 victims in its first months, Lynx scaled to over 393 confirmed victims by late March 2026, demonstrating aggressive growth and a maturing affiliate recruitment pipeline.

---

## Identity

| Field | Value |
|---|---|
| **Primary Name** | Lynx |
| **Known Aliases** | LynxLocker, Lynx RaaS |
| **Suspected Origin** | _Unknown_ (RaaS model with distributed affiliates; no credible nation-state attribution) |
| **Active Since** | July 2024 |
| **Primary Motivation** | 💰 Financial |

---

## Targeting

Lynx's targeting profile reflects opportunistic affiliate-driven operations with a notable concentration in North America and Western Europe. The group targets mid-market to large enterprises across multiple verticals, with manufacturing, business services, and energy sectors facing the highest observed victim counts. The group's self-declared "ethical" posture — avoiding hospitals, governments, and non-profits — has not been consistently upheld, and energy sector and infrastructure victims have been confirmed.

### Sectors

- Agriculture
- Architecture & Engineering
- Business Services
- Construction
- Education
- Energy, Oil & Gas / Utilities
- Environmental Services
- Finance & Financial Services
- Healthcare (limited; claimed to avoid but incidents reported)
- Legal Services
- Manufacturing
- Real Estate
- Retail
- Technology
- Tourism & Hospitality
- Transportation

### Regions

- Asia-Pacific (APAC)
- Australia
- Canada
- Europe (multiple countries)
- Germany
- Latin America
- Middle East
- Sub-Saharan Africa
- United Kingdom
- United States (primary — represents majority of confirmed victims)

---

## Capabilities

Lynx demonstrates moderate-to-high technical sophistication consistent with a maturing RaaS operation leveraging adapted, proven ransomware code. The core encryption routine — AES-128 in CTR mode with Curve25519 Donna for key exchange — is cryptographically sound and effectively precludes victim-side decryption without the threat actor's private key. Both Windows and Linux (including ESXi-targeted) builds exist, enabling the group to attack virtualized environments and maximize encryption scope across heterogeneous enterprise infrastructure. The affiliate panel infrastructure, infiltrated by Group-IB researchers, reveals a professional operational setup with victim management, dedicated negotiation chat functionality, publication scheduling for the leak site, and a pre-built binary archive covering Windows, Linux, and niche architectures.

### Signature TTPs

1. **Phishing for initial access** — Spear-phishing emails or credential theft via phishing pages are the primary documented initial access vector.
2. **Valid account abuse** — Compromised credentials used to authenticate to VPN solutions and remote access services for persistent access.
3. **Living-off-the-land reconnaissance** — AdFind used for Active Directory enumeration; Windows built-ins (Query Registry, Remote System Discovery, Network Share Discovery) used for environment mapping.
4. **Remote execution via PsExec** — Lateral movement and ransomware deployment across networked systems using PsExec and Windows Command Shell.
5. **Data exfiltration via Rclone** — Large volumes of data (typically 30GB+ per incident) staged and exfiltrated to attacker-controlled storage prior to encryption.
6. **Process termination pre-encryption** — CreateToolhelp32Snapshot / TerminateProcess used to kill security tools, databases, and backup processes before encryption begins.
7. **Double extortion with dedicated leak site** — Encrypted victim data published on Tor-hosted DLS if ransom is unpaid; ransom demands documented up to $18.1M per incident.

### Known Tools & Malware

- AdFind (Active Directory reconnaissance)
- AnyDesk (remote access / lateral movement enablement)
- Lynx ransomware binary — Windows EXE (x86/x64)
- Lynx ransomware binary — Linux/ESXi build
- PsExec (remote command execution)
- Rclone (data exfiltration to cloud/remote storage)

---

## MITRE ATT&CK Mapping

| ID | Technique | Tactic |
|---|---|---|
| [T1012](https://attack.mitre.org/techniques/T1012/) | Query Registry | Discovery |
| [T1018](https://attack.mitre.org/techniques/T1018/) | Remote System Discovery | Discovery |
| [T1059.001](https://attack.mitre.org/techniques/T1059/001/) | PowerShell | Execution |
| [T1059.003](https://attack.mitre.org/techniques/T1059/003/) | Windows Command Shell | Execution |
| [T1078](https://attack.mitre.org/techniques/T1078/) | Valid Accounts | Defense Evasion / Persistence / Privilege Escalation / Initial Access |
| [T1082](https://attack.mitre.org/techniques/T1082/) | System Information Discovery | Discovery |
| [T1087](https://attack.mitre.org/techniques/T1087/) | Account Discovery (via AdFind) | Discovery |
| [T1090.003](https://attack.mitre.org/techniques/T1090/003/) | Multi-hop Proxy (Tor C2) | Command and Control |
| [T1135](https://attack.mitre.org/techniques/T1135/) | Network Share Discovery | Discovery |
| [T1486](https://attack.mitre.org/techniques/T1486/) | Data Encrypted for Impact | Impact |
| [T1489](https://attack.mitre.org/techniques/T1489/) | Service Stop (process termination pre-encryption) | Impact |
| [T1490](https://attack.mitre.org/techniques/T1490/) | Inhibit System Recovery (VSS deletion) | Impact |
| [T1491.001](https://attack.mitre.org/techniques/T1491/001/) | Internal Defacement (ransom wallpaper change) | Impact |
| [T1566](https://attack.mitre.org/techniques/T1566/) | Phishing | Initial Access |
| [T1567](https://attack.mitre.org/techniques/T1567/) | Exfiltration Over Web Service (Rclone) | Exfiltration |

---

## Recent Activity

Lynx has maintained high operational tempo throughout 2025 and into 2026. From an initial base of ~20 victims in September 2024, confirmed victim counts reached 96 by January 2025, ~300 by August 2025, and over 393 by late March 2026. The group continues to recruit affiliates through RAMP and qTox channels, offering an 80/20 profit split favoring affiliates. In early 2026, Blackpoint Cyber published a dedicated threat profile indicating Lynx remains actively maintained and expanding, with new binary variants and ongoing infrastructure operations. Energy and utility sector organizations across the United States remain a high-priority targeting vertical.

### Notable Campaigns

- **Electrica (Romania)** — December 2024: Attack on major Romanian energy supplier disrupting operations and exfiltrating sensitive data.
- **DZS Inc. (USA)** — Late 2024: Attack on global network solutions provider; ~30GB data exfiltrated; $18.1M ransom demanded.
- **Regis Resources subsidiary** — Date unconfirmed 2024/2025: Claimed on Lynx dark web leak site.
- **US Energy/Oil & Gas facilities (multiple)** — July–November 2024: Multiple claimed victims in the US energy sector across the initial operating period.

---

## Indicators of Compromise

### IP Addresses
74.91.125[.]57 *(associated with thetavaluemetrics[.]com C2, observed by Darktrace, late 2025)*

### Domains
thetavaluemetrics[.]com *(rare external endpoint observed during Lynx intrusion, late 2025)*

### File Hashes
`eaa0e773eb593b0046452f420b6db8a47178c09e6db0fa68f6a2d42c3f48e3bc` *(SHA256 — earliest known Lynx sample, July 2024)*
`571f5de9dd0d509ed7e5242b9b7473c2b2cbb36ba64d38b32122a0a337d6cf8b` *(SHA256 — Windows EXE sample)*
`6e65483764d7c25523a5bbef5be99eb42349eef39d5517c46b3a4af262a80ceb` *(SHA256 — additional Lynx binary)*

### CVEs
_None publicly attributed to Lynx at time of generation. Initial access appears credential/phishing-based rather than CVE exploitation._

### Additional Host-Based Indicators
- Files encrypted with `.lynx` extension appended
- Ransom note dropped as `README.txt` in affected directories
- Desktop wallpaper replaced with Lynx ransom message
- Volume Shadow Copies deleted (inhibit recovery)
- Unusual outbound traffic to Tor exit nodes / large data transfers (exfiltration via Rclone)

---

## Source Articles

| # | Title | Source | Published | Recency |
|---|---|---|---|---|
| 1 | [Lynx Ransomware: A Rebranding of INC Ransomware](https://unit42.paloaltonetworks.com/inc-ransomware-rebrand-to-lynx/) | Palo Alto Unit42 | 2024-09 | Recent |
| 2 | [Ransomware Groups Demystified: Lynx Ransomware](https://www.rapid7.com/blog/post/2024/09/12/ransomware-groups-demystified-lynx-ransomware/) | Rapid7 | 2024-09-12 | Recent |
| 3 | [Ransomware Roundup – Lynx](https://www.fortinet.com/blog/threat-research/ransomware-roundup-lynx) | FortiGuard Labs | 2024 | Recent |
| 4 | [New Threat on the Prowl: Investigating Lynx Ransomware](https://www.darktrace.com/blog/new-threat-on-the-prowl-investigating-lynx-ransomware) | Darktrace | 2024–2025 | Recent |
| 5 | [Dark Web Profile: Lynx Ransomware](https://socradar.io/blog/dark-web-profile-lynx-ransomware/) | SOCRadar | 2024–2025 | Recent |
| 6 | [Lynx Ransomware Pouncing on Utilities](https://www.cisecurity.org/insights/blog/lynx-ransomware-pouncing-utilities) | CIS (Center for Internet Security) | 2025 | Recent |
| 7 | [Cat's out of the bag: Lynx Ransomware-as-a-Service](https://www.group-ib.com/blog/cat-s-out-of-the-bag-lynx-ransomware/) | Group-IB | 2024–2025 | Recent |
| 8 | [Lynx Ransomware Group Unveiled with Sophisticated Affiliate Program](https://www.infosecurity-magazine.com/news/lynx-ransomware-sophisticated/) | Infosecurity Magazine | 2024–2025 | Recent |
| 9 | [In-Depth Analysis of Lynx Ransomware](https://www.nextron-systems.com/2024/10/11/in-depth-analysis-of-lynx-ransomware/) | Nextron Systems | 2024-10-11 | Recent |
| 10 | [Lynx Ransomware: Exposing How INC Ransomware Rebrands Itself](https://www.picussecurity.com/resource/blog/lynx-ransomware) | Picus Security | 2024–2025 | Recent |
| 11 | [Cat's Got Your Files: Lynx Ransomware](https://thedfirreport.com/2025/11/17/cats-got-your-files-lynx-ransomware/) | DFIR Report | 2025-11-17 | Recent |
| 12 | [Lynx Ransomware THREAT PROFILE](https://blackpointcyber.com/wp-content/uploads/2026/02/Lynx.pdf) | Blackpoint Cyber | 2026-02 | Recent |
| 13 | [Lynx Ransomware Group Adds Affiliates to 'Industrialize'](https://www.darkreading.com/threat-intelligence/lynx-raas-group-industrializes-cybercrime-with-affiliate-operations) | Dark Reading | 2025 | Recent |
| 14 | [MOXFIVE Threat Actor Spotlight: Lynx](https://www.moxfive.com/resources/moxfive-threat-actor-spotlight-lynx) | MOXFIVE | 2025 | Recent |
| 15 | [Unraveling Lynx Ransomware](https://www.loginsoft.com/post/unraveling-lynx-ransomware) | Loginsoft | 2024–2025 | Recent |

---

## Analyst Notes

**INC Rebrand Attribution (High Confidence):** The INC ransomware lineage is established with high confidence. Multiple independent technical analyses (Unit42, Picus, Nextron) confirm >90% source code overlap between Lynx and INC ransomware. The mechanism — INC source code sold on RAMP for ~$300,000 in May 2024 — is consistent across reporting. Some debate exists whether Lynx represents the original INC operators rebranding versus a new actor who purchased the code; current weight of evidence suggests a new operator group rather than the original INC team, though this cannot be definitively resolved with open-source data alone.

**"Ethical" Targeting Claim:** Lynx publicly claims to avoid hospitals, government institutions, and non-profits. This should be treated as unverified and unreliable. Energy sector targeting — including Electrica, a Romanian national energy supplier — is inconsistent with this posture. Defenders in critical infrastructure should not treat this claim as meaningful threat mitigation.

**Affiliate Model Risks:** Group-IB's successful infiltration of the Lynx affiliate panel indicates the group's operational security is not impenetrable. However, the panel's sophistication (negotiation chats, pre-built binary library, publication scheduling) indicates the core operator team has invested significantly in scaling affiliate operations. The 80/20 profit split is competitive within the RaaS marketplace and is likely driving affiliate recruitment success.

**IOC Coverage Gaps:** Confirmed IOCs at time of generation are limited. The three SHA256 hashes documented represent early samples. Binary compilation dates and code signing details from the Nextron Systems and DFIR Report analyses contain additional IOCs not fully captured here. Analysts should consult ransomware.live and dedicated threat intel platforms (VirusTotal, ANY.RUN, MalwareBazaar) for an up-to-date IOC feed, as Lynx affiliates likely use customized builds per engagement.

**Victim Count Trajectory:** Growth from 0 to 393 victims in under two years is rapid. If current tempo continues, Lynx is on track to exceed 500 confirmed victims by mid-2026. Manufacturing and US-based organizations remain the highest-risk cohort.

**No CVE Exploitation Confirmed:** Current reporting does not attribute specific CVE exploitation to Lynx. Initial access appears primarily credential/phishing-driven. This may change as affiliates mature or the core operator team adds exploit capabilities.

---

## Profile Metadata

| Field | Value |
|---|---|
| **Skill Version** | threat-actor-profiler v1.1 |
| **Queries Run** | 7 (4 RSS trawl attempted — all failed due to proxy block; 7 web searches completed) |
| **Articles Retrieved** | 15+ unique sources |
| **Articles Analyzed** | 15 |
| **Articles Skipped** | 0 |
| **Profile Generated** | 2026-04-29 10:45:00 UTC |
| **Last Consolidated** | _Not yet consolidated_ |
| **Updates Merged** | 0 |
