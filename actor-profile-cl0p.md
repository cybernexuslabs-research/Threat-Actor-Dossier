# Threat Actor Profile: Cl0p

**Generated:** 2026-04-28 10:45:00 UTC  
**Search Window:** 2019-02-01 → 2026-04-28  
**Sources Analyzed:** 12 articles / advisories  
**Confidence:** 🟢 High — Multiple corroborating sources including CISA/FBI joint advisories, Mandiant, CrowdStrike, Proofpoint, and SentinelOne; findings consistent across government and private sector intelligence.

---

## Overview

Cl0p is a financially motivated ransomware operation that first emerged in February 2019 as a variant of the CryptoMix ransomware family. The group — assessed by the FBI and CISA to be synonymous with or operationally inseparable from the threat actor tracked as TA505 (also attributed under the aliases FIN11, GOLD TAHOE, DEV-0950, and Snakefly) — has evolved from large-scale spear-phishing campaigns into one of the most impactful ransomware operators ever observed, distinguished by its repeated exploitation of zero-day vulnerabilities in enterprise managed file transfer (MFT) software.

Since approximately 2020, Cl0p has systematically abandoned traditional ransomware deployment in favor of mass data-theft extortion, leveraging MFT zero-days to compromise hundreds or thousands of organizations in single coordinated waves. Major campaigns against Accellion FTA (2020–21), GoAnywhere MFT (2023), MOVEit Transfer (2023), Cleo (2024–25), and Oracle E-Business Suite (2025) have generated estimated earnings exceeding $500 million USD and compromised over 2,700 organizations in a single campaign alone. The group operates a Tor-based extortion portal (CL0P^_-LEAKS) and employs double-to-quadruple extortion tactics. Despite a June 2021 law-enforcement action that arrested six suspected members in Ukraine, the core operation has remained operationally resilient and continues to escalate.

---

## Identity

| Field | Value |
|---|---|
| **Primary Name** | Cl0p (CL0P, Clop, C10P) |
| **Known Aliases** | TA505, FIN11, Snakefly, GOLD TAHOE, DEV-0950 |
| **Suspected Origin** | Russia / Commonwealth of Independent States (CIS) |
| **Active Since** | February 2019 (ransomware variant); TA505 infrastructure active since at least 2014 |
| **Primary Motivation** | 💰 Financial |

---

## Targeting

Cl0p does not appear to limit targeting by geography or industry when conducting MFT mass-exploitation campaigns — organizations using vulnerable file transfer software are opportunistically compromised regardless of sector. In earlier phishing-led campaigns (2019–2020), the group showed a preference for large enterprises in North America and Western Europe. High-value verticals including healthcare and financial services are disproportionately represented among confirmed victims, likely because these sectors both hold sensitive data and are under regulatory pressure to avoid public exposure.

### Sectors

- Aerospace
- Automotive
- Education
- Energy
- Engineering & Manufacturing
- Finance & Banking
- Government
- Healthcare
- High-Tech
- Legal & Professional Services
- Retail & Consumer
- Telecommunications
- Transportation & Logistics

### Regions

- Asia-Pacific
- Europe (primarily Western)
- North America (United States and Canada are most heavily victimized)
- South Korea (confirmed law-enforcement partner in 2021 arrests)

---

## Capabilities

Cl0p represents an advanced-tier financially motivated threat actor with demonstrated capability to discover or acquire zero-day vulnerabilities in widely deployed enterprise software, develop bespoke web shells and loaders, and execute simultaneous mass exploitation of thousands of targets. The group's shift from file-encrypting ransomware to exfiltration-only extortion reflects sophisticated operational maturity — avoiding the operational complexity and law-enforcement visibility of ransomware deployment while still extracting payments. Cl0p uses digitally signed binaries to evade security tooling, systematically disables endpoint protection and backup systems, and has maintained operational continuity through at least one significant law-enforcement disruption.

### Signature TTPs

