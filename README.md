# IoT Security Strategy & Compliance Framework — Hill Valley Power Systems Ltd

**MSc Cybersecurity | Coventry University | Security of Connected Systems (Security of Connected Systems)**  
**Module Grade: 72%**

---

## Overview

This project delivers a comprehensive enterprise security strategy for a real-world IoT energy management company operating across 10,900 UK installations. The work covers organisational change strategy, secure design methodology, IoT-specific threat modelling, penetration testing planning, regulatory compliance mapping, and a phased 18-month roadmap to ISO 27001 / SOC 2 certification — all grounded in verified CVE case studies from comparable industrial platforms.

The report addresses two Chartered Management Institute units: CMI Unit 704 (strategic leadership) and CMI Unit 705 (leading organisational change), applying both business strategy and technical security frameworks to a commercially realistic IoT deployment scenario.

---

## Business Context

**Hill Valley Power Systems Ltd (HVPS)** is a fictional Coventry-based IoT energy management company with:
- 47 employees, £3.2M revenue, targeting £12M by 2028
- 2,400 commercial and 8,500 residential smart energy installations
- Product range: Flux-88 Gateway, Flux-121 Hub, Flux-Sense Sensors, and Flux Capacitor cloud API (AWS-hosted)
- £4.2M Series A investment from Clocktower Capital — with ISO 27001 or SOC 2 certification required within 18 months as a binding investor condition
- Planned expansion into Germany, Netherlands, and Ireland — blocked by absent BSI TR-03148 certification
- Active security gap: API vulnerability discovered by external researchers (Strickland Security Research Group) in late 2024, requiring 72-hour emergency patching

---

## Key Deliverables

### 1. Strategic Security Analysis

**PESTLE Environmental Scan**  
Identified legal, technological, and market pressures driving security investment urgency:
- UK PSTI Act 2022 — mandatory unique device credentials and vulnerability disclosure policy for all products shipped after April 2024
- EU Cyber Resilience Act — extending obligations to all EU markets targeted for 2028 entry
- GDPR, NIS2, and EU AI Act — applicable to data handling and incident reporting across planned expansion markets
- German market entry blocked without BSI TR-03148 smart metering certification

**Porter's Five Forces**  
Established that security certification functions as a market access credential in the UK building automation sector. Commercial customers are increasingly requiring penetration test reports and security documentation before contract award — HVPS had already experienced contract losses as a direct result of this gap.

**VRIO Internal Capability Assessment**  
Confirmed that HVPS's Flux product platform and 10,900-installation base provide a foundation for competitive advantage, but that the current security posture (zero dedicated security personnel, one penetration test since 2019) is an active weakness rather than a resource.

**McKinsey 7S Model**  
Mapped current and target state across all seven organisational elements:

| Element | Current State | Target State |
|---|---|---|
| Strategy | Growth focus; security as overhead | Security-as-enabler positioning |
| Structure | Zero dedicated security personnel | Fractional vCISO + Security Champions |
| Systems | No formal SDL | Microsoft SDL + STRIDE in sprint planning |
| Shared Values | Move fast, ship product | Secure and fast delivery culture |
| Staff | Zero security specialists | vCISO + 5 Security Champions |
| Skills | No secure coding training | 100% of developers STRIDE-trained |

---

### 2. Secure Design Strategy

**Framework Evaluation**

Three frameworks were assessed against HVPS's specific context:

| Framework | HVPS Applicability | Decision |
|---|---|---|
| Microsoft SDL + STRIDE | High — STRIDE runs in 30–60 min sprint sessions; no security specialists required | **Selected** |
| OWASP SAMM | Medium — maturity-based but generic; requires IoT overlay | Supplementary |
| OWASP IoT Security Guidelines | High — directly addresses Flux device attack surfaces | **Selected as overlay** |

**STRIDE Threat Model — Flux Product Range**

Full STRIDE threat model developed across all product layers:

| Threat Category | Flux Capacitor API | Flux-88 / Flux-121 / Flux-Sense |
|---|---|---|
| Spoofing | OAuth 2.0 token forgery; API key impersonation | Device identity spoofing on MQTT broker; unauthenticated firmware updates |
| Tampering | REST API payload manipulation; unauthorised write to building controls | Firmware modification via UART/JTAG; tampered OTA update packages |
| Repudiation | Incomplete or deletable API call logs | Device audit trail absent or overwritable via physical access |
| Information Disclosure | Plaintext credentials in API responses (cf. CVE-2021-36204) | Credentials in firmware plaintext; Wi-Fi PSK readable from flash memory |
| Denial of Service | Rate limiting absent on API; unauthenticated endpoint resource exhaustion | Device lockout via malformed MQTT payload; Zigbee radio jamming |
| Elevation of Privilege | Horizontal privilege escalation across 2,400 commercial site accounts | Root shell via UART; arbitrary code execution via JTAG boot loader access |

