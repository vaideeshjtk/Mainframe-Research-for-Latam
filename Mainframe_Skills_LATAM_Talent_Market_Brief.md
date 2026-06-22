# Mainframe Specialist Skill Sets & Argentina/Uruguay Talent Market Brief

---

## Executive Summary

The seven skill sets on your list aren't a single talent category — they split into two very different sourcing problems, and that distinction should drive how you plan the meeting.

The first group (Mainframe DB2/IMS application work, CICS/MQ middleware) overlaps with skills that genuinely exist in the Argentine market, because Argentina's large domestic banks (Banco Galicia, Santander Río, BBVA Argentina, Banco Nación, Banco Ciudad, and others) still run COBOL/CICS/DB2 core banking on z/OS, and that has sustained a real local talent pipeline plus global delivery centers (Accenture, IBM, DXC, TCS, EPAM, Globant, Stefanini) staffed out of Buenos Aires and Córdoba.

The second group — true z/OS **systems programming**: IOCDS/HCD, JES2 internals, DB2/IMS *subsystem* installation and zPARM tuning, GDPS/storage replication, SNA/VTAM network administration, REXX/Assembler automation, and RACF/ACF2 database administration — is a globally scarce specialty even in the US and Europe, and it is **dramatically scarcer** in Argentina and especially Uruguay. These are not "developer" jobs you can staff off a freelance platform; they sit inside a handful of bank IT departments and global systems-integrator delivery centers, and the people who hold them tend to be senior (often 45+), with very few mid-career or junior professionals coming up behind them — a pattern confirmed in multiple 2026 industry workforce studies.

For a cloud infrastructure migration specifically, this matters in a practical way: most of what you listed is **"run the legacy estate" expertise**, not **"migrate off the legacy estate" expertise**. You will likely need a small amount of the former (to keep the source system stable and to support data/workload extraction) and a larger amount of migration-specific skills (COBOL/JCL code analysis, data mapping, integration, cloud-native re-platforming) that aren't on this list at all. That's worth raising explicitly in your meeting — see Part 6.

---

## Part 1 — What Each Skill Set Actually Is and What It's Used For

### 1. Mainframe z/OS Admin (z/OS Systems Programmer)
This is the role that keeps the operating system itself alive. A z/OS sysprog installs and upgrades the z/OS operating system, and works at the hardware-definition layer: **IOCDS** (I/O Configuration Data Set — defines how the mainframe's channels, control units, and devices are physically wired together at the hardware level) and **HCD** (Hardware Configuration Definition — the ISPF-based tool used to build and validate that I/O configuration before it's activated). They also own **JES2** (Job Entry Subsystem 2 — the subsystem that manages batch job scheduling, spooling, and output across the mainframe) systems programming, and **speciality engine tuning** — configuring and balancing workload across zIIP and zAAP processors, the lower-cost specialty processors IBM offers for offloading Java, DB2, and other eligible workloads away from the expensive general-purpose CPs. This is the most foundational, and most senior, of the seven roles — everything else runs on top of what this person configures.

### 2. Mainframe DB2 SysProg Admin (DB2 for z/OS Systems Programmer)
Distinct from a DB2 *application* DBA, this is the person who installs and upgrades the DB2 *product itself*, creates and configures DB2 **regions/subsystems**, and implements **DB2 data sharing in a Parallel Sysplex** (the configuration where multiple DB2 subsystems on different mainframe LPARs share the same database, coordinated through the Coupling Facility, for continuous availability and horizontal scale). They manage software maintenance through **SMP/E** (System Modification Program/Extended — the standard IBM tool for installing and tracking software fixes and upgrades) and **user mods** (locally written or vendor-supplied patches applied outside the standard maintenance stream), and they tune **DB2 zPARMs** — the hundreds of system-level configuration parameters that govern memory, buffer pools, logging, locking, and overall subsystem performance.

