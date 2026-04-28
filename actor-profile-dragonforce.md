# Threat Actor Profile: DragonForce

**Generated:** 2026-04-28 10:50:00 UTC
**Search Window:** 2023-03-01 → 2026-04-28
**Sources Analyzed:** 16 articles / 8 web search queries
**Confidence:** 🟢 High — Multiple consistent, independent sources from Trend Micro, SentinelOne, Check Point, Group-IB, Bitdefender, Intel 471, Quorum Cyber, and Barracuda Networks corroborate core findings.

---

## Overview

DragonForce is a financially-motivated ransomware cartel that emerged in August 2023, initially presenting itself as a pro-Palestinian hacktivist collective before rapidly pivoting to large-scale extortion operations. The group formalized its criminal enterprise in June 2024 by launching a Ransomware-as-a-Service (RaaS) affiliate program offering affiliates 80% of ransom proceeds and white-label infrastructure. By early 2025, DragonForce rebranded as a ransomware "cartel," absorbing rival groups including BlackLock and attempting to co-opt RansomHub's affiliate base, demonstrating sophisticated awareness of the criminal marketplace. From December 2023 through April 2026, the group has publicly listed 363+ victim organizations on its data leak site, with high-profile operations including a coordinated assault on UK retail giants Marks & Spencer, Co-op, and Harrods in April–May 2025, estimated to have caused £270–440 million in financial damage. The group's aggressive cartel-building, white-label RaaS model, and willingness to absorb or undermine rival operators mark it as one of the most structurally ambitious criminal cyber enterprises observed in recent years.

---

## Identity

| Field | Value |
|---|---|
| **Primary Name** | DragonForce |
| **Known Aliases** | DragonForce Malaysia (related, disputed), DFRC (DragonForce Ransomware Cartel), DragonForce Cartel |
| **Suspected Origin** | Contested — possible ties to a Malaysian hacktivist collective (DragonForce Malaysia); however, DragonForce Malaysia has publicly denied a connection and the ransomware operation shows no confirmed Malaysian infrastructure or C2 |
| **Active Since** | August 2023 (ransomware operations); DragonForce Malaysia hacktivist collective predates this |
| **Primary Motivation** | 💰 Financial (evolved from Hacktivism) |

---

## Targeting

DragonForce demonstrates opportunistic, global targeting with a concentration on mid-to-large enterprises and critical public-sector organizations. While the group's earliest victims were concentrated in the Asia-Pacific and US markets, the 2025 UK retail campaign revealed a capability to conduct coordinated, high-impact operations against specific sectors. Affiliates under the cartel model expand geographic and sectoral coverage considerably.

### Sectors

- Financial Services
- Food & Beverage
- Government / Public Administration
- Healthcare & Pharmaceuticals
- Lottery & Gaming
- Manufacturing
- Retail (high priority in 2025–2026)
- Transportation & Transit
- Technology / IT Services

### Regions

- Australia
- Egypt
- Palau (government)
- Saudi Arabia
- Singapore
- United Kingdom (significant escalation in 2025)
- United States

---

## Capabilities

DragonForce operates at an intermediate-to-advanced level of technical sophistication, combining commodity tooling (Cobalt Strike, Mimikatz) with custom ransomware payloads and novel evasion techniques such as BYOVD kernel driver abuse. The group's true force multiplier is its cartel model: by providing affiliates with a full attack-management portal, white-label encryptors, and 80% revenue share, it lowers the barrier to entry and enables scale without requiring operator-level expertise from all participants. The group's 2025 absorption of BlackLock and attempted integration of RansomHub further demonstrated an ability to conduct offensive cyber operations against rival criminal infrastructure.

### Signature TTPs

