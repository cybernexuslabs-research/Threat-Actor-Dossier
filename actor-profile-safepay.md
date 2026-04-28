# Threat Actor Profile: SafePay

**Generated:** 2026-04-28 00:00:00 UTC
**Search Window:** 2024-09-01 → 2026-04-28
**Sources Analyzed:** 14 articles / reports
**Confidence:** 🟢 High — Multiple independent security vendors (Bitdefender, Huntress, Check Point, Halcyon, Acronis, KPMG, Picus, Infosecurity Magazine, GBHackers) have produced consistent technical findings with corroborating victim data.

---

## Overview

SafePay is a rapidly ascending ransomware group that first emerged in September–October 2024, positioning itself as a closed, centrally managed operation in the vacuum left by law enforcement actions against LockBit (Operation Cronos) and ALPHV/BlackCat. Unlike the dominant Ransomware-as-a-Service (RaaS) model, SafePay develops, deploys, and manages all aspects of its ransomware operation in-house — no affiliates, no franchise model. The group built its initial ransomware binary on leaked LockBit 3.0 (LockBit Black) source code, supplemented with elements resembling ALPHV/BlackCat and INC Ransom, making accurate binary-level attribution complex.

Within its first year of operation, SafePay claimed over 300 victims across multiple continents, briefly becoming the most active ransomware group globally in May 2025. Its most significant attack to date struck Ingram Micro in July 2025, compromising more than 42,000 individuals' data and demanding payment under a hard deadline. SafePay is believed to operate out of Eastern Europe, evidenced by language-detection logic that terminates execution on systems using former Soviet Union locale settings.

---

## Identity

| Field | Value |
|---|---|
| **Primary Name** | SafePay |
| **Known Aliases** | *None publicly confirmed* |
| **Suspected Origin** | Eastern Europe (former Soviet Union region) |
| **Active Since** | September 2024 |
| **Primary Motivation** | 💰 Financial |

---

## Targeting

SafePay exhibits broad opportunistic targeting with a pronounced bias toward North America and Western Europe. The United States is the single most targeted country, accounting for roughly 40% of confirmed victims (182 of ~450 total incidents tracked by Ransom-DB). Germany is the second most targeted nation; in Q1 2025, 24% of all reported ransomware victims in Germany were attributed to SafePay — the highest single-group share for any country in that period. The United Kingdom was an early target, with the Microlise attack in October 2024 being one of SafePay's first high-profile operations.

The group has demonstrated particular interest in managed service providers (MSPs) and IT distributors, likely due to the downstream impact potential and high-value data held by such organizations. Manufacturing, construction, and education appear consistently across victim lists.

### Sectors

- Agriculture
- Construction
- Education
- Finance / Financial Services
- Government
- Healthcare
- Information Technology / MSPs
- Manufacturing
- Retail
- Transport / Telematics

### Regions