**Strategic Innovation: Security-as-Literacy**  
Rather than a centralised security function (which creates throughput bottlenecks at scale), the strategy embeds security capability directly into the engineering team through a Security Champions programme. Maintaining a 20% champion ratio (5 for 25 developers, scaling to 24 for the 120-employee target) means security capacity scales with hiring rather than requiring discrete investment decisions.

---

### 3. Security Audit Strategy

**Methodology Evaluation**

| Methodology | HVPS Applicability |
|---|---|
| PTES (Penetration Testing Execution Standard) | High — enterprise credibility; clear deliverables; augmented with IoT-specific testing |
| OWASP Web Security Testing Guide | Medium — valuable for Flux Capacitor API only; insufficient for firmware |
| NIST SP 800-115 | Medium — network testing elements applicable; US-centric overall |
| OWASP IoT Testing Guide + ETSI TS 103 701 | High — firmware analysis, hardware interfaces, MQTT and Zigbee coverage |

**Recommended Approach: PTES + IoT Augmentation**

PTES provides the structural framework that enterprise customers and certification auditors expect. IoT augmentation adds coverage of attack surfaces that standard penetration testing frameworks miss:

- **Firmware analysis** using Binwalk (extraction) and EMBA (automated vulnerability identification)
- **Hardware interface testing** via UART and JTAG physical access probing
- **Wireless testing** covering Zigbee pairing security, Wi-Fi credential storage, and Bluetooth Low Energy
- **MQTT testing** covering authentication requirements, topic access controls, and payload validation

**Distributed Architecture Adaptations**  
PTES was designed for single-organisation perimeter testing. Three explicit adaptations were defined for HVPS's 10,900-installation distributed architecture:

1. Intelligence Gathering must shift from single-target enumeration to pattern analysis — a weakness in Flux Capacitor OAuth token management exposes all 2,400 commercial installations simultaneously
2. Exploitation testing must validate the full cascade chain: device compromise → cloud infrastructure lateral movement → API pivot → Business Portal access across the commercial portfolio
3. Reporting must present deployment-adjusted risk ratings alongside CVSS scores — a High-severity finding affecting 10,900 installations simultaneously carries Critical-level organisational risk

---

### 4. Case Study — Johnson Controls Metasys CVEs

Two verified industrial control system vulnerabilities were analysed as directly comparable failure patterns to the HVPS Flux Capacitor platform:

**CVE-2021-36204 | CVSS 7.8 | CWE-522 | CISA Advisory ICSA-23-012-06**  
Metasys ADS/ADX/OAS servers exposed plaintext credentials through API calls under certain conditions, affecting versions prior to 10.1.6 and 11.0.3. Identified post-deployment and reported to CISA by Johnson Controls.

**CVE-2025-26385 | CVSS 10.0 | CWE-77 | CISA Advisory ICSA-26-027-04**  
Special elements in user-supplied input not neutralised before command execution, enabling remote SQL execution. Affected ADS, ADX, LCS8500, NAE8500, SCT, and CCT components. CISA classified affected sectors as Commercial Facilities, Critical Manufacturing, Energy, Government Services, and Transportation Systems.

**Process Failures Mapped to HVPS Equivalent Risks**

| Process Failure | Metasys CVE | HVPS Equivalent Risk |
|---|---|---|
| No credential protection standard in API design | CVE-2021-36204 | Flux Capacitor REST APIs have no documented API security standard |
| No input validation for command execution | CVE-2025-26385 | No formal secure coding standard; basic input validation only |
| No formal Secure Development Lifecycle | Both | No SDL; security responsibilities distributed informally across Software and QA |
| No API security testing in QA | CVE-2021-36204 | One penetration test since 2019; no API security testing in CI/CD pipeline |
| No proactive vulnerability identification | Both | Both CVEs identified post-deployment — same failure pattern as Strickland disclosure |

Both vulnerabilities reached production undetected across multiple release cycles in a mature, well-resourced company — on a platform directly comparable to the Flux Capacitor environment. The Strickland disclosure followed the same pattern.

---

### 5. Organisational Change Strategy