1. **Social engineering at IT help desks** — Operators (often Scattered Spider affiliates) conduct phone-based impersonation, SIM swapping, MFA push bombing, and fake SSO portals to harvest VPN/Active Directory credentials, bypassing technical controls entirely.
2. **BYOVD for EDR/AV neutralization** — Deploying a legitimate but vulnerable kernel driver (observed: RogueKiller Anti-Rootkit Driver) and exploiting it to terminate endpoint security products before detonating the payload.
3. **Credential harvesting with Mimikatz & LaZagne** — Post-access dumping of LSASS memory for plaintext passwords, NTLM hashes, and Kerberos tickets, including domain admin credential capture.
4. **RMM-assisted lateral movement** — SimpleHelp RMM tool leveraged for stealthy lateral movement after initial foothold, blending into legitimate IT activity.
5. **Pre-encryption data exfiltration (double extortion)** — Sensitive data (customer PII, employee records, financial data) is exfiltrated before any ransomware deployment to maximize extortion leverage; victims face both encryption and publication threats.
6. **ESXi VM mass encryption** — DragonForce encryptors target VMware ESXi hosts, enabling rapid simultaneous encryption of large numbers of virtual machines and maximizing business disruption.
7. **White-label affiliate cartel model** — Core operators provide customizable payloads, dedicated leak sites, ransom negotiation infrastructure, and attack-management portals to affiliates, enabling diverse operator profiles under a single brand.

### Known Tools & Malware

- Cobalt Strike (lateral movement, C2)
- DragonForce Encryptor — LockBit 3.0-derived variant (AES-256 file encryption, RSA-2048 key wrapping)
- DragonForce Encryptor — Conti v3-derived variant (ChaCha8 file encryption, ChaCha20 config encryption)
- LaZagne (credential extraction)
- Mimikatz (LSASS credential dumping)
- RogueKiller Anti-Rootkit Driver (abused for BYOVD EDR kill)
- SimpleHelp RMM (lateral movement)
- SoftPerfect Network Scanner (network discovery/mapping)
- SystemBC (backdoor, persistent C2)

---

## MITRE ATT&CK Mapping

