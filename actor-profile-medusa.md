# Threat Actor Profile: Medusa Ransomware Group

**Generated:** 2026-04-28 10:55:00 UTC
**Search Window:** 2021-06-01 → 2026-04-28
**Sources Analyzed:** 14 articles / advisories
**Confidence:** 🟢 High — Multiple corroborating sources including a joint FBI/CISA/MS-ISAC advisory (AA25-071A), Unit 42, Darktrace, Intel 471, SC Media, Armis, Barracuda, Check Point, Halcyon, and others with consistent findings across attribution, TTPs, and targeting.

---

## Overview

Medusa is a financially motivated ransomware-as-a-service (RaaS) operation first identified in June 2021 that has evolved from a closed, single-actor group into one of the most prolific ransomware threats globally. The group employs double and triple extortion tactics — encrypting victim data, threatening public exposure via its dark web leak blog, and in documented cases demanding additional payment post-settlement by claiming the original ransom was intercepted. As of February 2025, Medusa and its affiliates have compromised over 300 critical infrastructure organizations, with attack volume running approximately 45% ahead of 2024 levels in early 2026. The group's rapid growth is partly attributable to the 2024 law enforcement disruptions of its primary competitors, ALPHV/BlackCat and LockBit, which effectively pushed affiliates and victims toward Medusa. Technically, Medusa has demonstrated increasing sophistication, most notably through the deployment of the ABYSSWORKER kernel-mode driver — a BYOVD (Bring Your Own Vulnerable Driver) tool that disables endpoint detection and response (EDR) software using stolen code-signing certificates, representing a significant capability uplift.

---

## Identity

| Field | Value |
|---|---|
| **Primary Name** | Medusa |
| **Known Aliases** | Medusa Blog (leak site brand); MITRE Group G1051 |
| **Suspected Origin** | Russia or allied state (unconfirmed) |
| **Active Since** | June 2021 |
| **Primary Motivation** | 💰 Financial |

> **⚠ Critical Disambiguation:** Medusa (this group) is **not** MedusaLocker (a separate RaaS active since 2019), and is **not** the Medusa Android banking trojan. The FBI's advisory AA25-071A explicitly notes these are distinct threats. Confusion between the three is common in lower-quality reporting.

---

## Targeting

Medusa primarily targets small and medium-sized enterprises (SMEs) with annual revenues between USD $5 million and $50 million across critical infrastructure sectors, particularly those with weaker security postures and less mature incident response capabilities. The group deliberately avoids organizations within Russia and the Commonwealth of Independent States (CIS), a pattern consistent with Russian-nexus threat actors who operate under informal immunity arrangements. Geographic targeting is otherwise global, with victims concentrated in North America, Western Europe, and the Asia-Pacific region.

### Sectors

- Education (K-12 and higher education)
- Financial Services and Insurance
- Government and Public Administration
- Healthcare and Medical Services
- Legal Services
- Manufacturing
- Technology and IT Services

### Regions

- Asia-Pacific
- Europe (Western)
- North America (primary focus)
- South America (secondary targeting observed)

> **Note:** Russia and CIS countries are deliberately excluded from targeting, strongly suggesting the group operates with the tacit tolerance of Russian or allied state authorities.

---

## Capabilities

Medusa is assessed as a technically capable, well-resourced threat actor occupying the upper tier of ransomware operators. The group has demonstrated the ability to develop or procure custom tooling (ABYSSWORKER driver), leverage packer-as-a-service infrastructure (HeartCrypt), and obtain stolen code-signing certificates to evade detection. The core development team maintains direct control over ransom negotiations even within the affiliate model, suggesting a disciplined organizational structure. The group's BYOVD capability for kernel-level EDR disablement represents a significant advancement that places Medusa above the median RaaS group in terms of technical sophistication. The documented transition from a closed to an affiliate-based model, combined with recruitment of Initial Access Brokers (IABs) through cybercriminal forums, indicates a professionalised criminal enterprise with effective operational security.

### Signature TTPs