### 3. Mainframe IMS Sys DBA (IMS Systems DBA)
IMS (Information Management System) is IBM's hierarchical database and transaction manager, still core to many of the world's largest insurance and banking back-ends. This role installs and upgrades the IMS product, establishes the connection between the **IMS DB** (database) and **IMS DC** (Data Communications — the transaction-processing front end), and creates the **IMS DC** environment that lets online transactions reach the database. It also covers IMS **data sharing in a Parallel Sysplex** (similar concept to DB2 data sharing, but for IMS), integrates with the **security interface** (typically RACF or ACF2, since IMS itself doesn't do its own security), and performs systems-level performance tuning and capacity planning. IMS specialists are now one of the rarest skill sets in the entire mainframe ecosystem globally, because IMS has a much smaller installed base than DB2.

### 4. Mainframe Storage Admin
This role manages the physical and logical storage layer and, critically, **disaster recovery replication**. **DFSMS** (Data Facility Storage Management Subsystem — note: your list shows "DSSK," which is very likely a typo for DFSMS, the standard z/OS storage management subsystem) governs automated storage allocation, backup, and space management. **DFSMShsm** ("CSM" in your list is most likely **DFSMShsm**, the Hierarchical Storage Manager component, or possibly TPC-R/Copy Services Manager — both plausible depending on the shop) handles migration of data between storage tiers. **Global Mirror** and **Metro Mirror** are IBM's two flagship storage replication technologies: Metro Mirror is synchronous replication for short-distance, zero-data-loss disaster recovery; Global Mirror is asynchronous replication for longer-distance DR where some data loss in a true disaster is acceptable in exchange for distance. **Encryption** covers data-at-rest encryption on DASD/tape. **GDPS** (Geographically Dispersed Parallel Sysplex) is IBM's automation layer that orchestrates failover across sites using this replication — it's the gold-standard mainframe DR/continuous-availability solution for banks. **TDMF** and **GDMF/GGM** are third-party (Broadcom/INNOVATION) data migration tools used to move data between storage devices or sites, often during hardware refreshes or data center moves — directly relevant to a cloud migration's data extraction phase.

### 5. Mainframe Middleware: CICS & MQ
**CICS** (Customer Information Control System) is the transaction-processing monitor that runs the vast majority of mainframe online banking, retail, and insurance transactions. **MQ** (formerly MQSeries) is IBM's enterprise message queuing middleware, used to integrate the mainframe with distributed and cloud systems asynchronously. This role covers MQ installation, customization, and **queue depth management** (sizing and monitoring queues to prevent message backlogs that can stall downstream processing); **CICSPlex** installation and maintenance (CICSPlex SM is the systems-management layer that lets you administer many CICS regions as one logical group); **CSD** routing table management (your list says "CIOS," almost certainly a typo for **CSD** — the CICS System Definition data set that holds resource definitions — or possibly "EXCI/CICS routing"); and **CICS region implementation and cloning** (standing up new CICS regions and replicating configurations for scale or environment promotion). This is one of the more transferable skill sets toward a cloud migration, since MQ in particular is a natural integration bridge to cloud-native services.

### 6. Mainframe Network (SNA/VTAM Administrator)
This is mainframe networking and connectivity, spanning both legacy and modern protocols. **SNA/VTAM** (Systems Network Architecture / Virtual Telecommunications Access Method) is IBM's original mainframe networking architecture, still used for terminal access and some legacy application connectivity. The role also covers standard **TCP/IP, UDP, FTP/FTPS/SFTP**, **Enterprise Extender** (a technology that tunnels SNA traffic over IP networks, letting shops retire pure SNA hardware while keeping SNA-dependent applications running), and **Connect:Direct** (a high-volume, scheduled file-transfer protocol widely used for B2B and interbank file exchange). **OSA** (Open Systems Adapter) is the physical network card for mainframes, and **OSC/OSD/OSE** refer to its different operating modes (OSA channel modes such as OSE for SNA/APPN/HPR, OSD for QDIO/TCP-IP, etc.). **LPAR VIPA** ("LRS VPS" in your list is most likely a typo for something like **VIPA — Virtual IP Addressing**, a mainframe TCP/IP high-availability technique) and **HOD** (Host On-Demand, IBM's browser-based terminal emulation product) round out connectivity. Certificate implementation/renewal and **SKMF** (likely a typo for **ICSF/SAF key management**, or a vendor-specific key management facility) cover the cryptographic side of network security. This is a genuinely rare specialization — mainframe network engineers who still know SNA/VTAM well are a shrinking, aging population worldwide.

