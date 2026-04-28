# Threat Actor Profile: Scattered Spider

**Generated:** 2026-04-28 11:00:00 UTC
**Search Window:** 2022-05-01 → 2026-04-28
**Sources Analyzed:** 18 articles and advisories
**Confidence:** 🟢 High — Multiple consistent findings across government advisories, vendor reports, court documents, and MITRE ATT&CK

---

## Overview

Scattered Spider is a financially motivated, English-speaking cybercriminal collective active since at least March 2022. The group rose to notoriety through a sophisticated blend of social engineering, SIM swapping, and phishing-as-a-service infrastructure, initially compromising telecommunications and technology firms before dramatically expanding its targeting footprint. Unlike traditional nation-state APTs, Scattered Spider is a loosely organized "hacker-for-hire" community — often referred to as "The Com" — comprising young individuals primarily from the United States and the United Kingdom. The group reached peak public notoriety in September 2023 with back-to-back ransomware attacks against casino giants MGM Resorts International and Caesars Entertainment, causing hundreds of millions of dollars in losses. By 2025, members of the group had pivoted to working alongside the DragonForce ransomware cartel, conducting high-profile attacks against UK retailers. Concurrent with ongoing criminal operations, law enforcement has made significant progress: multiple members have been charged, sentenced, and convicted, though the broader community continues to operate.

---

## Identity

| Field | Value |
|---|---|
| **Primary Name** | Scattered Spider |
| **Known Aliases** | UNC3944, 0ktapus, Roasted 0ktapus, Octo Tempest, Storm-0875, Storm-0971, Muddled Libra, Scatter Swine, Starfraud, DEV-0971, The Com |
| **MITRE Group ID** | G1015 |
| **Suspected Origin** | United States and United Kingdom (English-speaking) |
| **Active Since** | March 2022 (0ktapus campaign) |
| **Primary Motivation** | 💰 Financial — ransomware, data extortion, cryptocurrency theft |
| **Structure** | Loose-knit collective / "The Com" community, not a single tight-knit gang |
| **Demographic Profile** | Primarily ages 18–26; English-native speakers; US and UK nationals |

---

## Targeting

Scattered Spider has evolved its targeting profile significantly since 2022. The group initially focused on organizations with large external help desks and outsourced IT support functions — specifically telecommunications companies and business process outsourcing (BPO) / CRM firms — where impersonating employees at IT service desks proved highly effective. Beginning in 2023, the group expanded aggressively into high-revenue consumer-facing sectors. By 2024–2025, the group was conducting attacks against healthcare systems and large UK-based retailers, often in partnership with ransomware-as-a-service operators.

### Sectors

- Casinos and Gaming
- Cloud Services and SaaS Providers
- Financial Services
- Healthcare
- Hospitality
- Manufacturing
- Managed Service Providers (MSPs)
- Retail (including UK high-street retailers)
- Technology and Software
- Telecommunications and Business Process Outsourcing (BPO)
- Transportation and Transit

### Regions

- Canada
- India
- United Kingdom
- United States

---

## Capabilities

Scattered Spider demonstrates a high degree of social engineering sophistication that is disproportionate to its members' ages. The group's primary strength lies not in custom malware development but in human manipulation — the actors are highly fluent in corporate terminology and IT procedures, enabling them to convincingly impersonate employees to service desk representatives. Technically, the group combines commoditized phishing kits with off-the-shelf remote access tools, living-off-the-land (LOTL) techniques, and increasingly capable ransomware partnerships. Their rapid adaptation of TTPs — rotating toolsets, infrastructure, and ransomware affiliates — makes them a persistent and evolving threat. Since late 2023, the group has demonstrated capability to navigate complex cloud environments including AWS, Azure, and Okta, indicating growing technical depth.

### Signature TTPs

1. **Help desk vishing / impersonation** — Calling victim IT service desks while accurately impersonating employees; using LinkedIn and data broker research to answer identity verification questions
2. **SMS phishing (smishing) with typosquatted domains** — Crafting victim-specific phishing pages mimicking SSO, Okta, VPN, and MFA portals; 169+ phishing domains identified in the 0ktapus campaign alone
3. **SIM swapping** — Acquiring victims' mobile numbers from carriers to intercept SMS MFA codes
4. **MFA push bombing / fatigue attacks** — Flooding victims with authentication requests until they approve out of frustration or confusion
5. **Okta / Azure / SSO tenant compromise** — Pivoting through identity providers to gain broad access to connected applications
6. **Cloud environment lateral movement** — Using AWS Systems Manager Inventory and similar tools to map and traverse cloud infrastructure post-compromise
7. **Data exfiltration to attacker-controlled cloud storage** — Staging stolen data to MEGA.nz or attacker-controlled Amazon S3 buckets prior to extortion
8. **Ransomware deployment via RaaS partnerships** — Deploying ALPHV/BlackCat, RansomHub, Qilin, and DragonForce as affiliates to encrypt victim environments and enable double extortion