- Europe (Germany, United Kingdom, wider EU)
- North America (United States — primary)
- Asia (limited, confirmed in Ingram Micro's affected operations)

---

## Capabilities

SafePay is assessed as a technically capable, well-resourced threat actor for a group less than two years old. Its ransomware payload is a PE32 DLL executed via `regsvr32.exe` (or `rundll32.exe`) with a mandatory `-pass=` command-line argument that decodes embedded configuration. Encryption is adaptive: ChaCha20 is used on capable systems, AES on lower-end hardware, with per-file symmetric keys protected by asymmetric cryptography — an architecture consistent with professional ransomware engineering. The group's decision to operate without affiliates suggests strong internal operational security discipline and a deliberate effort to minimize exposure.

The group routinely terminates a wide range of security, backup, and database services prior to encryption, demonstrates proficient use of living-off-the-land (LOLBin) techniques, and uses legitimate administrative tools (PsExec, WinRM, RDP) for lateral movement. This reduces their dependency on custom tooling and makes detection more challenging. Their adoption of email bombing and vishing (voice phishing) tactics to overwhelm victims — reported by Barracuda Networks in July 2025 — indicates evolving social engineering tradecraft beyond typical ransomware groups.

### Signature TTPs

1. **Initial access via stolen/purchased credentials** — valid VPN credentials acquired on dark web marketplaces, enabling RDP and VPN gateway entry with minimal noise
2. **Defense evasion via LOLBins and PowerShell** — disables Windows Defender and terminates AV/EDR products (Sophos confirmed) using native system utilities
3. **Discovery via ShareFinder.ps1** — repurposes the legitimate `Invoke-ShareFinder` script to enumerate network shares for data staging and lateral movement pivot points
4. **Lateral movement via RDP, PsExec, WinRM, and admin shares** — near-simultaneous deployment of the ransomware DLL across the environment
5. **Data exfiltration before encryption** — staged with WinRAR/7-Zip, then exfiltrated via Rclone or FileZilla; double-extortion model
6. **Shadow copy and backup destruction** — stops the VSS (Volume Shadow Copy Service) and deletes backups, including third-party solutions (Veeam confirmed)
7. **Email bombing and phone scams at ransom stage** — floods victim staff inboxes and uses voice calls to pressure payment; reported in attacks with very large ransom demands

### Known Tools & Malware

- 7-Zip (archival/staging)
- FileZilla (exfiltration)
- PsExec (lateral movement)
- Rclone (exfiltration)
- regsvr32.exe / rundll32.exe (payload execution)
- SafePay ransomware DLL (LockBit Black-derived)
- ShareFinder.ps1 / Invoke-ShareFinder (network reconnaissance)
- WinRAR (archival/staging)
- WinRM (lateral movement)

---

## MITRE ATT&CK Mapping

| ID | Technique | Tactic |
|---|---|---|
| [T1078](https://attack.mitre.org/techniques/T1078/) | Valid Accounts | Initial Access / Defense Evasion / Persistence |
| [T1133](https://attack.mitre.org/techniques/T1133/) | External Remote Services (VPN/RDP) | Initial Access |
| [T1059.001](https://attack.mitre.org/techniques/T1059/001/) | PowerShell | Execution |
| [T1047](https://attack.mitre.org/techniques/T1047/) | Windows Management Instrumentation | Execution |
| [T1218.010](https://attack.mitre.org/techniques/T1218/010/) | Regsvr32 | Defense Evasion |
| [T1562.001](https://attack.mitre.org/techniques/T1562/001/) | Impair Defenses: Disable or Modify Tools | Defense Evasion |
| [T1027](https://attack.mitre.org/techniques/T1027/) | Obfuscated Files or Information | Defense Evasion |
| [T1135](https://attack.mitre.org/techniques/T1135/) | Network Share Discovery | Discovery |
| [T1083](https://attack.mitre.org/techniques/T1083/) | File and Directory Discovery | Discovery |
| [T1021.001](https://attack.mitre.org/techniques/T1021/001/) | Remote Services: Remote Desktop Protocol | Lateral Movement |
| [T1021.006](https://attack.mitre.org/techniques/T1021/006/) | Remote Services: Windows Remote Management | Lateral Movement |
| [T1570](https://attack.mitre.org/techniques/T1570/) | Lateral Tool Transfer | Lateral Movement |
| [T1560.001](https://attack.mitre.org/techniques/T1560/001/) | Archive Collected Data: Archive via Utility | Collection |
| [T1048](https://attack.mitre.org/techniques/T1048/) | Exfiltration Over Alternative Protocol | Exfiltration |
| [T1490](https://attack.mitre.org/techniques/T1490/) | Inhibit System Recovery (VSS deletion) | Impact |
| [T1486](https://attack.mitre.org/techniques/T1486/) | Data Encrypted for Impact | Impact |
| [T1489](https://attack.mitre.org/techniques/T1489/) | Service Stop | Impact |

---

## Recent Activity

SafePay's operational tempo accelerated sharply through 2025. After claiming approximately 22 victims in its initial period (October–November 2024), the group surged to 43 confirmed victims in March 2025 (4th most active group globally), 58 victims in May 2025 (1st most active globally, per Cyble), and had accumulated over 403 total victims by mid-2026 according to Ransom-DB. The group's most consequential operation was the July 2025 attack on Ingram Micro, which disrupted global IT distribution operations and compromised personal data belonging to more than 42,000 employees and job applicants. SafePay posted a hard payment deadline of August 1, 2025.

Starting in mid-2025, Barracuda Networks documented a tactical evolution: SafePay began pairing email bombing campaigns against victim organizations with targeted voice phishing calls to overwhelm help desks and security teams — a social engineering overlay used alongside their technical ransomware deployment.

### Notable Campaigns

- **Microlise (October 2024)** — UK telematics/transport management firm; SafePay claimed 1.2 TB exfiltrated; disrupted DHL deliveries and UK Ministry of Justice prisoner van security systems
- **Ingram Micro (July 2025)** — Major global IT distributor; systems taken offline in US, Europe, and Asia; 42,000+ individuals' data compromised; August 1 payment deadline issued
- **Germany campaign (Q1 2025)** — SafePay accounted for 24% of all ransomware victims reported in Germany, the highest single-group concentration in any country during that quarter

---

## Indicators of Compromise

### IP Addresses
*No confirmed C2 IP addresses publicly released as of profile date.*

### Domains
*No confirmed C2 domains publicly released; ransom note references a V3 Tor onion address and a TON (The Open Network) site. Specific addresses not publicly disclosed.*

### File Hashes

`a0dc80a37eb7e2716c02a94adc8df9baedec192a77bde31669faed228d9ff526`, `fd509df74a8d6a9e96762337efd46280ebf8d154c6c5dfbac7b3e8f7bb61f191`, `625abbf876f256662f33a88c122bf787edf74b882c35adbd61562b5bd1b2ac27`

*(SHA256; sourced from ThreatLocker and HivePro analyses; SafePay regularly rotates binaries)*

### CVEs
*No specific CVE exploitation confirmed in public reporting; VPN appliance weaknesses and unpatched VMware/Citrix systems referenced generically.*

### Behavioral / Artifact IOCs

- Encrypted file extension: `.safepay`
- Ransom note filename: `readme_safepay.txt`
- Execution pattern: `regsvr32.exe <dll> -pass=<encoded_config>`
- Suspicious tools: `ShareFinder.ps1`, `PsExec`, `Rclone`, `FileZilla` executing in unusual contexts
- Batch files dropped to `%ProgramData%`
- DLL files written to administrative shares followed by remote execution via PsExec or WinRM

---

## Source Articles

| # | Title | Source | Published |
|---|---|---|---|
| 1 | [SafePay Ransomware: How a Non-RaaS Group Executes Rapid Fire Attacks](https://www.bitdefender.com/en-us/blog/businessinsights/safepay-ransomware-attacks-ttps) | Bitdefender Business Insights | 2024–2025 |
| 2 | [It's Not Safe to Pay SafePay](https://www.huntress.com/blog/its-not-safe-to-pay-safepay) | Huntress | 2024–2025 |
| 3 | [Unmasking the SafePay Ransomware Group](https://www.infosecurity-magazine.com/news-features/unmasking-safepay-ransomware-group/) | Infosecurity Magazine | 2025 |
| 4 | [SafePay Ransomware: An Emerging Threat in 2025](https://www.checkpoint.com/cyber-hub/threat-prevention/ransomware/safepay-ransomware/) | Check Point Software | 2025 |
| 5 | [Emerging Threat Actor: SafePay Ransomware](https://www.halcyon.ai/blog/emerging-threat-actor-safepay-ransomware) | Halcyon | 2024–2025 |
| 6 | [SafePay ransomware: The fast-rising threat targeting MSPs](https://www.acronis.com/en/tru/posts/safepay-ransomware-the-fast-rising-threat-targeting-msps/) | Acronis | 2025 |
| 7 | [SafePay ransomware explained: IOCs, TTPs, and defense strategies](https://www.threatlocker.com/blog/safepay-ransomware-explained-iocs-ttps-and-defense-strategies) | ThreatLocker | 2025 |
| 8 | [Inside SafePay: Analyzing the New Centralized Ransomware Group](https://www.picussecurity.com/resource/blog/inside-safepay-analyzing-the-new-centralized-ransomware-group) | Picus Security | 2025 |
| 9 | [SafePay Ransomware Strikes 260+ Victims Across Multiple Countries](https://gbhackers.com/safepay-ransomware-strikes-260-victims/) | GBHackers | 2025 |
| 10 | [SafePay: Email bombs, phone scams and really big ransoms](https://blog.barracuda.com/2025/07/25/safepay--email-bombs--phone-scans--and-really-big-ransoms) | Barracuda Networks | July 2025 |
| 11 | [Ingram Micro Working Through Ransomware Attack by SafePay Group](https://www.msspalert.com/news/ingram-micro-working-through-ransomware-attack-by-safepay-group) | MSSP Alert | July 2025 |
| 12 | [SafePay ransomware is a rapidly emerging threat](https://assets.kpmg.com/content/dam/kpmgsites/in/pdf/2026/01/kpmg-ctip-safepay-ransomware-07-Jan-2026.pdf.coredownload.inline.pdf) | KPMG CTIP | January 2026 |
| 13 | [SafePay Ransomware: LockBit's Lonewolf Ghost](https://dailysecurityreview.com/resources/safepay-ransomware-lockbits-lonewolf-ghost/) | Daily Security Review | 2025 |
| 14 | [SafePay ransomware: Obscure group uses LockBit builder, claims 22 victims](https://www.scworld.com/news/safepay-ransomware-obscure-group-uses-lockbit-builder-claims-22-victims) | SC Media | November 2024 |

---

## Analyst Notes

**Attribution confidence is Medium-High.** The Eastern European origin assessment rests primarily on the presence of CIS language-detection kill logic in the ransomware binary — a common (though not definitive) tradecraft indicator. No direct infrastructure attribution to specific individuals or organizations has been publicly confirmed.

**LockBit lineage is well-evidenced but not conclusive.** Multiple independent reversals (Bitdefender, Huntress, Picus) confirm significant code overlap with LockBit Black (3.0) leaked source code from 2022, along with ALPHV/BlackCat and INC Ransom resemblances. This suggests the operators either had access to, or are former participants of, those ecosystems — but code reuse is common across the ransomware ecosystem and does not prove direct organizational lineage.

**Operational security posture is notable.** SafePay's refusal to operate a RaaS model is a deliberate choice that limits their scale but improves OPSEC and attribution resistance. The group avoids the affiliate leakage problems that helped law enforcement dismantle LockBit. This is a strategic differentiator worth tracking.

**IOC shelf life is short.** SafePay regularly rotates binaries and infrastructure. File hashes listed in this profile should be treated as historical indicators; behavioral and artifact-based IOCs (file extension, execution pattern, tool combinations) are more durable detection signals.

**Social engineering evolution is a watch item.** The introduction of email bombing and vishing in mid-2025 aligns with tactics used by groups like Scattered Spider and suggests SafePay may be expanding its team or playbook. This tactical shift warrants continued monitoring.

**Gap: no public ransom amounts confirmed.** Despite being linked to high-profile attacks, no authoritative ransom demand figures have been published. The Barracuda report's reference to "really big ransoms" suggests large-enterprise targeting strategies are in play.

---

## Profile Metadata

| Field | Value |
|---|---|
| **Skill Version** | threat-actor-profiler v1.1 |
| **Queries Run** | 6 (4 OSINT + 2 supplemental web searches) |
| **Articles Retrieved** | 14 |
| **Articles Analyzed** | 14 |
| **Articles Skipped** | 0 |
| **Profile Generated** | 2026-04-28 00:00:00 UTC |
| **Last Consolidated** | *Not yet consolidated* |
| **Updates Merged** | 0 |