### 7. Mainframe Automation
This role builds the automated operations layer on top of everything else, using commercial tools — **CA OPS/MVS** (Broadcom), **IBM System Automation (SA)**, and **BMC Mainview** — to install, customize, and define automated responses to system events, including **time-of-day rules** (scheduling automated actions, such as starting/stopping subsystems or running housekeeping jobs, on a calendar/clock basis). Underneath the tools, this role requires deep **REXX** programming (the primary mainframe scripting language for automation, used heavily inside all three tools above), **Assembler** language proficiency (for low-level exits and performance-critical routines), and **CLIST** (an older, ISPF-native scripting language, largely superseded by REXX but still present in many legacy environments). This is the skill set most directly reusable for building bridges, monitoring hooks, and automated handoffs between the mainframe and a cloud target environment during a migration.

### 8. Mainframe Security: RACF & ACF2 Administrator
RACF (Resource Access Control Facility, IBM's native security manager) and ACF2 (Broadcom's competing product) both control who can access what on the mainframe — datasets, transactions, programs, and resources. This role covers **digital certificate** management and **digital ring** ("keyring") implementation (used for TLS/SSL on the mainframe), **passphrase** implementation (RACF's longer, more secure alternative to traditional 8-character passwords), and full **database administration** of the RACF or ACF2 security database itself — creation, modification, and ongoing maintenance — plus the **SMP/E** process for installing and maintaining the security product's own software. Note: a shop runs *either* RACF *or* ACF2 (rarely both, except during a migration between them), so "RACF & ACF2" experience together typically signals either a consultant who has worked across multiple client environments, or an active project to migrate from one to the other.

---

## Part 2 — Why This Talent Is Hard to Find Anywhere (Global Context)

Before narrowing to Argentina and Uruguay, it's worth grounding expectations: this is a global problem, not a regional one. Industry survey data shows 62% of organizations now cite a lack of modern mainframe skills as their single biggest hiring barrier, a sharp jump from just a few years ago, even as 71% of organizations report running hybrid or cloud-integrated mainframe environments — meaning demand is shifting toward people who understand *both* legacy and modern/cloud architecture, which is an even smaller pool than legacy-only specialists. The average mainframe specialist is now over 50 years old, and only about 7% of the mainframe workforce is under age 30, largely because university curricula rarely teach mainframe skills anymore, so the talent pipeline behind today's senior specialists is thin almost everywhere. At the same time, 91% of companies say they plan to hire for mainframe roles in the next two years, which means demand is rising even as supply ages out — a dynamic that pushes up rates and lengthens sourcing timelines for the niche systems-programming roles in particular.

---

## Part 3 — Argentina Talent Market

Argentina is genuinely one of the stronger nearshore IT markets in Latin America, but the strength is concentrated in **application-layer** mainframe skills, not infrastructure/sysprog depth.

**What's realistically available:** Active job postings and recruiting activity in Buenos Aires show ongoing demand and supply for COBOL development in mainframe z/OS environments, including CICS, DB2, JCL, and increasingly z/OS Connect work to expose mainframe services as APIs — this last point is actually a good sign for a migration project, since several Argentine teams are already doing mainframe-to-API integration work as part of modernization projects, not pure legacy maintenance. Postings also surface roles for mainframe DB2 stored-procedure development, environment management, and Control-M scheduling, and demand comes from a recognizable mix of global system integrators (Stefanini, Accenture, Adecco's technology division) and domestic banks.

**What's scarce-to-absent:** Targeted searches for the *systems programming* side of your list — RACF/ACF2 administration, DB2 zPARM tuning, IMS sysprog, GDPS/storage replication, SNA/VTAM administration — turn up almost no Argentina-specific local job postings or salary data. That's a meaningful signal: these roles are not absent from Argentina entirely (the local banks running z/OS obviously have *someone* doing this work), but they are not roles that get openly recruited on the local job market the way developer roles do. In practice, this talent sits inside (a) the in-house infrastructure teams of the major Argentine banks, who rarely release people to outside contracts, and (b) the Argentina/regional delivery centers of global SIs — IBM, Accenture, DXC, TCS, Capgemini, Kyndryl — who staff these specialists across multiple clients and don't advertise them as discrete local vacancies. Sourcing these roles will realistically mean going through one of those global SIs or a specialist mainframe staffing boutique (e.g., outfits like CPT Global, Vertali, or similar firms that specifically broker mainframe systems-programming talent), rather than posting a job and expecting local applicants.