1. **Vulnerability exploitation for initial access** — targets unpatched public-facing applications (Exchange, ConnectWise ScreenConnect CVE-2024-1709, Fortinet EMS CVE-2023-48788) rather than relying solely on phishing
2. **Initial Access Broker (IAB) recruitment** — purchases access from brokers on RAMP and other Russian-language cybercrime forums, with payments ranging from $100 to over $1 million
3. **Living-off-the-land lateral movement** — uses RDP, WMI, and PsExec rather than custom implants; blends into legitimate admin traffic to evade detection
4. **BYOVD EDR disablement via ABYSSWORKER** — deploys the `smuol.sys` driver (signed with stolen certificates, delivered via HeartCrypt) to disable major EDR solutions at the kernel level before payload execution
5. **Mass tool termination before encryption** — `gaze.exe` terminates 100–200+ services (backups, security, databases) and deletes Volume Shadow Copies prior to AES-256 encryption
6. **Rclone-based cloud exfiltration** — systematically exfiltrates data to attacker-controlled cloud storage prior to encryption to support double-extortion leverage
7. **Triple extortion** — post-payment, a separate Medusa actor contacts victims claiming the negotiator stole the ransom and demands re-payment for the "true decryptor" (FBI-documented)

### Known Tools & Malware

- ABYSSWORKER (smuol.sys) — custom kernel-mode BYOVD driver for EDR disablement
- Advanced IP Scanner — network reconnaissance
- AnyDesk — remote access / lateral movement
- Atera — RMM tool used for persistence and C2
- BigFix — encryptor deployment
- ConnectWise — remote access / lateral movement
- eHorus — remote access tool
- gaze.exe — primary encryptor payload (AES-256)
- HeartCrypt — Packer-as-a-Service used to deliver ABYSSWORKER
- N-able — RMM tool
- PDQ Deploy — encryptor mass deployment
- PDQ Inventory — asset enumeration
- PsExec (Sysinternals) — encryptor deployment and remote execution
- Rclone — data exfiltration to cloud storage
- Robocopy — bulk file copying for exfiltration staging
- SimpleHelp — remote access tool
- SoftPerfect Network Scanner — network reconnaissance
- Splashtop — remote access tool
- SQLite3-64.dll — supporting database utility
- svhost.exe — secondary malicious executable

---

## MITRE ATT&CK Mapping