### Known Tools & Malware

- **AnyDesk** — Remote access
- **BlackLotus** — Custom UEFI bootkit (EDR/AV bypass)
- **Chisel** — TCP/UDP tunneling
- **ConnectWise Control (ScreenConnect)** — Remote access
- **CS-Paralyzer** — Custom malware
- **DCSync / secretsdump.py** — Credential dumping
- **LogMeIn** — Remote access
- **Mimikatz** — Credential harvesting
- **ngrok** — Tunneling / reverse proxy
- **Plink** — SSH tunneling (to Telegram)
- **POORTRY** — Kernel driver to terminate security software
- **STONESTOP** — EDR/AV process termination loader
- **Tailscale** — Mesh VPN for covert remote access
- **TightVNC** — Remote access

### Ransomware Variants (as RaaS Affiliate)

- **ALPHV/BlackCat** — Used in MGM and Caesars attacks (2023)
- **DragonForce** — Used in UK retail attacks (2025); DragonForce rebranded as a "ransomware cartel" offering affiliates 80% of profits
- **Qilin** — Adopted Q2 2024
- **RansomHub** — Adopted Q2 2024

---

## MITRE ATT&CK Mapping

| ID | Technique | Tactic |
|---|---|---|
| [T1003](https://attack.mitre.org/techniques/T1003/) | OS Credential Dumping (Mimikatz, DCSync, RAM capture) | Credential Access |
| [T1071](https://attack.mitre.org/techniques/T1071/) | Application Layer Protocol | Command and Control |
| [T1078](https://attack.mitre.org/techniques/T1078/) | Valid Accounts | Defense Evasion, Persistence, Privilege Escalation |
| [T1090](https://attack.mitre.org/techniques/T1090/) | Proxy (rotating machine names, proxy networks) | Command and Control |
| [T1110](https://attack.mitre.org/techniques/T1110/) | Brute Force | Credential Access |
| [T1114](https://attack.mitre.org/techniques/T1114/) | Email Collection (Slack, Teams, Exchange monitoring) | Collection |
| [T1136](https://attack.mitre.org/techniques/T1136/) | Create Account | Persistence |
| [T1219](https://attack.mitre.org/techniques/T1219/) | Remote Access Software (AnyDesk, ConnectWise, LogMeIn) | Command and Control |
| [T1484](https://attack.mitre.org/techniques/T1484/) | Domain Policy Modification | Defense Evasion, Privilege Escalation |
| [T1486](https://attack.mitre.org/techniques/T1486/) | Data Encrypted for Impact (ransomware deployment) | Impact |
| [T1539](https://attack.mitre.org/techniques/T1539/) | Steal Web Session Cookie | Credential Access |
| [T1556](https://attack.mitre.org/techniques/T1556/) | Modify Authentication Process | Credential Access, Defense Evasion |
| [T1562](https://attack.mitre.org/techniques/T1562/) | Impair Defenses (STONESTOP, POORTRY, BlackLotus) | Defense Evasion |
| [T1566](https://attack.mitre.org/techniques/T1566/) | Phishing | Initial Access |
| [T1567](https://attack.mitre.org/techniques/T1567/) | Exfiltration Over Web Service (MEGA, S3) | Exfiltration |
| [T1585](https://attack.mitre.org/techniques/T1585/) | Establish Accounts (fake social media profiles) | Resource Development |
| [T1598](https://attack.mitre.org/techniques/T1598/) | Phishing for Information | Reconnaissance |
| [T1621](https://attack.mitre.org/techniques/T1621/) | Multi-Factor Authentication Request Generation (push bombing) | Credential Access |
| [T1660](https://attack.mitre.org/techniques/T1660/) | Phishing: Smishing | Initial Access |

---

## Timeline & Notable Campaigns

### 2022: Origins — The 0ktapus Campaign

- **March 2022**: 0ktapus campaign launches. The group begins an unprecedented-scale SMS phishing campaign targeting employees at 130+ organizations, including Twilio, Cloudflare, DoorDash, Signal, and Coinbase. 9,931+ accounts compromised. Researchers identify 169 unique phishing domains using keywords: SSO, VPN, OKTA, MFA, HELP.
- **August 2022**: Group1 (Group-IB) publishes first major attribution report linking the campaign to what would become known as Scattered Spider. The campaign's Telegram channel administrator is partially identified; researchers trace activity to a US-based actor.

### 2023: Escalation — Casino Ransomware Attacks

- **June 2023**: CISA begins tracking the group under the name UNC3944; CrowdStrike formalizes the "Scattered Spider" moniker.
- **September 7, 2023**: **Caesars Entertainment attack** — Scattered Spider compromises Caesars' IT support vendor via social engineering. Driver's license numbers and Social Security numbers of a "significant number" of customers are exfiltrated. Caesars reportedly pays $15M of a $30M ransom demand.
- **September 11, 2023**: **MGM Resorts attack** — A 10-minute help desk phone call, using employee information sourced from LinkedIn, enables the group to gain administrator privileges to MGM's Okta and Azure tenants. Slot machines go offline, digital room keys fail, reservations systems shut down for approximately 10 days. Estimated third-quarter losses of ~$100M. ALPHV/BlackCat ransomware deployed. Combined data exfiltration: 6 terabytes across both companies.
- **November 2023**: CISA and FBI publish joint advisory AA23-320A. FS-ISAC and AHA issue sector-specific guidance for financial and healthcare organizations.

### 2024: Law Enforcement Response & RaaS Diversification

- **Q2 2024**: Scattered Spider adds RansomHub and Qilin to its ransomware affiliate portfolio.
- **June 2024**: **Tyler Buchanan ("TylerB")** arrested in Spain by Spanish authorities while attempting to board a flight to Italy. $27M in Bitcoin seized. He is later extradited to the United States (April 2025).
- **Summer 2024**: A 17-year-old from Walsall, UK, is arrested in coordination with the FBI; connected to Scattered Spider operations.
- **November 2024**: US DOJ charges **five defendants** linked to Scattered Spider: Tyler Buchanan, Noah Michael Urban, Ahmed Hossam Eldin Elbadawy, Evans Onyeaka Osiebo, and Joel Martin Evans.
- **Late 2024**: HC3 issues updated healthcare sector advisory following targeting expansion.

### 2025: UK Retail Campaign & Sentencing

- **Early 2025**: Scattered Spider affiliates, operating through the DragonForce ransomware cartel, target major UK retailers:
  - **Marks & Spencer** — Significant operational disruption, online orders and payment systems impacted.
  - **Co-op** — Ransomware deployment; customer and employee data threatened for publication.
  - **Harrods** — Targeted but reportedly contained.
  - **Transport for London (TfL)** — Attack attributed to connected group members.
  - UK **healthcare providers** targeted.
- **2025**: **Noah Michael Urban ("King Bob")**, 21, of Palm Coast, Florida, sentenced to **10 years in federal prison** and ordered to pay **$13M in restitution**.
- **April 2025**: Buchanan transferred to US federal custody following extradition.
- **July 2025**: Updated joint advisory (IC3 AA23-320A revision) published, documenting new TTPs including help desk vishing targeting Microsoft Entra ID, SSO, and VDI accounts; DragonForce ransomware deployment confirmed.

### 2026: Continued Prosecution

- **April 20, 2026**: **Tyler Buchanan** pleads guilty to wire fraud conspiracy and aggravated identity theft, admitting his role in the 2022 phishing campaign and theft of at least **$8M in cryptocurrency**. He becomes the second known Scattered Spider member to plead guilty.
- **Pending**: UK proceedings against **Owen Flowers** (18) and **Thalha Jubair** (20) for attacks on UK retailers, TfL, and healthcare providers. US charges remain active against Elbadawy, Osiebo, and Evans.

---

## Recent Activity (2025–2026)

Scattered Spider remains active despite significant law enforcement pressure. The group's most recent confirmed campaign involves partnering with the DragonForce ransomware cartel to target UK high-street retailers, causing major disruptions to Marks & Spencer, Co-op, and Harrods in early 2025. Tactically, the group has refined its help desk vishing playbook to specifically target Microsoft Entra ID, SSO, and VDI accounts, and has incorporated ETL-based data staging pipelines to aggregate exfiltrated data before uploading to MEGA or attacker-controlled S3 buckets. The July 2025 IC3 advisory indicates the group continues adapting TTPs at a pace consistent with prior years.

### Notable Campaigns (Comprehensive)

- **0ktapus / Operation Oktapus** (March–August 2022) — 130+ organizations, 9,931+ accounts
- **Coinbase breach** (2022) — Linked to 0ktapus infrastructure
- **Caesars Entertainment ransomware** (September 2023) — $15M ransom paid
- **MGM Resorts ransomware** (September 2023) — ~$100M losses
- **UK Retail Campaign: Marks & Spencer, Co-op, Harrods** (Early 2025) — DragonForce deployment
- **Transport for London (TfL) attack** (2025)
- **UK healthcare provider attacks** (2025)

---

## Indicators of Compromise

### IP Addresses

_No specific IPs publicly attributed across sources (group actively rotates infrastructure and uses proxy networks)._

### Domains

- 169 phishing domains identified during the 0ktapus campaign (August 2022), using keywords: **sso**, **vpn**, **okta**, **mfa**, **help** in domain construction (e.g., `[company]-sso[.]com`, `[company]-okta[.]com`)
- Attackers routinely register victim-specific typosquatted domains prior to each campaign

### File Hashes

_No publicly confirmed hashes across analyzed sources._

### CVEs

- **CVE-2015-2291** — Intel Ethernet diagnostics driver for Windows (`iqvw64.sys`); exploited to load STONESTOP/POORTRY kernel driver for EDR/AV termination

### Exfiltration Infrastructure

- **MEGA.nz** — Primary data staging and exfiltration destination
- **Amazon S3** (attacker-controlled buckets) — Secondary exfiltration destination

---

## Member Attribution & Law Enforcement Status

| Individual | Alias | Nationality | Status (as of April 2026) |
|---|---|---|---|
| Tyler Robert Buchanan | TylerB | Scottish (UK) | Pleaded guilty April 2026; wire fraud conspiracy + aggravated identity theft; $8M+ theft admitted |
| Noah Michael Urban | King Bob | US (Florida) | Sentenced 10 years + $13M restitution (2025) |
| Ahmed Hossam Eldin Elbadawy | — | — | Charged (US DOJ, November 2024); case pending |
| Evans Onyeaka Osiebo | — | US (Dallas, TX) | Charged (US DOJ, November 2024); case pending |
| Joel Martin Evans | — | US | Charged (US DOJ, November 2024); case pending |
| Owen Flowers | — | UK | UK charges pending; attacks on retailers, TfL, healthcare |
| Thalha Jubair | — | UK | UK charges pending; attacks on retailers, TfL, healthcare |
| Unidentified 17-year-old | — | UK (Walsall, West Midlands) | Arrested 2024 in coordination with FBI |

---

## Source Articles

| # | Title | Source | Published | Recency |
|---|---|---|---|---|
| 1 | ['Scattered Spider' Member 'Tylerb' Pleads Guilty](https://krebsonsecurity.com/2026/04/scattered-spider-member-tylerb-pleads-guilty/) | Krebs on Security | April 2026 | Recent |
| 2 | [British hacker tied to Scattered Spider campaign pleads guilty in $8M scheme](https://therecord.media/hacker-scattered-spider-guilty-plea) | The Record (Recorded Future) | April 2026 | Recent |
| 3 | [US gets second Scattered Spider-linked guilty plea](https://www.theregister.com/2026/04/20/scattered_spider_linked_scot_plead_guilty/) | The Register | April 2026 | Recent |
| 4 | [Scattered Spider Ramps Up Ransomware In 2025 Cyber Alert](https://cyble.com/blog/scattered-spider-ransomware-in-2025-cyber-alert/) | Cyble | 2025 | Recent |
| 5 | [The DragonForce Cartel: Scattered Spider at the gate](https://www.acronis.com/en/tru/posts/the-dragonforce-cartel-scattered-spider-at-the-gate/) | Acronis | 2025 | Recent |
| 6 | [Deep dive into DragonForce ransomware and its Scattered Spider connection](https://www.bleepingcomputer.com/news/security/deep-dive-into-dragonforce-ransomware-and-its-scattered-spider-connection/) | Bleeping Computer | 2025 | Recent |
| 7 | [Product ID: AA23-320A (Updated July 2025)](https://www.ic3.gov/CSA/2025/250729.pdf) | FBI / IC3 | July 2025 | Recent |
| 8 | [Inside Scattered Spider: Evolving TTPs Exposed](https://www.darktrace.com/blog/untangling-the-web-darktraces-investigation-of-scattered-spiders-evolving-tactics) | Darktrace | 2025 | Recent |
| 9 | [HC3 TLP Clear Threat Actor Profile: Scattered Spider](https://www.aha.org/h-isac-white-reports/2024-10-24-hc3-tlp-clear-threat-actor-profile-scattered-spider-october-24-2024) | HHS HC3 / AHA | October 2024 | Recent |
| 10 | [US Charges Five Alleged Scattered Spider Members](https://www.securityweek.com/us-charges-five-alleged-scattered-spider-members/) | SecurityWeek | November 2024 | Recent |
| 11 | [U.K. Hacker Linked to Scattered Spider Arrested in Spain](https://thehackernews.com/2024/06/uk-hacker-linked-to-notorious-scattered.html) | The Hacker News | June 2024 | Recent |
| 12 | [SCATTERED SPIDER Escalates Attacks Across Industries](https://www.crowdstrike.com/en-us/blog/crowdstrike-services-observes-scattered-spider-escalate-attacks/) | CrowdStrike | 2023–2024 | Historical |
| 13 | [Scattered Spider \| CISA Advisory AA23-320A](https://www.cisa.gov/news-events/cybersecurity-advisories/aa23-320a) | CISA / FBI | November 2023 | Historical |
| 14 | [Scattered Spider & BlackCat Ransomware Mitigation Guidance](https://www.fsisac.com/hubfs/Knowledge/ScatteredSpider&BlackCatRansomware-MitigationGuidance.pdf) | FS-ISAC | November 2023 | Historical |
| 15 | [An Overview of the MGM Cyber Attack](https://netwrix.com/en/resources/blog/mgm-cyber-attack/) | Netwrix | 2023 | Historical |
| 16 | [Scattered Spider, G1015](https://attack.mitre.org/groups/G1015/) | MITRE ATT&CK | Ongoing | Recent |
| 17 | [0ktapus: Twilio, Cloudflare phishers targeted 130+ organizations](https://www.helpnetsecurity.com/2022/08/25/0ktapus-twilio-cloudflare-phishers-targets/) | Help Net Security | August 2022 | Historical |
| 18 | [Defending Against SCATTERED SPIDER and The Com](https://www.sans.org/blog/defending-against-scattered-spider-and-the-com-with-cybercrime-intelligence/) | SANS Institute | 2023 | Historical |

---

## Analyst Notes

**Attribution Complexity:** "Scattered Spider" is not a monolithic group but a loose community known as "The Com" — a sprawling, English-speaking underground network where participants collaborate on specific operations without rigid organizational structure. Different vendors track overlapping subsets of this community under different names (UNC3944 by Mandiant/Google, Octo Tempest by Microsoft, Muddled Libra by Palo Alto Unit 42, Storm-0875 by Microsoft). Indicators attributed to one alias may not apply equally to all actors operating under the Scattered Spider umbrella.

**Law Enforcement Impact:** Multiple arrests and convictions since 2024 represent the most significant disruption to the group to date. However, "The Com" structure means prosecutions of individual members are unlikely to eliminate the collective. UK proceedings against Flowers and Jubair and the pending US cases against Elbadawy, Osiebo, and Evans may yield additional intelligence on TTPs and infrastructure.

**Ransomware Partnership Risk:** The group's willingness to pivot rapidly between ransomware affiliates (BlackCat → RansomHub → Qilin → DragonForce) reflects a mature affiliate-as-a-service operating model and means attribution solely based on ransomware variant is unreliable. The DragonForce "cartel" model (80% affiliate revenue share) is particularly attractive to the group's financial motivation.

**Intel Gaps:** Specific network IOCs (IP addresses, C2 domains active post-2023) are not well-documented in public sources. The group's heavy use of infrastructure rotation and proxy networks degrades IOC utility. Specific member identities beyond those indicted remain unknown. The full scope of 0ktapus-era victim organizations has never been publicly disclosed.

**Healthcare Targeting:** HC3's October 2024 advisory specifically flagged Scattered Spider as an active threat to the US healthcare sector. Healthcare organizations should prioritize help desk verification procedures and out-of-band identity confirmation as primary defensive controls.

---

## Profile Metadata

| Field | Value |
|---|---|
| **Skill Version** | threat-actor-profiler v1.1 |
| **Queries Run** | 7 (WebSearch, supplementing blocked RSS feeds) |
| **Articles Retrieved** | 18+ |
| **Articles Analyzed** | 18 |
| **Articles Skipped** | 0 |
| **Profile Generated** | 2026-04-28 11:00:00 UTC |
| **Last Consolidated** | _Not yet consolidated_ |
| **Updates Merged** | 0 |