**Macro context worth knowing:** Argentina's overall software engineer talent pool is large and well-regarded — average software engineer compensation in Argentina runs around $63,000 per year, the highest average among the Latin American markets tracked, and Argentina's roughly 150,000 developers increasingly work under USD-denominated contracts specifically to navigate the country's persistent currency inflation. That inflation/currency dynamic is a real consideration for any multi-year engagement: expect contracts to be USD-pegged, and budget for some rate renegotiation risk over a long migration program.

---

## Part 4 — Uruguay Talent Market

Uruguay is a smaller, higher-quality, but **much thinner** pool for mainframe work specifically — and for the systems-programming roles on your list, it's close to a non-market.

**Overall market shape:** Uruguay's IT sector is small relative to Argentina or Brazil — estimates range from roughly 14,000 to 33,000 developers depending on the source and definition used, against a population of about 3.5 million. Uruguay's strongest verticals are fintech, banking, AI/ML, IoT, and enterprise software, and the country has built a genuine reputation for institutional stability — it holds an EU Adequacy Decision recognizing its data protection laws as GDPR-equivalent, and ranks first in Latin America for ease of doing business. IBM, TCS, and Sabre all run regional delivery or consulting centers out of Montevideo, which is a relevant data point — it means there *is* an enterprise-grade IT services presence in-country, but the work those centers do skews toward modern stacks (cloud, fintech platforms, travel-tech) rather than legacy mainframe infrastructure.

**Mainframe specifically:** Uruguay's banking sector is small (a handful of major banks: Banco República/BROU, Santander, Itaú, Scotiabank, BBVA), so the domestic z/OS installed base — and therefore the domestic talent pool trained on it — is correspondingly small. We found no evidence of an active, advertised local market for mainframe systems programming, DB2/IMS sysprog, RACF/ACF2 administration, or SNA/VTAM roles in Uruguay; the country's tech-talent narrative is built almost entirely around modern languages, mobile, and cloud platforms. If you need Uruguay-based mainframe systems-programming talent specifically, the realistic options are: (a) a handful of individuals inside the few local banks' own IT departments (not generally available for outside contracts), (b) IBM's or TCS's Montevideo delivery centers, who may be able to pull in resources from elsewhere in their LATAM network even if not locally based, or (c) treating "Uruguay" as a legal-entity/EOR (Employer of Record) location for *Argentine* or other regional mainframe talent working remotely — which is a common structure nearshore staffing firms already use, since Uruguay's business-friendly regulatory environment makes it an attractive EOR jurisdiction even when the actual engineer is elsewhere in the region.

**Cost context:** Uruguay's cost of living runs roughly 93% higher than Argentina's, which feeds directly into compensation — average Uruguayan software engineer compensation is about $61,000 per year, very close to Argentina's average despite the much smaller talent pool, but without Argentina's currency-inflation volatility, since Uruguay's peso is comparatively stable. For any role you can actually find in Uruguay, expect to pay roughly Argentina-equivalent rates, with less negotiating leverage given the thinner bench.

---

## Part 5 — Rate Card

A direct, sourced rate card for "z/OS systems programmer in Buenos Aires" doesn't exist in public market data — these roles simply aren't traded on the open job-board market the way general developer roles are. What follows triangulates three things: (1) directly sourced general nearshore developer rates for Argentina/Uruguay, (2) directly sourced **US/global mainframe systems-programming contractor rates** as an anchor, and (3) a reasoned scarcity premium applied to the nearshore baseline — since niche mainframe infrastructure skills consistently command 30–80% above general senior-developer rates in every market where data exists. Treat the mainframe-specific figures as **planning ranges to validate with 2–3 actual vendor quotes**, not quoted prices.

### Anchor data points (directly sourced)

