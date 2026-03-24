---
title: TryHackMe - Threat Modelling Walkthrough
date: 2026-03-15T21:08:00
draft: false
tags:
  - thm
  - walkthrough
  - threat-modelling
  - mitre-attck
  - dread
  - stride
  - pasta
  - cybersecurity
description: 'TryHackMe Threat Modelling walkthrough covering MITRE ATT&CK mapping, DREAD risk scoring, STRIDE threat categorization, and PASTA framework application to financial services and e-commerce scenarios.'
cover:
  image: /images/Screenshot 2026-03-15 212805.png
  alt: ''
  caption: ''
  relative: false
---

**Author:** Mario Martinez Jr. (ku5e / Gary7) | TryHackMe USA Rank #76 | Top 1%

**Difficulty:** Easy

**Topics:** Threat Modelling, MITRE ATT&CK, DREAD, STRIDE, PASTA

**Link:** [Threat Modelling on TryHackMe](https://tryhackme.com/room/threatmodelling)

---

Answers are redacted within the narrative to allow you to complete the tasks on your own, but a full table of answers is available at the end of this walkthrough.

This room walks through four threat modeling frameworks used by security teams to identify, categorize, and prioritize risks. You apply each framework to realistic organizational scenarios, including a financial services company and an e-commerce payment processor.

<!-- IMAGE #5: Hero/Banner — ATT&CK Navigator screenshot with post title overlay. See Image Proposals.md -->

---

## Task 1: Learning Objectives

No questions. The room covers MITRE ATT&CK, DREAD, STRIDE, and PASTA, with prerequisites from the Intro to Threat Emulation and Principles of Security rooms.

---

## Task 2: Threat Modelling Overview

Threat modeling is a structured process for identifying and prioritising risks before they become incidents. The room introduces a high-level methodology with distinct phases: scope definition, asset identification, threat identification, and diagramming.

A **vulnerability** is a weakness or flaw in a system, application, or process that an attacker can exploit. It differs from a threat, which is the actor or event that takes advantage of that weakness.

**Asset Identification** is where you build the diagrams that visualize your organization's architecture and dependencies. Getting this step right determines whether the rest of the exercise produces actionable results or generic noise.

An **attack tree** is the specific diagram type used to describe and analyse potential threats against a system or application. It maps out paths an attacker might take from initial access to a target objective, branching at each decision point.

### Technical Deep Dive: Attack Trees

An attack tree starts with a root node representing the attacker's goal, such as exfiltrating customer PII. Each branch beneath it represents a different method to reach that goal. Sub-branches break those methods down further. The result is a visual map of every plausible path to compromise, which lets defenders trace backward from the goal to identify where controls have the highest leverage.

---

## Task 3: Modelling with MITRE ATT&CK

The MITRE ATT&CK framework organises adversary behavior into a matrix of tactics (high-level goals) and techniques (methods used to achieve them). Each technique page contains five sections: technique details, procedure examples from real-world operations, recommended mitigations, detection strategies, and external references.

The room uses **Exploit Public-Facing Application** as the working example. This technique covers attackers targeting software exposed to the internet, ranging from web application vulnerabilities to unpatched services. Its technique ID is **[REDACTED]**, and it falls under the **[REDACTED]** tactic, which covers the methods adversaries use to establish their first foothold in a target environment.

### Technical Deep Dive: Mapping Threats to ATT&CK

After completing your asset inventory and threat identification steps, ATT&CK adds a mapping layer. For each identified threat, you locate the corresponding technique in the matrix and pull its procedure examples, mitigations, and detection strategies. This converts a generic "we might get hacked" concern into a specific adversary behavior with documented countermeasures. For a financial services organization, this means reviewing which techniques are attributed to threat groups known to target that sector, then checking your controls against those specific methods.

---

## Task 4: Mapping with ATT&CK Navigator

The ATT&CK Navigator is an open-source web tool for building custom views of the ATT&CK matrix. You can filter by platform (Windows, GCP, Office 365), search by threat group, annotate techniques with scores and color codes, and export the result as JSON, Excel, or SVG for reporting.

The practical scenario in this task uses a financial services organization running GCP infrastructure with an internal online banking platform and a CRM. Known threat groups targeting that sector include APT28, APT29, Carbanak, FIN7, and Lazarus Group. The Navigator lets you pull all techniques attributed to a threat group with a single search, then layer on platform filters to narrow the matrix to what is actually relevant to your stack.

<!-- IMAGE #1: ATT&CK Navigator Layer — Navigator heatmap with room techniques highlighted. See Image Proposals.md -->

**APT33** has **[REDACTED]** techniques attributed to it in the Navigator. Filtering the matrix to the IaaS platform and looking at the Discovery tactic returns **[REDACTED]** techniques, each of which represents a method an attacker might use to enumerate resources inside your cloud environment.

### Technical Deep Dive: Layering Filters for Realistic Scope

Running a raw ATT&CK matrix for threat modelling produces hundreds of techniques, most of which won't apply to your environment. The correct workflow is to layer filters: start with the platform (GCP, Windows, Azure), add the threat groups active in your sector, then cross-reference with your known attack surfaces. The techniques left after filtering are the ones worth prioritising in your vulnerability remediation backlog.

---

## Task 5: DREAD Framework

DREAD is a qualitative risk scoring model from Microsoft. Each letter maps to a category:

- **Damage**: How bad is the impact if the vulnerability is exploited?
- **Reproducibility**: How easily can an attacker repeat the exploit?
- **Exploitability**: How much effort does launching the attack require?
- **Affected Users**: How many users are impacted?
- **Discoverability**: How easily can an attacker find the vulnerability?

Each category is scored 1-10 and averaged for an overall risk rating. The framework is opinion-based, which means its reliability depends on documented scoring criteria and cross-team review. Without a shared rubric, two analysts can score the same vulnerability differently and produce incompatible results.

<!-- IMAGE #2: DREAD Scoring Table — DREAD score breakdown with numeric values per category. See Image Proposals.md -->

The room walks through three vulnerabilities scored against a defined rubric: Unauthenticated RCE (8.0), IDOR in User Profiles (6.5), and Server Misconfiguration Leading to Information Disclosure (5.0). The highest score does not always mean the most complex vulnerability. Unauthenticated RCE scores higher than the misconfiguration because its damage and affected-user impact are higher, even though the misconfiguration is easier to reproduce.

**[REDACTED]** assesses potential harm from a successful exploit. **[REDACTED]** evaluates how easily an attacker can identify the vulnerability. **[REDACTED]** accounts for how many users are impacted when exploitation occurs.

---

## Task 6: STRIDE Framework

STRIDE is a threat categorisation framework, also from Microsoft, built on the CIA triad. Each letter maps to a threat category and the security property it violates:

<!-- IMAGE #3: STRIDE Framework Diagram — STRIDE categories with security property violations. See Image Proposals.md -->

| Category | Violation |
|---|---|
| Spoofing | Authentication |
| Tampering | Integrity |
| Repudiation | Non-repudiation |
| Information Disclosure | Confidentiality |
| Denial of Service | Availability |
| Elevation of Privilege | Authorization |

The practical value of STRIDE is in systematic coverage. When you decompose a system and run each component through all six categories, you avoid the common failure of only modelling the threats that already occurred to you. The framework forces you to ask about repudiation even when no one on the team mentioned it.

STRIDE is applied to an e-commerce payment processing system in this task. The static site lab walks you through matching attack scenarios to their STRIDE categories and returns a flag upon completion: **[REDACTED]**.

The foundational concept STRIDE builds on is the **[REDACTED]**. **[REDACTED]** violates Confidentiality. **[REDACTED]** covers unauthorised modification of data. **[REDACTED]** covers disruption of availability.

---

## Task 7: PASTA Framework

PASTA (Process for Attack Simulation and Threat Analysis) is a seven-step, risk-centric framework created by Tony UcedaVélez and Marco Morana, published in 2015. Unlike STRIDE, which organises threat categories, PASTA connects threat modelling directly to business objectives and risk tolerance.

<!-- IMAGE #4: PASTA Methodology Flow — Seven-stage PASTA process diagram. See Image Proposals.md -->

The seven steps:

1. **Define the Objectives** - Set security scope and compliance requirements.
2. **Define the Technical Scope** - Build the asset inventory and map architecture.
3. **Decompose the Application** - Break the system into components, entry points, and trust boundaries.
4. **Analyze the Threats** - Identify threat sources using intelligence feeds and known attack patterns.
5. **Vulnerabilities and Weaknesses Analysis** - Scan for existing weaknesses using static analysis, dynamic testing, or penetration testing.
6. **Analyze the Attacks** - Simulate attack scenarios using attack trees and evaluate likelihood and impact.
7. **Risk and Impact Analysis** - Prioritize and implement countermeasures aligned with organizational risk tolerance.

**Decompose the Application** is where you break the system into components. **Analyze the Attacks** is where you simulate attack scenarios. **Define the Technical Scope** is where you create the asset inventory.

The lab scenario covers an online banking platform serving the Asia Pacific region. Completing the seven-step PASTA exercise in the static site returns a flag: **[REDACTED]**.

### Technical Deep Dive: PASTA vs. STRIDE

STRIDE and PASTA are complementary, not competing. STRIDE is strong at cataloguing threat types systematically during design review. PASTA is stronger when you need to tie findings to business risk and present prioritized remediation to stakeholders who think in terms of revenue impact and compliance exposure. Many mature security programs use STRIDE to identify and PASTA to prioritize.

---

## Task 8: Conclusion

No questions. The room closes with a comparison of all four frameworks:

- **MITRE ATT&CK**: Map threats to real adversary tactics and techniques. Best for assessing controls against known threat groups.
- **DREAD**: Score and rank identified threats numerically. Best for communicating prioritized risk to teams that need a clear ranking.
- **STRIDE**: Systematically categorize threats in software systems by security property violated. Best during design and architecture review.
- **PASTA**: Conduct risk-centric modeling aligned with business objectives. Best when findings need to connect to organizational risk tolerance and compliance requirements.

The next rooms in this sequence are Atomic Red Team and CALDERA, which cover threat emulation.

---

## Answer Table

| Task | Question | Answer |
|---|---|---|
| 2 | What is a weakness or flaw in a system that can be exploited by a threat? | vulnerability |
| 2 | What is the process of developing diagrams to visualise architecture and dependencies? | Asset Identification |
| 2 | What diagram describes and analyses potential threats against a system? | attack tree |
| 3 | What is the technique ID of "Exploit Public-Facing Application"? | T1190 |
| 3 | Under what tactic does this technique belong? | Initial Access |
| 4 | How many MITRE ATT&CK techniques are attributed to APT33? | 31 |
| 4 | Upon applying the IaaS platform filter, how many techniques are under Discovery? | 13 |
| 5 | What DREAD component assesses potential harm from exploiting a vulnerability? | Damage |
| 5 | What DREAD component evaluates how others can easily find the vulnerability? | Discoverability |
| 5 | Which DREAD component considers the number of impacted users? | Affected Users |
| 6 | What foundational information security concept does STRIDE build upon? | CIA Triad |
| 6 | What policy does Information Disclosure violate? | Confidentiality |
| 6 | Which STRIDE component involves unauthorised modification of data? | Tampering |
| 6 | Which STRIDE component refers to disruption of system availability? | Denial of Service |
| 6 | Flag for the STRIDE exercise | THM{m0d3ll1ng_w1th_STR1D3} |
| 7 | In which step do you break down the system into components? | Decompose the Application |
| 7 | During which step do you simulate potential attack scenarios? | Analyse the Attacks |
| 7 | In which step do you create an inventory of assets? | Define the Technical Scope |
| 7 | Flag for the PASTA exercise | THM{c00k1ng_thr34ts_w_P4ST4} |

*Written by Mario Martinez Jr. (ku5e / Gary7) | [TryHackMe Profile](https://tryhackme.com/p/ku5e) | [ku5e.com/blog](https://ku5e.com/blog)*