1. **Zero-day exploitation of MFT software** — Repeatedly acquires or discovers zero-day flaws in enterprise file transfer products (Accellion FTA, GoAnywhere, MOVEit, Cleo, Oracle EBS) before any patch exists.
2. **Web shell deployment for persistent access** — Installs custom web shells (DEWMODE, LEMURLOOT) via SQL injection into vulnerable MFT applications to enable data exfiltration and command execution.
3. **Mass simultaneous victim compromise** — Executes exploitation campaigns in tight windows against large populations of internet-facing targets, then extorts all victims concurrently.
4. **Double/quadruple extortion** — Combines data encryption (historically), data exfiltration, Tor-based leak-site exposure, and direct harassment of victim leadership and partners.
5. **Backup and recovery destruction** — Deletes shadow copies, disables Windows Volume Shadow Copy Service, and disables automated backup solutions prior to ransomware deployment to eliminate recovery options.
6. **Security tool disablement** — Actively terminates endpoint detection and response (EDR) processes and disables Windows Defender before executing the encryption payload.
7. **Signed binary abuse** — Code-signs malicious binaries with legitimate or stolen certificates to bypass reputation-based security controls.

### Known Tools & Malware

- Cl0p ransomware (CryptoMix-derived)
- DEWMODE (web shell — Accellion FTA campaigns)
- FlawedAmmyy RAT (remote access trojan)
- Get2 (first-stage downloader/loader)
- LEMURLOOT (web shell — MOVEit Transfer campaigns)
- SDBot (backdoor trojan for lateral movement)
- ServHelper (backdoor for remote access and credential harvesting)

---

## MITRE ATT&CK Mapping