| Market | Role type | Rate | Source basis |
|---|---|---|---|
| Argentina | General senior developer (nearshore) | $35–55/hr | Curotec 2026 LATAM rate benchmarking |
| Argentina | Senior nearshore engineer, agency-billed, international project experience | $50–90/hr | ParallelStaff 2026 nearshore cost guide |
| Argentina | Average FTE software engineer (all levels, verified payroll data) | ~$63,000/yr | Howdy 2026 LatAm benchmark (12,500+ payroll records) |
| Uruguay | Average FTE software engineer (all levels, verified payroll data) | ~$61,000/yr | Howdy 2026 LatAm benchmark |
| Uruguay | Offshore/nearshore blended rate | ~$61/hr (regional average cited) | Uvik 2026 offshore rate guide |
| US (onshore anchor) | z/OS / DB2 / RACF systems programmer, contract | $47–74/hr (many postings cluster $49–$74) | ZipRecruiter live postings, Dec 2025; ZipRecruiter RACF-specific postings, Jun 2026 |
| US (onshore anchor) | DB2 DBA on z/OS, contract | $38–68/hr | ZipRecruiter, Jun 2026 |
| US (onshore anchor) | z/OS storage systems programmer, salaried | up to ~$150,000/yr | SPCI mainframe jobs board, Apr 2026 |

### Planning estimates — Argentina & Uruguay mainframe specialist rates (Claude's synthesis, not directly quoted)

| Skill Set | Availability (AR) | Availability (UY) | Est. nearshore hourly (USD) | Est. FTE annual equivalent (USD, fully loaded) |
|---|---|---|---|---|
| z/OS Admin (sysprog, IOCDS/HCD, JES2, speciality engines) | Low (concentrated in banks/GSIs) | Very low | $55–90/hr | $90,000–130,000 |
| DB2 SysProg Admin (zPARM tuning, data sharing) | Low–moderate | Very low | $50–80/hr | $80,000–120,000 |
| IMS Sys DBA | Very low (IMS shrinking globally) | Negligible | $60–95/hr | $95,000–140,000 |
| Storage Admin (GDPS, Metro/Global Mirror, encryption) | Low | Very low | $55–85/hr | $90,000–125,000 |
| CICS & MQ Middleware | Moderate (most "available" of the seven) | Low | $40–65/hr | $65,000–95,000 |
| Network (SNA/VTAM) | Low | Negligible | $50–80/hr | $80,000–115,000 |
| Automation (OPS/MVS, SA, Mainview, REXX/Assembler) | Low–moderate | Very low | $45–75/hr | $75,000–110,000 |
| Security (RACF/ACF2 admin) | Low | Very low | $50–80/hr | $80,000–115,000 |

A few things to flag about these numbers before your meeting: first, the "low/very low" availability ratings mean that for several of these roles, a credible vendor quote may come back at or above the *US onshore* contract rate, because the supplier is really just allocating a senior person from a global bench rather than a true low-cost local hire — ask any vendor directly how many in-country, dedicated resources they have for each specific skill before accepting a "nearshore rate" framing. Second, FTE/permanent hiring for these specialties in Argentina or Uruguay is genuinely difficult — given the small pool and aging demographic, a contract/staff-augmentation model through an established mainframe-specialist vendor is almost always more realistic than a direct-hire search. Third, Argentina's currency volatility means any quote should be USD-denominated with an explicit rate-review clause if the engagement runs more than 12 months.

---

## Part 6 — How This Maps to Your Cloud Infrastructure Migration

This is the most important strategic point to bring into the meeting. Looking at your list against what an actual mainframe-to-cloud migration program needs, there's a mismatch worth naming explicitly:

**What this skill list is good for:** keeping the source mainframe environment stable, secure, and performant *while* a migration is underway (which can take 18–36+ months for a large core-banking estate), supporting data extraction and replication out of the platform (storage admin's TDMF/GDMF and Global Mirror/Metro Mirror experience is directly reusable here), and providing the connectivity bridge during a hybrid/coexistence period (MQ and network/Enterprise Extender skills are genuinely migration-relevant, not just "run" skills).

**What's notably absent from this list, and what you'll also need:** application-side skills to actually understand and re-platform the COBOL/PL1 codebase and JCL batch logic (code analysis, dependency mapping, often supported by AI-assisted modernization tooling); data architects who can map VSAM/DB2/IMS hierarchical and relational structures to target cloud data stores; integration engineers who build and test the API layer (the z/OS Connect work showing up in Argentina job postings is a good early signal that this skill exists locally); cloud-native platform engineers on the receiving end (AWS/Azure/GCP, Kubernetes, modern CI/CD); and a migration program/cutover manager who has actually run a mainframe decommissioning before, since the failure mode in these programs is almost never "we couldn't build the new system," it's "the cutover and parallel-run/reconciliation process went wrong."

A practical recommendation for the meeting: split your sourcing into two tracks. Track one — "keep the source system safe" — staff with a small number (often 2–5 people total, blended across skill sets, since one senior generalist sysprog can often cover 2–3 of your eight categories) of senior mainframe infrastructure specialists, sourced through a global SI or specialist mainframe boutique rather than the open Argentina/Uruguay job market, likely on a time-and-materials or managed-service contract. Track two — "build the target state" — staff much more heavily from Argentina's genuinely deep and more reasonably priced application/cloud developer pool, where the market data in this brief is far more reliable and the talent is far more available.

---

## Part 7 — Questions Worth Raising in Your Meeting

A few open items this research couldn't fully resolve, and that are worth surfacing directly with whoever is running the meeting: what's the actual current size and age profile of the in-house mainframe infrastructure team today, and is the migration partly motivated by succession risk on that team (a very common driver); is the organization open to sourcing these specific eight specialist roles through a global SI/mainframe boutique rather than direct hire, given how concentrated and non-public this talent is; what's the realistic program length, since that determines whether a 12-month nearshore contract rate or a 30+ month managed-service arrangement is the better commercial structure; and whether "Argentina and Uruguay" is a hard requirement or a starting hypothesis — given how thin the Uruguay-specific bench is for these particular skills, it may be worth widening the lens to include Brazil (similarly large bank-driven mainframe install base) or hybrid sourcing (Argentina for the bulk, global SI delivery centers for the rarest 1–2 roles like IMS).

---

## Sources

- Planet Mainframe, "Mainframe Careers Are Changing, Not Disappearing: What to Expect" (Jan 2026) — planetmainframe.com
- Ensono, "The Mainframe Branding Problem and Its Impact on the Skills Shortage" — ensono.com
- Tech Talent Solutions / Per Scholas, "The Mainframe Talent Gap: 7 Numbers Tech Leaders Need To Know" — enterprise.perscholas.org
- Glassdoor / Indeed Argentina mainframe & COBOL job listings (2025–2026 postings)
- ZipRecruiter, US mainframe systems programmer / DB2 DBA / RACF contract rate listings (2025–2026)
- Curotec, "2026 Nearshore Developer Hourly Rates by Country in Latin America" — curotec.com
- DevNearshore.com, nearshore rate comparison (2025)
- Uvik Software, "Offshore Dev Rates by Country 2026" — uvik.net
- DistantJob, "Offshore vs. Nearshore vs. Onshore Outsourcing: 2026 Developer Rates" — distantjob.com
- Howdy, "2026 LatAm Software Engineer Cost Benchmarks by Country" — howdy.com
- ParallelStaff, "How Much Does Nearshore Software Development Cost in 2026?" — parallelstaff.com
- N-iX, "Uruguay software development outsourcing in 2026" — n-ix.com
- Hyperion360, Uruguay outsourcing country profile — hyperion360.com
- Accelerance, Uruguay country profile — accelerance.com
- Gini Talent, "Top 10 Technology Recruitment Agencies in Argentina" — ginitalent.com
- Svitla, "Top 5 Nearshore Software Development Companies in Argentina" — svitla.com

*Note on methodology: where this brief presents specific dollar ranges for the eight specialist roles (Part 5, second table), these are reasoned planning estimates built from the sourced anchor data plus a scarcity-premium adjustment — not directly quoted vendor pricing. Treat them as a starting point for budget conversations, and validate against 2–3 actual vendor proposals before committing to a number.*
