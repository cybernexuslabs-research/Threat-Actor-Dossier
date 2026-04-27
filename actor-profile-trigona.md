# Threat Actor Profile: Trigona

**Generated:** 2026-04-10 14:40:00 UTC
**Search Window:** 2022-06-01 → 2026-04-10
**Sources Analyzed:** 28+ articles across vendor research (Trend Micro, Unit 42, ASEC, SentinelOne, Zscaler), trade press, and hacktivist disclosures
**Confidence:** 🟡 Medium — Well-documented during the active period (June 2022 – October 2023) by multiple major vendors; post-takedown activity and the BlackNevas rebrand hypothesis are supported by ASEC analysis but partially speculative. No government/CISA advisory was ever published specifically for Trigona.

---

## Overview

Trigona (tracked by Trend Micro as "Water Ungaw") is a financially motivated ransomware operation whose binaries were first observed in June 2022 and whose public identity emerged in late October 2022. The operators are assessed as Russian-speaking based on their presence on the Russian Anonymous Marketplace (RAMP) forum, from which they solicited compromised credentials and negotiated with initial access brokers. Trend Micro and other researchers have documented strong tactical and toolchain overlap with the CryLock ransomware family, including shared ransom note file naming conventions and operator email addresses, indicating likely personnel or codebase continuity between CryLock and Trigona.

The encryptor is written in Delphi, uses RSA + AES (TDCP_rijndael library) for file encryption, appends the `._locked` extension, and drops a ransom note directing victims to a Tor-based negotiation portal with Monero-only payments. Trigona employed double extortion: data theft and a Tor-hosted leak site listing victims. The operation ran continuously from June 2022 until October 18, 2023, when the Ukrainian Cyber Alliance (UCA) hacktivists exploited CVE-2023-22515 (Atlassian Confluence) to breach, exfiltrate, and wipe Trigona's entire infrastructure — servers, leak site, internal tools, and Monero wallets. Despite this takedown, the same threat actor resumed attacks on MSSQL servers by January 2024 (this time deploying both the Trigona payload and the Mimic ransomware), and by November 2024 a derivative strain called "BlackNevas" appeared with the same encryption underpinnings and MSSQL-targeting TTPs, suggesting the operation continues under a new brand.

---

## Identity

| Field | Value |
|---|---|
| **Primary Name** | Trigona |
| **Known Aliases** | Water Ungaw (Trend Micro tracking name), BlackNevas / Trial Recovery (assessed successor, November 2024+) |
| **Suspected Origin** | Russia / Russian-speaking (active on RAMP forum; CryLock lineage) |
| **Active Since** | June 2022 (earliest binary); publicly named October 2022 |
| **Primary Motivation** | 💰 Financial (double-extortion ransomware) |
| **Operating Model** | Hybrid: small core group developing the payload and infrastructure, purchasing access from IABs, and advertising 20–50% revenue share with accomplices |
| **Affiliation** | Assessed as an evolution or rebrand of the CryLock ransomware operation based on shared TTPs, ransom note conventions, and email addresses |

---

## Targeting

Trigona was largely opportunistic but showed a consistent preference for organizations running poorly managed, internet-exposed Microsoft SQL servers — a vector that became the group's dominant initial access pathway from April 2023 onward. Beyond MSSQL, the group purchased credentials from RAMP-connected initial access brokers and exploited public-facing applications (notably ManageEngine, CVE-2021-40539). Victim telemetry shows the United States (28%), India (25%), and Turkey as the primary geographies, with additional hits across Europe, Latin America, and the broader APAC region. Sector targeting was broad but heaviest in technology, healthcare, manufacturing, finance, construction, and retail.

### Sectors

- Agriculture
- Construction and engineering
- Education
- Finance and banking
- Healthcare
- High technology / IT services
- Legal services
- Manufacturing
- Marketing
- Retail

### Regions

- Asia-Pacific (India, Turkey, and broader APAC)
- Europe
- Latin America
- North America (United States — 28% of observed victims)

---

## Capabilities