| ID | Technique | Tactic |
|---|---|---|
| [T1059](https://attack.mitre.org/techniques/T1059/) | Command and Scripting Interpreter | Execution |
| [T1106](https://attack.mitre.org/techniques/T1106/) | Native API | Execution |
| [T1566.001](https://attack.mitre.org/techniques/T1566/001/) | Phishing: Spearphishing Attachment | Initial Access |
| [T1190](https://attack.mitre.org/techniques/T1190/) | Exploit Public-Facing Application | Initial Access |
| [T1078](https://attack.mitre.org/techniques/T1078/) | Valid Accounts | Persistence / Defense Evasion |
| [T1505.003](https://attack.mitre.org/techniques/T1505/003/) | Server Software Component: Web Shell | Persistence |
| [T1553.002](https://attack.mitre.org/techniques/T1553/002/) | Subvert Trust Controls: Code Signing | Defense Evasion |
| [T1027](https://attack.mitre.org/techniques/T1027/) | Obfuscated Files or Information | Defense Evasion |
| [T1562.001](https://attack.mitre.org/techniques/T1562/001/) | Impair Defenses: Disable or Modify Tools | Defense Evasion |
| [T1070](https://attack.mitre.org/techniques/T1070/) | Indicator Removal | Defense Evasion |
| [T1083](https://attack.mitre.org/techniques/T1083/) | File and Directory Discovery | Discovery |
| [T1057](https://attack.mitre.org/techniques/T1057/) | Process Discovery | Discovery |
| [T1021](https://attack.mitre.org/techniques/T1021/) | Remote Services | Lateral Movement |
| [T1105](https://attack.mitre.org/techniques/T1105/) | Ingress Tool Transfer | Command and Control |
| [T1071](https://attack.mitre.org/techniques/T1071/) | Application Layer Protocol | Command and Control |
| [T1041](https://attack.mitre.org/techniques/T1041/) | Exfiltration Over C2 Channel | Exfiltration |
| [T1486](https://attack.mitre.org/techniques/T1486/) | Data Encrypted for Impact | Impact |
| [T1489](https://attack.mitre.org/techniques/T1489/) | Service Stop | Impact |
| [T1490](https://attack.mitre.org/techniques/T1490/) | Inhibit System Recovery | Impact |
| [T1485](https://attack.mitre.org/techniques/T1485/) | Data Destruction | Impact |

---

## Campaign Timeline

Cl0p's operational history spans three distinct phases: an initial spear-phishing-led ransomware deployment phase (2019–2020), a transition to MFT appliance exploitation (2020–2022), and a mature mass-extortion-only model focused exclusively on MFT zero-days (2023–present). The group was briefly disrupted by the June 2021 Ukrainian arrests but resumed operations within weeks.

### Notable Campaigns

- **Initial Cl0p deployments via spear-phishing (Feb 2019 – 2020)** — Large-scale campaigns using Get2 loader → SDBot/FlawedAmmyy → Cl0p ransomware payload; targets primarily in North America and Europe.
- **Accellion FTA zero-day campaign (Dec 2020 – Jan 2021)** — Exploited CVE-2021-27101/27102/27103/27104 in Accellion File Transfer Appliance; installed DEWMODE web shell via SQL injection; 100+ confirmed victims including Shell, Qualys, and the Reserve Bank of New Zealand.
- **SolarWinds Serv-U exploitation (Jul 2021)** — Exploited CVE-2021-35211 to spawn sub-processes enabling command execution, reconnaissance, and lateral movement for ransomware staging.
- **Ukrainian law enforcement arrests (Jun 2021)** — Six suspected Cl0p members arrested in Ukraine in a joint operation with South Korean and US law enforcement; operation briefly disrupted but core group remained intact.
- **GoAnywhere MFT zero-day campaign (Jan–Feb 2023)** — Exploited CVE-2023-0669; 130+ victims compromised over roughly 10 days; exfiltration-only, no file encryption deployed.
- **MOVEit Transfer zero-day campaign (May–Jun 2023)** — Exploited CVE-2023-34362 (CVSS 9.8); deployed LEMURLOOT web shell; ~2,000 organizations compromised; victims included US federal agencies, UK regulator OFCOM, British Airways, BBC, Shell, and dozens of universities; single largest ransomware campaign by victim count.
- **Cleo MFT zero-day campaign (Dec 2024 – Q1 2025)** — Exploited CVE-2024-50623 and CVE-2024-55956 in Cleo LexiCom, VLTrader, and Harmony products; 200+ organizations compromised in December 2024 alone; Cl0p listed 389 victims in February 2025, a 1,400% increase year-over-year.
- **Oracle E-Business Suite zero-day campaign (2025)** — Exploited CVE-2025-61882; high-profile victims confirmed or reported include Broadcom, Cox Enterprises, Harvard University, The Washington Post, and Logitech.

---

## Indicators of Compromise

### IP Addresses

_See CISA Advisory AA23-158A and AA21-055A for full IOC sets; dynamic infrastructure limits persistent IP tracking._

### Domains

_Cl0p operates Tor-based extortion infrastructure (CL0P^_-LEAKS); C2 domains rotate per campaign. See CISA/FBI joint advisories for campaign-specific domains._

### File Hashes

_Campaign-specific hashes documented in CISA AA23-158A (MOVEit/LEMURLOOT), AA21-055A (Accellion/DEWMODE), and MAR-10325064-1.v1 (Accellion malware report). Representative samples tracked on Malpedia under win.clop._

### CVEs

CVE-2021-27101, CVE-2021-27102, CVE-2021-27103, CVE-2021-27104 (Accellion FTA), CVE-2021-35211 (SolarWinds Serv-U), CVE-2023-0669 (GoAnywhere MFT), CVE-2023-34362 (MOVEit Transfer), CVE-2024-50623 (Cleo LexiCom/VLTrader/Harmony), CVE-2024-55956 (Cleo Autorun), CVE-2025-61882 (Oracle E-Business Suite)

---

## Source Articles

| # | Title | Source | Published | Recency |
|---|---|---|---|---|
| 1 | [#StopRansomware: CL0P Ransomware Gang Exploits CVE-2023-34362 MOVEit Vulnerability](https://www.cisa.gov/news-events/cybersecurity-advisories/aa23-158a) | CISA / FBI | 2023-06-07 | Historical |
| 2 | [Exploitation of Accellion File Transfer Appliance](https://www.cisa.gov/news-events/cybersecurity-advisories/aa21-055a) | CISA | 2021-02-26 | Historical |
| 3 | [Clop Ransomware: History, Timeline, And Adversary Simulation](https://fourcore.io/blogs/clop-ransomware-history-adversary-simulation) | FourCore | 2024 | Historical |
| 4 | [Dissecting Cl0p Ransomware: Encryption and Evasion Tactics](https://www.picussecurity.com/resource/clop-ransomware-gang) | Picus Security | 2024 | Historical |
| 5 | [Profile: TA505 / CL0P ransomware](https://www.cyber.gc.ca/en/guidance/profile-ta505-cl0p-ransomware) | Canadian Centre for Cyber Security | 2023 | Historical |
| 6 | [Ransomware Spotlight: Clop](https://www.trendmicro.com/vinfo/us/security/news/ransomware-spotlight/ransomware-spotlight-clop) | Trend Micro | 2023 | Historical |
| 7 | [Clop ransomware gang names dozens of victims hit by Cleo mass-hack](https://techcrunch.com/2025/01/16/clop-ransomware-gang-names-dozens-of-victims-hit-by-cleo-mass-hack-but-several-firms-dispute-breaches/) | TechCrunch | 2025-01-16 | Recent |
| 8 | [Oracle E-Business Suite Zero-Day Exploited in Widespread Extortion Campaign](https://cloud.google.com/blog/topics/threat-intelligence/oracle-ebusiness-suite-zero-day-exploitation) | Google Cloud / Mandiant | 2025 | Recent |
| 9 | [Cl0p Ransomware Surge 2025: Operational Patterns and Key Mitigations](https://hivepro.com/threat-advisory/cl0p-ransomware-surge-2025-operational-patterns-and-key-mitigations/) | HivePro | 2025 | Recent |
| 10 | [Clop (Software S0611)](https://attack.mitre.org/software/S0611/) | MITRE ATT&CK | Ongoing | Recent |
| 11 | [Clop gang exploiting SolarWinds Serv-U flaw in ransomware attacks](https://www.bleepingcomputer.com/news/security/clop-gang-exploiting-solarwinds-serv-u-flaw-in-ransomware-attacks/) | BleepingComputer | 2021 | Historical |
| 12 | [FIN11 Spun Out From TA505 Umbrella as Distinct Attack Group](https://www.securityweek.com/fin11-spun-out-ta505-umbrella-distinct-attack-group/) | SecurityWeek | 2020 | Historical |

---

## Analyst Notes

**Attribution uncertainty — TA505/FIN11 relationship:** The FBI and CISA publicly assess that CL0P and TA505 are the same threat actor. Mandiant tracks a subset of this activity as FIN11 (separated out in 2016 for distinct post-compromise TTPs) and Google/Mandiant now classify FIN11 activity under the UNC2546 cluster for the MFT campaigns. The consensus view is that these designations refer to the same core Russian-speaking criminal organization, with some analytical disagreement about whether TA505 is a broader umbrella that includes Cl0p-using affiliates, or whether Cl0p is the exclusive tooling of a single cohesive group.

**Law-enforcement resilience:** The June 2021 Ukrainian arrests targeted lower-tier operational personnel — money mules, network intrusion support — rather than the leadership or developers. The group resumed operations within weeks, suggesting the core technical and leadership tier is insulated from the arrested individuals and almost certainly resides outside Ukraine.

**Shift away from file encryption:** Since approximately 2022–2023, Cl0p has largely ceased deploying the file-encrypting ransomware payload in favor of pure exfiltration-and-extortion. This avoids triggering incident response by not causing visible service disruption, reduces attack complexity, and circumvents some backup-based defenses. Analysts should not expect file encryption to be a reliable detection signal in current Cl0p intrusions.

**MFT vendor pattern:** Cl0p has targeted Kiteworks/Accellion, SolarWinds Serv-U, GoAnywhere MFT, MOVEit Transfer, Cleo, and Oracle EBS in sequence. This strongly suggests the group (or its zero-day procurement channel) systematically audits enterprise MFT solutions for vulnerabilities, prioritizing software used by large numbers of enterprise customers across sectors. Defenders should treat any new critical CVE in enterprise file transfer software as a potential Cl0p exploitation target.

**Intelligence gap:** Precise financial flows, ransom payment rates, and current operational infrastructure (C2 servers, current personnel) remain poorly documented in open-source reporting. The group's internal structure, number of affiliates, and whether it operates as a closed operation or RaaS with external affiliates is contested across sources.

---

## Profile Metadata

| Field | Value |
|---|---|
| **Skill Version** | threat-actor-profiler v1.1 |
| **Queries Run** | 6 (WebSearch fallback — RSS proxy blocked) |
| **Articles Retrieved** | 12 |
| **Articles Analyzed** | 12 |
| **Articles Skipped** | 0 |
| **Profile Generated** | 2026-04-28 10:45:00 UTC |
| **Last Consolidated** | _Not yet consolidated_ |
| **Updates Merged** | 0 |