**Change Typology: Reconstruction (Balogun & Hope Hailey)**  
Rapid, wide-scope change without a fundamental business model shift — investor timeline drives pace; security is layered onto an existing growth-focused business.

**Kotter's 8-Step Model**  
Programme mapped across all eight steps: urgency established via investor deadline and Strickland disclosure; guiding coalition formed as Security Steering Committee (CEO, CTO, vCISO, Security Champions lead, Clocktower Capital representative); short-term win anchored to Cyber Essentials Plus at month 6.

**ADKAR — Developer Resistance Management**  
Developer resistance identified as the primary friction point. Key interventions:
- Awareness: CEO communication linking security to lost contracts and Strickland incident
- Desire: Security Champions programme framed as professional development
- Knowledge/Ability: Mandatory STRIDE training in Phase 2; vCISO coaches first three sprint sessions
- Reinforcement: STRIDE findings treated as process improvement, not individual blame

**Lewin's Force Field Analysis**  

Driving forces: Clocktower Capital certification deadline, commercial contract losses, PSTI Act 2022 obligations, Strickland API vulnerability, German market entry blocked, EU Cyber Resilience Act.

Restraining forces: Limited capital, zero internal security expertise, developer time pressure, cultural inertia from speed-first culture since 2019, vCISO bandwidth dependency.

---

### 6. Implementation Roadmap

| Phase | Timeline | Key Deliverables | Success Metric |
|---|---|---|---|
| Phase 1: Quick Wins | Months 0–6 | vCISO engaged; MFA enforced; 5 Security Champions; Incident Response Plan | Cyber Essentials Plus certification |
| Phase 2: Foundation | Months 6–12 | STRIDE in sprint planning; developer training; vulnerability disclosure policy; PTES penetration test | 100% developer training completion; disclosure policy live |
| Phase 3: Maturity | Months 12–18 | ISO 27001 or SOC 2 certification | Certification achieved by month 18 |
| Phase 3 Extension: BSI | Months 18–24 | BSI TR-03148 certification process | German market entry enabled |

---

### 7. Regulatory Compliance Mapping

| Requirement | Regulation | Market | Programme Element |
|---|---|---|---|
| Unique default credentials per device | PSTI Act 2022 | UK (all products post April 2024) | SDL Design phase; IoT security checklist |
| Vulnerability disclosure policy | PSTI Act 2022; EU CRA; Investor condition | UK and EU | Phase 2; month 9 target |
| Information security management certification | ISO 27001 | Investor requirement; UK commercial | Phase 3; month 18 target |
| Connected product cybersecurity requirements | EU Cyber Resilience Act | Germany, Netherlands, Ireland | SDL + OWASP IoT foundation |
| Smart metering certification | BSI TR-03148 | Germany specifically | Phase 3 Extension; months 18–24 |
| Data handling and incident reporting | GDPR; NIS2 | All EU markets | Incident Response Plan; disclosure policy |
| Consumer IoT baseline security | ETSI EN 303 645 | EU and UK | IoT security checklist; PTES augmentation |

---

## Frameworks and Standards Applied

**Security Frameworks:** Microsoft SDL, STRIDE Threat Modelling, OWASP SAMM, OWASP IoT Security Guidelines, OWASP Web Security Testing Guide, PTES, NIST SP 800-115, NIST SP 800-218 (SSDF), ETSI TS 103 701, ETSI EN 303 645

**Compliance Standards:** ISO 27001, SOC 2, Cyber Essentials Plus, BSI TR-03148, PSTI Act 2022, EU Cyber Resilience Act, GDPR, NIS2

**Business Strategy Frameworks:** PESTLE, Porter's Five Forces, VRIO, McKinsey 7S, Balanced Scorecard, SAFe (Suitability, Acceptability, Feasibility)

**Change Management Models:** Kotter's 8-Step Model, ADKAR, Lewin's Force Field Analysis, Balogun and Hope Hailey Change Typology

**IoT Testing Tools Referenced:** Trivy, Binwalk, EMBA, Ubertooth (Zigbee), UART/JTAG hardware probing, MQTT protocol analysis

---

## Academic Context

This report was submitted for **Security of Connected Systems — Security of Connected Systems** at Coventry University as part of the MSc Cybersecurity programme (2025–2026). Module grade: **72%**.

The report satisfies CMI Unit 704 (Strategic Leadership) and CMI Unit 705 (Leading Organisational Change) learning outcomes, applying both management and technical security disciplines to a commercially realistic IoT deployment scenario.