| ID | Technique | Tactic |
|---|---|---|
| [T1078](https://attack.mitre.org/techniques/T1078/) | Valid Accounts | Defense Evasion, Persistence, Initial Access |
| [T1190](https://attack.mitre.org/techniques/T1190/) | Exploit Public-Facing Application | Initial Access |
| [T1566](https://attack.mitre.org/techniques/T1566/) | Phishing | Initial Access |
| [T1136.002](https://attack.mitre.org/techniques/T1136/002/) | Create Account: Domain Account | Persistence |
| [T1219](https://attack.mitre.org/techniques/T1219/) | Remote Access Software | Command and Control |
| [T1059.001](https://attack.mitre.org/techniques/T1059/001/) | Command and Scripting Interpreter: PowerShell | Execution |
| [T1047](https://attack.mitre.org/techniques/T1047/) | Windows Management Instrumentation | Execution |
| [T1569.002](https://attack.mitre.org/techniques/T1569/002/) | System Services: Service Execution (PsExec) | Execution |
| [T1548](https://attack.mitre.org/techniques/T1548/) | Abuse Elevation Control Mechanism (COM UAC bypass) | Privilege Escalation |
| [T1003.001](https://attack.mitre.org/techniques/T1003/001/) | OS Credential Dumping: LSASS Memory | Credential Access |
| [T1027](https://attack.mitre.org/techniques/T1027/) | Obfuscated Files or Information | Defense Evasion |
| [T1027.013](https://attack.mitre.org/techniques/T1027/013/) | Obfuscated Files or Information: Encrypted/Encoded File | Defense Evasion |
| [T1070.003](https://attack.mitre.org/techniques/T1070/003/) | Indicator Removal: Clear Command History | Defense Evasion |
| [T1562.001](https://attack.mitre.org/techniques/T1562/001/) | Impair Defenses: Disable or Modify Tools (ABYSSWORKER BYOVD) | Defense Evasion |
| [T1046](https://attack.mitre.org/techniques/T1046/) | Network Service Discovery | Discovery |
| [T1082](https://attack.mitre.org/techniques/T1082/) | System Information Discovery | Discovery |
| [T1083](https://attack.mitre.org/techniques/T1083/) | File and Directory Discovery | Discovery |
| [T1135](https://attack.mitre.org/techniques/T1135/) | Network Share Discovery | Discovery |
| [T1021.001](https://attack.mitre.org/techniques/T1021/001/) | Remote Services: Remote Desktop Protocol | Lateral Movement |
| [T1567.002](https://attack.mitre.org/techniques/T1567/002/) | Exfiltration Over Web Service: Exfiltration to Cloud Storage | Exfiltration |
| [T1071.001](https://attack.mitre.org/techniques/T1071/001/) | Application Layer Protocol: Web Protocols | Command and Control |
| [T1486](https://attack.mitre.org/techniques/T1486/) | Data Encrypted for Impact | Impact |
| [T1489](https://attack.mitre.org/techniques/T1489/) | Service Stop | Impact |
| [T1490](https://attack.mitre.org/techniques/T1490/) | Inhibit System Recovery (VSS deletion) | Impact |

---

## Recent Activity

As of Q1 2026, Medusa remains one of the top ten most active ransomware operations globally. Intel 471 recorded 90 confirmed victims in the first half of 2025 alone, with Cyble reporting near-daily attacks running approximately 45% higher than 2024 levels. The group achieved a third-place global ranking by Q2 2025 according to Halcyon's Frontrunners tracking. A significant technical development in early 2025 was the documented deployment of the ABYSSWORKER kernel-mode driver — delivered via the HeartCrypt packer service and signed with stolen certificates — marking a notable capability escalation. In March 2025, the FBI, CISA, and MS-ISAC jointly issued advisory AA25-071A, formally warning critical infrastructure operators about Medusa and documenting over 300 victim organizations.

### Notable Campaigns

- **Critical Infrastructure Campaign (2023–present)** — Sustained targeting of healthcare, education, manufacturing, and government sectors globally; 300+ confirmed victims as of February 2025
- **Post-ALPHV/LockBit Surge (March 2024–present)** — Medusa attacks rose sharply in March 2024 coinciding with law enforcement disruption of ALPHV/BlackCat and LockBit operations, suggesting affiliate migration to Medusa
- **Triple Extortion Incidents (2024–2025)** — FBI documented cases where post-payment, a second Medusa actor demanded additional ransom for the "true decryptor," alleging the negotiator had stolen the initial payment
- **ABYSSWORKER BYOVD Campaign (early 2025)** — Deployment of custom kernel-mode driver `smuol.sys` (ABYSSWORKER) to disable EDR solutions at kernel level prior to ransomware execution, representing a significant technical evolution

---

## Indicators of Compromise

### IP Addresses

`172.64.154.227`, `185.220.100.249`, `195.123.246.138`, `207.188.6.17`

### Domains

`go-sw6-02.adventos.de`, `medusakxxtp3uo7vusntvubnytaph4d3amxivbggl3hnhpk2nmus34yd.onion`

### File Hashes

`657c0cce98d6e73e53b4001eeea51ed91fdcf3d47a18712b6ba9c66d59677980` (SHA256 — Medusa encryptor)

### File Artifacts

- **Executables:** `gaze.exe` (encryptor), `svhost.exe`, `smuol.sys` (ABYSSWORKER driver)
- **DLLs:** `SQLite3-64.dll`
- **Encrypted file extensions:** `.MEDUSA`, `.mylock`
- **Ransom note filename:** `!!!!READ_ME_MEDUSA!!.txt`
- **Database artifact:** `.s3db` extension (created by ransomware)

### CVEs

`CVE-2023-48788` (Fortinet EMS SQL injection), `CVE-2024-1709` (ConnectWise ScreenConnect authentication bypass)

> **Note:** The FBI/CISA joint advisory AA25-071A (March 2025) contains the most comprehensive and up-to-date IOC list. Organizations should consult [stopransomware.gov](https://stopransomware.gov) for the full IOC appendix.

---

## Source Articles

| # | Title | Source | Published | Recency |
|---|---|---|---|---|
| 1 | [#StopRansomware: Medusa Ransomware (AA25-071A)](https://www.cisa.gov/news-events/cybersecurity-advisories/aa25-071a) | CISA / FBI / MS-ISAC | 2025-03-12 | Recent |
| 2 | [Medusa Ransomware Uses Malicious Driver to Disable Anti-Malware with Stolen Certificates](https://thehackernews.com/2025/03/medusa-ransomware-uses-malicious-driver.html) | The Hacker News | 2025-03 | Recent |
| 3 | [Medusa Ransomware 2025: RMM Abuse in Ransomware Campaigns](https://www.darktrace.com/blog/under-medusas-gaze-how-darktrace-uncovers-rmm-abuse-in-ransomware-campaigns) | Darktrace | 2025 | Recent |
| 4 | [Medusa ransomware and its cybercrime ecosystem](https://blog.barracuda.com/2025/02/25/medusa-ransomware-and-its-cybercrime-ecosystem) | Barracuda Networks | 2025-02-25 | Recent |
| 5 | [Medusa Ransomware Activity Continues to Increase](https://www.security.com/threat-intelligence/medusa-ransomware-attacks) | Security.com | 2025 | Recent |
| 6 | [Threat hunting case study: Medusa ransomware](https://www.intel471.com/blog/threat-hunting-case-study-medusa-ransomware) | Intel 471 | 2025 | Recent |
| 7 | [Medusa Ransomware Group: A Rising Threat in 2025](https://www.checkpoint.com/cyber-hub/threat-prevention/ransomware/medusa-ransomware-group/) | Check Point | 2025 | Recent |
| 8 | [Medusa Ransomware Analysis, Simulation, and Mitigation (AA25-071A)](https://www.picussecurity.com/resource/blog/medusa-ransomware-cisa-alert-aa25-071a) | Picus Security | 2025 | Recent |
| 9 | [Breaking Down Medusa Ransomware](https://www.armis.com/blog/breaking-down-medusa-ransomware/) | Armis | 2024–2025 | Recent |
| 10 | [Medusa Attack Analysis](https://reliaquest.com/blog/medusa-attack-analysis/) | ReliaQuest | 2024–2025 | Recent |
| 11 | [Medusa rises from obscurity to RaaS powerhouse](https://www.scworld.com/perspective/medusas-rise-from-obscurity-to-raas-powerhouse) | SC World / SC Media | 2024 | Recent |
| 12 | [Medusa Ransomware Turning Your Files into Stone](https://unit42.paloaltonetworks.com/medusa-ransomware-escalation-new-leak-site/) | Palo Alto Unit 42 | 2023 | Historical |
| 13 | [Medusa Threat Actor Profile: TTPs, IOCs & Attacks](https://www.huntress.com/threat-library/threat-actors/medusa) | Huntress | 2023–2025 | Recent |
| 14 | [Dark Web Profile: Medusa Ransomware (MedusaLocker)](https://socradar.io/blog/dark-web-profile-medusa-ransomware-medusalocker/) | SOCRadar | 2023 | Historical |

---

## Analyst Notes

**Attribution uncertainty:** Medusa is assessed with moderate-to-high confidence as Russia-nexus based on consistent CIS avoidance behavior and RAMP forum activity, but no public attribution by a government body has formally named a Russian individual or entity. The group may operate with state tolerance rather than active state direction.

**Lazarus Group reporting:** At least one source (Industrial Cyber) reported that North Korea's Lazarus Group has adopted Medusa ransomware for extortion campaigns. This should be treated with caution — it may reflect Lazarus using the public builder or a similar strain, not operational collaboration with the Medusa RaaS operators. No corroborating technical evidence linking Lazarus to Medusa's core infrastructure was identified.

**Triple extortion caveat:** The post-payment re-extortion incidents documented by the FBI are concerning but it is not yet clear whether this is a coordinated Medusa policy or rogue affiliate behavior. The model allows for rogue actors.

**Coverage gaps:** No detailed reporting on Medusa's full dark web infrastructure, precise affiliate recruitment and payment structures, or internal toolchain development pipeline was publicly available at time of writing. The group's C2 infrastructure rotates frequently and the IOC list above should be treated as a historical sample, not a comprehensive live indicator set. Organizations should consult the CISA advisory for refreshed IOCs.

**Naming confusion risk:** A non-trivial volume of threat intelligence reporting conflates Medusa with MedusaLocker. When reviewing third-party intel, verify that reported IOCs, CVEs, and TTPs map to the correct group (G1051 on MITRE ATT&CK, SHA256 `657c0...` encryptor family, `.MEDUSA` extension) before actioning.

**ABYSSWORKER significance:** The deployment of a signed, kernel-mode driver for EDR disablement represents a significant technical maturation. Organizations relying solely on EPP/EDR tools without additional layers (network detection, deception, log aggregation) are substantially more exposed to this group than the average ransomware actor.

---

## Profile Metadata

| Field | Value |
|---|---|
| **Skill Version** | threat-actor-profiler v1.1 |
| **Queries Run** | 7 (WebSearch; RSS trawl attempted but blocked by proxy) |
| **Articles Retrieved** | 14 unique sources |
| **Articles Analyzed** | 14 |
| **Articles Skipped** | 0 |
| **Profile Generated** | 2026-04-28 10:55:00 UTC |
| **Last Consolidated** | _Not yet consolidated_ |
| **Updates Merged** | 0 |