| ID | Technique | Tactic |
|---|---|---|
| [T1078](https://attack.mitre.org/techniques/T1078/) | Valid Accounts | Initial Access / Persistence |
| [T1190](https://attack.mitre.org/techniques/T1190/) | Exploit Public-Facing Application | Initial Access |
| [T1566](https://attack.mitre.org/techniques/T1566/) | Phishing | Initial Access |
| [T1547](https://attack.mitre.org/techniques/T1547/) | Boot or Logon Autostart Execution (Registry Run Keys) | Persistence |
| [T1134](https://attack.mitre.org/techniques/T1134/) | Access Token Manipulation | Privilege Escalation |
| [T1548](https://attack.mitre.org/techniques/T1548/) | Abuse Elevation Control Mechanism | Privilege Escalation |
| [T1562](https://attack.mitre.org/techniques/T1562/) | Impair Defenses (BYOVD / kill EDR) | Defense Evasion |
| [T1003](https://attack.mitre.org/techniques/T1003/) | OS Credential Dumping (LSASS via Mimikatz) | Credential Access |
| [T1082](https://attack.mitre.org/techniques/T1082/) | System Information Discovery | Discovery |
| [T1046](https://attack.mitre.org/techniques/T1046/) | Network Service Discovery (SoftPerfect) | Discovery |
| [T1021](https://attack.mitre.org/techniques/T1021/) | Remote Services | Lateral Movement |
| [T1219](https://attack.mitre.org/techniques/T1219/) | Remote Access Software (SimpleHelp) | Command and Control |
| [T1041](https://attack.mitre.org/techniques/T1041/) | Exfiltration Over C2 Channel | Exfiltration |
| [T1486](https://attack.mitre.org/techniques/T1486/) | Data Encrypted for Impact | Impact |
| [T1489](https://attack.mitre.org/techniques/T1489/) | Service Stop | Impact |
| [T1657](https://attack.mitre.org/techniques/T1657/) | Financial Theft / Double Extortion | Impact |

---

## Recent Activity

DragonForce's operational tempo increased sharply through 2025 and into 2026. The April–May 2025 UK retail campaign — coordinated attacks on Marks & Spencer, Co-op, and Harrods, attributed to DragonForce affiliates working alongside Scattered Spider — resulted in estimated losses of £270–440 million and triggered UK Parliamentary scrutiny. Four individuals were subsequently arrested in connection with the attacks (July 2025). Concurrently, DragonForce executed a hostile takeover of the BlackLock ransomware group and announced an integration with RansomHub (April 2025), though this relationship collapsed into mutual hostility within weeks. As of late April 2026, DragonForce claimed fresh attacks against FAT Brands Inc. (US) and Gelatissimo Pty Ltd (Australia), maintaining consistent targeting of retail and food-service sectors.

### Notable Campaigns

- **Ohio Lottery** (December 2023) — 538K customers compromised; 3 million records / ~600 GB exfiltrated
- **Yakult Australia** (early 2024) — confirmed victim; data exfiltrated
- **Government of Palau** (March 2024) — ransom notes from both LockBit and DragonForce left on compromised systems
- **Honolulu OTS / Oahu Transit Services** (2024)
- **Coca-Cola Singapore** (2024)
- **Marks & Spencer / Co-op / Harrods** (April–May 2025) — coordinated UK retail campaign; M&S online orders suspended for ~1 week; ESXi hosts encrypted; £270–440M estimated impact; 4 arrests
- **BlackLock Absorption** (early 2025) — DragonForce compromised BlackLock's infrastructure, leaked internal chat logs, and effectively destroyed the rival group
- **RansomHub Cartel Integration / Conflict** (April 2025) — DragonForce announced RansomHub would join the cartel (April 8); RansomHub disputed this and counter-claimed DragonForce's DLS was compromised by insiders (April 25)
- **FAT Brands Inc.** (April 27, 2026) — claimed attack against US restaurant franchise operator
- **Gelatissimo Pty Ltd** (April 27, 2026) — claimed attack against Australian ice cream brand

---

## Indicators of Compromise

> ⚠️ IOCs are point-in-time and may be rotated. Verify against current threat intelligence feeds before actioning.

### IP Addresses

131.226.2.6, 134.199.202.205, 104.238.159.149, 188.130.206.168

### Domains

c34718cbb4c6.ngrok-free.app

### File Hashes

_None confirmed in open sources reviewed._

### CVEs

CVE-2025-49704, CVE-2025-49706, CVE-2025-53770, CVE-2025-53771

### Behavioral IOCs

- File extension: `.dragonforce_encrypted` (configurable per affiliate; may vary)
- Ransom note filename: `README.txt`
- Encrypted config filename pattern: `e47qfsnz2trbkhnt.devman`
- Shadow copy deletion via `vssadmin` or `wmic` prior to encryption
- Registry Run key modifications for persistence

---

## Source Articles

| # | Title | Source | Published | Recency |
|---|---|---|---|---|
| 1 | [DragonForce Ransomware Group Targets 363 Companies, Expands Cartel-Like Operations Since 2023](https://gbhackers.com/dragonforce-ransomware-group/) | GBHackers | 2026 | Recent |
| 2 | [Four Arrested in £440M Cyber Attack on Marks & Spencer, Co-op, and Harrods](https://thehackernews.com/2025/07/four-arrested-in-440m-cyber-attack-on.html) | The Hacker News | Jul 2025 | Recent |
| 3 | [DragonForce Ransomware Cartel vs. Everybody](https://blog.barracuda.com/2025/06/09/dragonforce-ransomware-cartel-vs--everybody) | Barracuda Networks | Jun 2025 | Recent |
| 4 | [DragonForce Ransomware: Redefining Hybrid Extortion in 2025](https://blog.checkpoint.com/security/dragonforce-ransomware-redefining-hybrid-extortion-in-2025/) | Check Point Blog | 2025 | Recent |
| 5 | [DragonForce Ransomware Gang \| From Hacktivists to High Street Extortionists](https://www.sentinelone.com/blog/dragonforce-ransomware-gang-from-hacktivists-to-high-street-extortionists/) | SentinelOne | 2025 | Recent |
| 6 | [Retail Under Fire: Inside the DragonForce Ransomware Attacks on Industry Giants](https://www.picussecurity.com/resource/blog/dragonforce-ransomware-attacks-retail-giants) | Picus Security | 2025 | Recent |
| 7 | [The DragonForce Cartel: Scattered Spider at the gate](https://www.acronis.com/en/tru/posts/the-dragonforce-cartel-scattered-spider-at-the-gate/) | Acronis | 2025 | Recent |
| 8 | [BlackLock Ransomware Exposed & DragonForce Activity](https://slcyber.io/blog/blacklock-ransomware-exposed-and-dragonforce-makes-moves/) | Searchlight Cyber | 2025 | Recent |
| 9 | [The Godfather of Ransomware? Inside DragonForce's Cartel Ambitions](https://www.levelblue.com/blogs/spiderlabs-blog/the-godfather-of-ransomware-inside-dragonforces-cartel-ambitions) | LevelBlue/SpiderLabs | 2025 | Recent |
| 10 | [DragonForce: The Ransomware Cartel Guarding Its Burrow](https://www.bitdefender.com/en-us/blog/businessinsights/dragonforce-ransomware-cartel) | Bitdefender | 2025 | Recent |
| 11 | [DragonForce Ransomware Group](https://www.group-ib.com/blog/dragonforce-ransomware/) | Group-IB | 2025 | Recent |
| 12 | [DragonForce Ransomware \| Intel 471](https://www.intel471.com/blog/dragonforce-ransomware) | Intel 471 | 2025 | Recent |
| 13 | [Ransomware Spotlight: DragonForce](https://www.trendmicro.com/vinfo/us/security/news/ransomware-spotlight/ransomware-spotlight-dragonforce) | Trend Micro | 2024–2025 | Recent |
| 14 | [DragonForce Ransomware: Technical Analysis and Mitigation Strategies](https://www.loginsoft.com/post/dragonforce-ransomware-technical-analysis-and-mitigation-strategies) | Loginsoft | 2024 | Recent |
| 15 | [DragonForce Ransomware — Reverse Engineering Report](https://www.resecurity.com/blog/article/dragonforce-ransomware-reverse-engineering-report) | Resecurity | 2024 | Recent |
| 16 | [Ohio Lottery Cyberattack Compromised 538K Customers](https://www.casino.org/news/ohio-lottery-cyberattack-compromised-538k-customers/) | Casino.org | Dec 2023 | Historical |

---

## Analyst Notes

**Attribution caveat — Malaysia link:** The relationship between DragonForce (ransomware cartel) and DragonForce Malaysia (hacktivist collective) is widely debated. DragonForce Malaysia has publicly denied any connection and states it has never used ransomware. Infrastructure and C2 analysis shows no confirmed Malaysian hosting, and the ransomware group imposes no rules prohibiting attacks on Malaysian targets. The shared name may be coincidental or an intentional misdirection; analysts should treat them as distinct entities unless new evidence emerges.

**Cartel model risks and fragmentation:** The April 2025 RansomHub integration collapsed rapidly into mutual accusations of betrayal and counter-attacks, suggesting DragonForce's cartel ambitions may face structural instability as affiliates and partner groups protect their own brand equity. The group's aggressive absorption of BlackLock also sets a precedent for hostile takeover of rivals, creating adversarial dynamics within the criminal ecosystem.

**Scattered Spider nexus:** Multiple incidents link Scattered Spider / The Com to DragonForce ransomware deployment. Scattered Spider supplies initial access (social engineering, SIM swapping) while DragonForce provides the payload and extortion infrastructure. This is a division-of-labor arrangement, not a formal merger; Scattered Spider members operate independently and are not DragonForce operators.

**Intelligence gaps:** Published file hash IOCs are limited in open sources — a curated hash list from CISA, FBI, or ISAC advisories should be sought for detection engineering. Attribution of specific attacks to named DragonForce operators (as opposed to affiliates) remains thin. The cartel's dark web infrastructure (.onion addresses) rotates frequently.

**CVE note:** CVEs listed (CVE-2025-49704, CVE-2025-49706, CVE-2025-53770, CVE-2025-53771) appeared in a threat advisory context and may relate to a specific affiliate campaign rather than core DragonForce tooling; verify applicability to your environment.

---

## Profile Metadata

| Field | Value |
|---|---|
| **Skill Version** | threat-actor-profiler v1.1 |
| **Queries Run** | 8 (4 primary + 4 supplemental) |
| **Articles Retrieved** | 16 unique sources |
| **Articles Analyzed** | 16 |
| **Articles Skipped** | 0 |
| **Profile Generated** | 2026-04-28 10:50:00 UTC |
| **Last Consolidated** | _Not yet consolidated_ |
| **Updates Merged** | 0 |