Trigona is a mid-tier ransomware operation that combined readily available commodity tools (Mimikatz, Cobalt Strike, legitimate remote-access software) with a custom Delphi-based encryptor and custom tooling for SQL-server exploitation. The group demonstrated adaptability: when its primary infrastructure was destroyed in October 2023, the same operators resumed operations within months using Mimic ransomware alongside their own payload, and ultimately shipped the BlackNevas derivative (November 2024) with improved encryption parameters. The encryptor supports both Windows and Linux (the Linux variant appeared in May 2023), has configurable behavior via embedded resource sections, and uses intermittent encryption parameters tuned per-version. While Trigona lacked the zero-day capability and scale of top-tier brands, it was efficient at high-volume exploitation of exposed databases and remote services, and could turn initial access to full domain compromise and data exfiltration within days.

### Signature TTPs

1. Initial access predominantly via brute-force or dictionary attacks against internet-exposed Microsoft SQL (MSSQL) servers with weak credentials. The Bulk Copy Program (BCP) utility of MSSQL is abused to stage and install malware payloads.
2. Installation of CLR SqlShell malware on compromised MSSQL servers as a persistence mechanism and command executor, supporting privilege escalation (MS16-032 exploitation), information gathering, and user-account manipulation.
3. Exploitation of public-facing applications: ManageEngine ADSelfService Plus (CVE-2021-40539) for initial access; operators also purchased credentials from initial access brokers via RAMP.
4. Deployment of Cobalt Strike beacons via PowerShell for C2, including use of the MHYPROTINST driver/loader to terminate AV processes (a BYOVD-adjacent tactic).
5. Credential theft using Mimikatz (packaged as a password-protected executable `DC2.exe` to evade detection).
6. Network reconnaissance using Advanced Port Scanner and SoftPerfect Network Scanner; domain enumeration via standard Windows commands.
7. Lateral movement via legitimate remote-access tools — Splashtop, AnyDesk, ScreenConnect, LogMeIn, TeamViewer — and RDP with newly created accounts.
8. Defense evasion via Defender Control (`DC.exe`) to disable Windows Defender, and SDelete (`xdel.exe` from Sysinternals) for anti-forensic file wiping.
9. Data exfiltration prior to encryption (specific tooling not publicly documented, but double extortion with a leak site confirms systematic exfiltration).
10. File encryption using the Delphi-based Trigona payload: RSA + AES (TDCP_rijndael), `._locked` extension, ransom note directing to Tor negotiation portal, Monero-only payments.
11. Post-takedown adaptation: deployment of Mimic ransomware (which abuses the "Everything" file-search engine for rapid file discovery) alongside Trigona payloads in January 2024+; shipping of BlackNevas (November 2024) with same AES-256 + RSA-4112 OFB-mode encryption and SMB spreading capabilities.

### Known Tools & Malware

- Advanced Port Scanner (network discovery)
- AnyDesk / LogMeIn / ScreenConnect / Splashtop / TeamViewer (remote access)
- BCP (Bulk Copy Program — MS-SQL malware staging)
- BlackNevas ransomware (assessed successor, November 2024+)
- CLR SqlShell (MSSQL persistence and command execution)
- Cobalt Strike Beacon (C2)
- Defender Control / DC.exe (AV tampering)
- Everything (file-search utility, abused by Mimic variant)
- MHYPROTINST (driver loader for AV process termination)
- Mimikatz / DC2.exe (credential dumping)
- Mimic ransomware (adopted by same actor post-takedown)
- SDelete / xdel.exe (anti-forensic wiping)
- SoftPerfect Network Scanner (network discovery)
- Trigona ransomware — Windows (Delphi, `._locked` extension)
- Trigona ransomware — Linux (May 2023 variant)

---

## MITRE ATT&CK Mapping

| ID | Technique | Tactic |
|---|---|---|
| [T1190](https://attack.mitre.org/techniques/T1190/) | Exploit Public-Facing Application (ManageEngine CVE-2021-40539, MSSQL) | Initial Access |
| [T1110](https://attack.mitre.org/techniques/T1110/) | Brute Force (MSSQL dictionary/brute-force attacks) | Credential Access / Initial Access |
| [T1078](https://attack.mitre.org/techniques/T1078/) | Valid Accounts (purchased from IABs via RAMP) | Initial Access / Persistence |
| [T1059.001](https://attack.mitre.org/techniques/T1059/001/) | PowerShell (Cobalt Strike delivery) | Execution |
| [T1059.004](https://attack.mitre.org/techniques/T1059/004/) | Unix Shell (Linux variant) | Execution |
| [T1505.001](https://attack.mitre.org/techniques/T1505/001/) | SQL Stored Procedures (CLR SqlShell) | Persistence |
| [T1136](https://attack.mitre.org/techniques/T1136/) | Create Account (new domain accounts for RDP access) | Persistence |
| [T1068](https://attack.mitre.org/techniques/T1068/) | Exploitation for Privilege Escalation (MS16-032 via SqlShell) | Privilege Escalation |
| [T1562.001](https://attack.mitre.org/techniques/T1562/001/) | Disable or Modify Tools (Defender Control, MHYPROTINST) | Defense Evasion |
| [T1070.004](https://attack.mitre.org/techniques/T1070/004/) | File Deletion (SDelete/xdel.exe anti-forensics) | Defense Evasion |
| [T1003](https://attack.mitre.org/techniques/T1003/) | OS Credential Dumping (Mimikatz) | Credential Access |
| [T1046](https://attack.mitre.org/techniques/T1046/) | Network Service Discovery (Advanced Port Scanner, SoftPerfect) | Discovery |
| [T1018](https://attack.mitre.org/techniques/T1018/) | Remote System Discovery | Discovery |
| [T1021.001](https://attack.mitre.org/techniques/T1021/001/) | Remote Desktop Protocol | Lateral Movement |
| [T1219](https://attack.mitre.org/techniques/T1219/) | Remote Access Software (AnyDesk, Splashtop, ScreenConnect) | Command and Control |
| [T1071](https://attack.mitre.org/techniques/T1071/) | Application Layer Protocol (Cobalt Strike C2) | Command and Control |
| [T1048](https://attack.mitre.org/techniques/T1048/) | Exfiltration Over Alternative Protocol | Exfiltration |
| [T1486](https://attack.mitre.org/techniques/T1486/) | Data Encrypted for Impact (RSA + AES, `._locked`) | Impact |
| [T1490](https://attack.mitre.org/techniques/T1490/) | Inhibit System Recovery | Impact |
| [T1657](https://attack.mitre.org/techniques/T1657/) | Financial Theft (Monero extortion) | Impact |

---

## Recent Activity

Trigona's lifecycle divides cleanly into three phases across the search window:

**Phase 1 — Active Operations (June 2022 – October 2023):** Continuous victim posting from late 2022 through mid-2023, peaking with the MSSQL brute-force campaign (April 2023 onward). Publicly reported ransoms ranged from USD 150,000 (engineering firm) to USD 2 million (healthcare provider), with the peak disclosed demand at USD 750,000 against a U.S. manufacturing company. Trend Micro, Unit 42, Zscaler, and ASEC all published detailed technical analyses during this period. A Linux variant appeared in May 2023.

**Phase 2 — Takedown and Immediate Aftermath (October 2023 – January 2024):** On October 18, 2023, the Ukrainian Cyber Alliance exploited CVE-2023-22515 (Atlassian Confluence critical privilege-escalation vulnerability) to compromise Trigona's Confluence server, maintain undetected persistence for approximately six days, and ultimately exfiltrate the entire infrastructure — including the admin panel, landing page, blog, leak site, Monero hot wallets, developer environment, and internal Rocket Chat/Jira instances — before defacing and wiping all servers. UCA pledged to release decryption keys if found. Despite the infrastructure loss, by mid-January 2024 ASEC detected new attacks attributed to the same actor, this time deploying both Mimic ransomware and Trigona payloads against MSSQL targets using BCP for staging.

**Phase 3 — Suspected Rebrand: BlackNevas (November 2024 – Present):** In November 2024, the BlackNevas (aka "Trial Recovery") ransomware strain emerged with encryption internals nearly identical to Trigona — AES-256 + RSA-4112 in OFB mode — plus the same SMB spreading capabilities and MSSQL-targeting TTPs. Operational differences include use of Telegram (`@BlackNevas`) and victim-specific email addresses for negotiation rather than a Tor portal. As of early 2026, BlackNevas has attacked organizations in Asia (50% of targets), North America, and Europe across critical infrastructure and business sectors, and ASEC intelligence reports in March 2026 continued to flag activity attributed to the Trigona/BlackNevas cluster.

### Notable Campaigns and Incidents

- June 2022: Earliest Trigona binary samples observed in the wild.
- October 2022: Trigona publicly identified; first public victim disclosures appear on BleepingComputer forums.
- Late 2022 – early 2023: Operations ramp up with initial access via IABs and ManageEngine exploitation.
- March 2023: Infiltration of a U.S. manufacturing company via phishing; USD 750,000 demand for encrypted CAD/CAM files.
- April 2023: MSSQL brute-force campaign begins; CLR SqlShell becomes the group's primary persistence mechanism; ASEC publishes detailed analysis.
- May 2023: Linux variant of Trigona appears.
- 2023: Publicly reported ransom payments include USD 150,000 (engineering firm), USD 750,000 (manufacturing), and USD 2 million (healthcare provider).
- October 18, 2023: Ukrainian Cyber Alliance (UCA) breaches Trigona's Confluence server via CVE-2023-22515, exfiltrates all infrastructure (including admin panels, Monero wallets, dev environment, Rocket Chat, Jira), defacess and wipes all servers. "Trigona is Gone!" message posted.
- January 2024: ASEC detects new attacks by the same actor, deploying Mimic ransomware alongside Trigona payloads, still targeting MSSQL servers.
- July 2024: Incident response engagement identifies an intrusion with Trigona-consistent TTPs (SoftPerfect Network Scanner, openrdp.bat, newuser.bat), confirming continued operator activity.
- November 2024: BlackNevas ransomware first observed; SentinelOne and ASEC analyze samples confirming Trigona code-level lineage.
- 2025–2026: BlackNevas continues targeting businesses and critical infrastructure in Asia-Pacific, North America, and Europe. ASEC threat intelligence reports (through March 2026) continue to track the Trigona/BlackNevas cluster as active.

---

## Indicators of Compromise

Trigona's original infrastructure was wiped by UCA in October 2023 and is no longer operational. Post-takedown activity uses fresh infrastructure. Atomic indicators below are sourced from ASEC, Trend Micro, and Unit 42 reporting from the active period; defenders should pull current BlackNevas IOCs from ASEC's 2025–2026 publications.

### IP Addresses
_None captured in this collection run. Pull from ASEC and Trend Micro Trigona/BlackNevas reporting for current C2 addresses._

### Domains
- Original Tor leak site and negotiation portal: offline since October 2023 (defaced/wiped by UCA)
- BlackNevas: uses Telegram channel `@BlackNevas` and victim-specific email addresses; no known .onion DLS

### File Hashes
_None captured in this collection run. Defenders should pull hashes from: ASEC "Trigona Ransomware Attacking MS-SQL Servers" (2023); Trend Micro "Ransomware Spotlight: Trigona" and "An Overview of the Different Versions" (2023); Unit 42 "Bee-Ware of Trigona" (2023); SentinelOne "BlackNevas" (2024); ASEC "Trigona Rebranding Suspicions and BlackNevas Analysis" (2025)._

### CVEs

CVEs exploited **by Trigona operators for initial access:**
- CVE-2021-40539 (ManageEngine ADSelfService Plus authentication bypass / remote code execution)
- MS16-032 (Windows Secondary Logon local privilege escalation — used via CLR SqlShell)

CVE exploited **against Trigona's own infrastructure** (by UCA hacktivist takedown):
- CVE-2023-22515 (Atlassian Confluence Data Center / Server critical privilege escalation)

---

## Source Articles

| # | Title | Source | Published | Recency |
|---|---|---|---|---|
| 1 | [Trigona Rebranding Suspicions and Global Threats, and BlackNevas Ransomware Analysis](https://asec.ahnlab.com/en/90080/) | ASEC (AhnLab) | 2025 | 2025 |
| 2 | [Analysis of Trigona Threat Actor's Latest Attack Cases](https://asec.ahnlab.com/en/90793/) | ASEC (AhnLab) | 2025 | 2025 |
| 3 | [Trigona Ransomware Threat Actor Uses Mimic Ransomware](https://asec.ahnlab.com/en/61000/) | ASEC (AhnLab) | 2024-01 | 2024 |
| 4 | [BlackNevas](https://www.sentinelone.com/anthology/blacknevas/) | SentinelOne | 2024 | 2024 |
| 5 | [Threat Intelligence Report March 24 to March 30, 2026](https://redpiranha.net/news/threat-intelligence-report-march-24-march-30-2026) | Red Piranha | 2026-03 | Recent |
| 6 | [Ransomware Spotlight: Trigona](https://www.trendmicro.com/vinfo/us/security/news/ransomware-spotlight/ransomware-spotlight-trigona) | Trend Micro | 2023 | Historical |
| 7 | [An Overview of the Different Versions of the Trigona Ransomware](https://www.trendmicro.com/en_us/research/23/f/an-overview-of-the-trigona-ransomware.html) | Trend Micro | 2023-06 | Historical |
| 8 | [Bee-Ware of Trigona, An Emerging Ransomware Strain](https://unit42.paloaltonetworks.com/trigona-ransomware-update/) | Palo Alto Unit 42 | 2023 | Historical |
| 9 | [Technical Analysis of Trigona Ransomware](https://www.zscaler.com/blogs/security-research/technical-analysis-trigona-ransomware) | Zscaler | 2023 | Historical |
| 10 | [Trigona Ransomware Attacking MS-SQL Servers](https://asec.ahnlab.com/en/51343/) | ASEC (AhnLab) | 2023-04 | Historical |
| 11 | [Trigona Ransomware: In-Depth Analysis, Detection, and Mitigation](https://www.sentinelone.com/anthology/trigona/) | SentinelOne | 2023 | Historical |
| 12 | [Trigona Ransomware Family Explained](https://nisos.com/research/trigona-ransomware-explained/) | Nisos | 2023 | Historical |
| 13 | [Observations on New Trigona Ransomware (PDF)](https://areteir.com/static/5055b091d5c24a9ed63a06d70f2da20e/Trigona-Report_020224_web.pdf) | Arete IR | 2023-02 | Historical |
| 14 | [Ukrainian activists hack Trigona ransomware gang, wipe servers](https://www.bleepingcomputer.com/news/security/ukrainian-activists-hack-trigona-ransomware-gang-wipe-servers/) | BleepingComputer | 2023-10 | Historical |
| 15 | [Pro-Ukraine group says it took down Trigona ransomware website](https://therecord.media/trigona-ransomware-group-website-takedown-ukrainian-cyber-alliance) | The Record | 2023-10 | Historical |
| 16 | [Ukrainian Cyber Alliance Disrupts Trigona Ransomware Operations, Exfiltrates Key Data](https://www.bitdefender.com/en-us/blog/hotforsecurity/ukrainian-cyber-alliance-disrupts-trigona-ransomware-operations-exfiltrates-key-data) | Bitdefender | 2023-10 | Historical |
| 17 | [Ukrainian Hacktivists Claim Trigona Ransomware Takedown](https://www.bankinfosecurity.com/ukrainian-hacktivists-claim-trigona-ransomware-takedown-a-23343) | BankInfoSecurity | 2023-10 | Historical |
| 18 | [Trigona Ransomware Takedown By Ukrainian Cyber Alliance](https://thecyberexpress.com/trigona-ransomware-takedown-with-servers-wiped/) | The Cyber Express | 2023-10 | Historical |
| 19 | [Trigona Ransomware targets Microsoft SQL servers](https://securityaffairs.com/145036/cyber-crime/trigona-ransomware-targets-microsoft-sql-servers.html) | Security Affairs | 2023 | Historical |
| 20 | [Trigona Ransomware Trolling for 'Poorly Managed' MS-SQL Servers](https://www.darkreading.com/remote-workforce/trigona-ransomware-trolling-for-poorly-managed-ms-sql-servers-) | Dark Reading | 2023 | Historical |
| 21 | [Trigona Ransomware: An Evolving Cyberthreat Targeting Global Industries](https://www.anvilogic.com/threat-reports/evolving-threat-trigona-ransomware) | Anvilogic | 2023 | Historical |
| 22 | [Trigona: A Ransomware Wiper](https://www.acronis.com/en/tru/posts/trigona-a-ransomware-wiper/) | Acronis | 2023 | Historical |
| 23 | [Trigona Ransomware: What Is It And How To Defend Against It](https://stonefly.com/blog/what-is-trigona-ransomware/) | StoneFly | 2023 | Historical |
| 24 | [Trigona Ransomware: How to Remove and Prevent Attacks](https://www.provendata.com/blog/trigona-ransomware) | Proven Data | 2023 | Historical |
| 25 | [Hackers Hijacking MS-SQL Servers to Install Mimic Ransomware](https://gbhackers.com/hackers-hijacking-ms-sql-servers/) | GBHackers | 2024 | 2024 |
| 26 | [Cyber Awakeness Month: Takedown of Trigona](https://socradar.io/cyber-awakeness-month-takedown-of-trigona-hive-ransomware-resurges-ransomedforum-and-new-raas-qbit/) | SOCRadar | 2023-10 | Historical |
| 27 | [Ransomware.live: Trigona](https://www.ransomware.live/group/trigona) | Ransomware.live | Ongoing | Reference |
| 28 | [Ransomlook: Black Nevas](https://www.ransomlook.io/group/Black%20Nevas) | Ransomlook | 2025 | 2025 |

---

## Analyst Notes

Trigona's story across the search window is a compact case study in the resilience of mid-tier ransomware operations. The group emerged from CryLock lineage with a Delphi-based encryptor in June 2022, found product-market fit with MSSQL brute-forcing as its signature access vector by April 2023, and then suffered one of the most publicly dramatic infrastructure takedowns in ransomware history when UCA exploited its own Confluence server against it. Yet within three months the same operators were back — initially recycling their payloads alongside the Mimic strain, and by late 2024 shipping BlackNevas, a cleaned-up derivative with the same encryption internals.

For defenders, the durable lesson is that Trigona/BlackNevas intrusions are overwhelmingly dependent on two attack surfaces: internet-exposed MSSQL servers with weak credentials and unpatched public-facing management applications (ManageEngine in the original phase). Organizations that enforce strong credentials and MFA on database services, disable SA accounts, restrict BCP utility usage, and keep ADSelfService Plus and similar products patched will eliminate the vast majority of Trigona-lineage attack surface. Post-compromise, the group's toolchain is commodity-heavy — Mimikatz, Cobalt Strike, legitimate remote-access software — and should trip standard detection engineering for credential theft and anomalous RDP/Splashtop/AnyDesk sessions.

The CryLock → Trigona → BlackNevas lineage (June 2022 → November 2024) and the group's unbroken targeting of MSSQL even after a full infrastructure loss suggest a small, technically skilled team with deep familiarity with database infrastructure as an entry point. They operate below the scale that draws multi-national law-enforcement task forces, which likely explains why no CISA advisory or formal government attribution exists for Trigona, despite the group's real-world impact (documented ransoms up to USD 2 million). Threat-intelligence teams should monitor ASEC's ongoing MSSQL-campaign reporting and BlackNevas IOC sets as the most current indicators of this cluster's activity.

---

## Profile Metadata

| Field | Value |
|---|---|
| **Skill Version** | threat-actor-profiler v1.1 |
| **Queries Run** | 7 (web search, multi-angle) |
| **Articles Retrieved** | 50+ |
| **Articles Analyzed** | 28 |
| **Articles Skipped** | 0 |
| **Profile Generated** | 2026-04-10 14:40:00 UTC |
| **Last Consolidated** | _Not yet consolidated_ |
| **Updates Merged** | 0 |
| **Collection Method Note** | RSS trawler blocked by sandbox egress; switched to authenticated web search against vendor and trade-press sources. |
