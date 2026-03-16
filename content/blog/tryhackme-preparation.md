---
title: 'TryHackMe: Preparation'
date: 2026-03-15T20:31:00
draft: false
tags:
  - tryhackme
  - walkthrough
  - incident-response
  - digital-forensics
  - csirt
  - log-management
  - Windows-event-logs
description: ''
cover: null
---

**Author:** Mario Martinez Jr. (ku5e / Gary7) | TryHackMe USA Rank #76 | Top 1%

**Difficulty:** Easy

**Topics:** Incident Response, CSIRT, Digital Forensics, Log Management, Windows Event Logs

***

Answers are redacted within the narrative to allow you to complete the tasks on your own, but a full table of answers is available at the end of this walkthrough.

This room covers the Preparation phase of the incident response lifecycle, the foundation that determines whether a team can respond to a breach effectively or scramble in the dark. You take the role of an incident responder building out the people, processes, and technology required to detect and contain adversarial activity before the next room moves into identification and scoping.

<!-- IMAGE #1: Room Banner — TryHackMe Preparation room banner or header. See Image Proposals.md -->

***

## Task 1: Introduction

This task sets the stage for the room, framing incident response as a discipline that combines digital forensics concepts with structured handling procedures. No questions are asked here, but the framing is worth internalising: incident response is not a reactive scramble. It is a practised process built during quiet periods so it runs cleanly under pressure.

***

## Task 2: Incident Response Capability

Before building a response capability, you need a shared definition of what you are responding to. Two terms get conflated constantly and cost teams time during active incidents.

An **event** is any observed occurrence within a system or network. A user authenticating to a file server is an event. Anti-malware blocking a file is an event. Events are noise until proven otherwise.

An **incident** is a violation of security policy with adverse intent. Data exfiltration, ransomware encryption, and denial of service are incidents. Incidents require a response. Events require monitoring.

The IR process used in this room follows six phases:

1. **Preparation:** Lay down procedures before a breach occurs.
2. **Identification:** Detect operational deviations that indicate adversarial activity.
3. **Analysis or Scoping:** Determine the extent of the incident, affected systems, and data at risk.
4. **Containment:** Isolate affected systems and preserve forensic evidence.
5. **Eradication:** Remove adversarial artefacts and restore affected systems.
6. **Recovery and Lessons Learned:** Resume operations and update response capabilities based on what happened.

An incident response plan (IRP) ties these phases together. It is not a general document. It defines roles, responsibilities, communication channels, and metrics for effectiveness. Accompanying the IRP are playbooks, which give the team step-by-step procedures for specific incident types.

**What is an observed occurrence within a system?**
**[REDACTED]**

**What is described as a violation of security policies and practices?**
**[REDACTED]**

**Under which incident response phase do organizations lay down their procedures?**
**[REDACTED]**

**Under which phase will an organization resume business operations fully and update its response capabilities?**
**[REDACTED]**

***

## Task 3: People and Documentation Preparation

### People

The most targeted attack surface in any organization is its people. Social engineering and phishing are the entry points for most breaches because humans are the weakest link in the chain. Preparation addresses this through two mechanisms.

**CSIRT formation:** A cyber security incident response team must include business, technical, legal, and public relations personnel. Each member needs defined permissions under an access control policy. When the CSIRT uses privileged access, system administrators must be notified. Ad hoc privilege escalation during an incident is both a security risk and an evidence integrity problem.

**Training and assessment:** Regular social engineering simulations, spear phishing tests, and current threat awareness training keep the team sharp. End users matter here too. A well-trained employee who reports a suspicious email is a sensor. An untrained one is a liability. Incident handlers specifically need familiarity with forensic imaging tools, audit log analysis, and honeypot operation.

### Documentation

**Policies** define the rules. They must be visible to all employees and stakeholders, including through warning banners that establish no expectation of privacy on organizational systems. Legal review is required to align policies with local regulations.

**Communication plan:** The CSIRT needs a defined point of contact and a clear escalation path that includes when to notify law enforcement, media, or third parties.

**Chain of custody documents** track every piece of evidence from collection through analysis and reporting. If an incident reaches criminal proceedings, the integrity of this chain determines whether the evidence is admissible.

**Response procedures** define the default actions for every role. Clarity of process during a breach directly reduces damage.

**What group handles events involving cybersecurity breaches, comprising individuals with different skills and expertise?**
**[REDACTED]**

**Which documents accompany any evidence collected and track who handles the investigation?**
**[REDACTED]**

***

## Task 4: Technology Preparation

### Asset Inventory

You cannot protect what you do not know exists. The asset inventory classifies all hardware and software within the organization, including servers, endpoints, cloud platforms, and proprietary tools. Each asset is logged with its name, operating system, and IP address. This inventory drives prioritisation: high-value assets like mail servers and VPN servers get more protective and detective coverage than a standard workstation.

### Technical Instrumentation

Once assets are catalogued, telemetry collection follows. This means mapping every network device, cloud platform, system, and application to understand normal behavior. Detection mechanisms built on top of this telemetry include:

- Anti-malware
- Endpoint Detection and Response (EDR)
- Data Loss Prevention (DLP)
- Intrusion Detection and Prevention Systems (IDPS)
- Log collection

Network subnetting supports containment. Logical grouping of devices with defined access policies means that when a segment is compromised, it can be isolated without taking down the entire network.

### Investigation Capabilities

<!-- IMAGE #2: Jump Bag — Photo or illustration of an incident responder's jump bag with labelled contents. See Image Proposals.md -->

The jump bag is the physical readiness kit for incident responders. Its contents vary by team but should include:

- Media drives for evidence storage
- Disk imaging software: FTK Imager, EnCase, The Sleuth Kit
- Network tap for traffic mirroring
- Cables and adapters: USB, SATA, card readers
- PC repair tools: screwdrivers, tweezers
- Copies of IR forms and communication playbooks

### Technical Deep Dive: TheHive Project

TheHive is an open-source incident case management platform used to collect, track, and correlate telemetry and incident details across a CSIRT. It provides a central dashboard for managing cases, alerts, and tasks during an active incident. The platform connects with threat intelligence feeds and supports collaborative investigation across team members.

**What would a kit containing the necessary incident-handling tools be called?**
**[REDACTED]**

***

## Task 5: Visibility

### Why Visibility Matters

A prepared team with no visibility into their network is flying blind. Visibility means aggregating logs from every device, monitoring threat intelligence feeds for emerging TTPs, and ingesting vendor patch advisories. The benefits are concrete:

- Factual evidence of who accessed what resource and when
- Concrete evidence for incident handling and legal proceedings
- Compliance with data protection regulations
- Awareness of emerging adversarial techniques and signatures
- Confirmation that systems are patched

### Log Types

Four log types form the backbone of visibility:

- **Event logs:** Login attempts, application events, network traffic
- **Audit logs:** Sequential activity records capturing who did what and how the system responded. Two classes: Success and Failure.
- **Error logs:** Service failures and system problems
- **Debug logs:** Troubleshooting data generated during testing

### Log Sources

- **Network logs:** Switches, routers, packet capture
- **Host perimeter logs:** Firewalls, proxies, VPN servers
- **System logs:** Operating system events and services
- **Application logs:** Web apps, cloud services, databases, proprietary tools

### Technical Deep Dive: Windows Event Log Setup

<!-- IMAGE #3: Event Viewer Error — Screenshot of the Event Viewer warning banner when the Event Log service is unavailable. See Image Proposals.md -->

The VM in this task demonstrates a common misconfiguration: Windows Event Logging is disabled. The Event Viewer opens with a warning that the Event Log service is unavailable. The registry key controlling this service is at:

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\start`

A value of `4` means the service is disabled. Changing it to `2` sets the service to automatic startup. After a reboot, Event Logging is active.

<!-- IMAGE #4: Registry Editor — Screenshot of the Registry Editor showing the EventLog start key set to 2. See Image Proposals.md -->

### Validating Visibility with Atomic Red Team

With logging active, the room uses Atomic Red Team to simulate a ransomware note creation (MITRE ATT&CK T1486) and verify that Sysmon captures the event.

```powershell
Invoke-AtomicTest T1486 -ShowDetailsBrief
Invoke-AtomicTest T1486-5
```

The resulting log entry appears under:
`Application and Service Logs > Microsoft > Windows > Sysmon > Operational`

<!-- IMAGE #5: Sysmon Log Entry — Screenshot of the Sysmon Operational log showing the Process Create event from the Atomic Red Team test. See Image Proposals.md -->

The File Created event generated by the test carries Event ID **[REDACTED]**.

**What is the Event ID for the File Created rule associated with the test?**
**[REDACTED]**

**Under the Software Restriction Policies, what is the default security level assigned to all policies?**
**[REDACTED]**

**Find the Audit Policy folder under Local Policies. What setting has been assigned to the policy Audit logon events?**
**[REDACTED]**

***

## Task 6: Conclusion

The Preparation phase is the only phase you control entirely before an incident occurs. Every other phase happens under pressure, with incomplete information, while the clock runs. The quality of your response during identification, containment, and eradication is determined by decisions made here: who is on the CSIRT, what policies are in place, what tools are staged, and whether logging is actually running.

The next rooms in this series move into the Identification and Scoping stages of the IR lifecycle.

***

## Answer Table

| Task | Question | Answer |
| --- | --- | --- |
| Task 2 | What is an observed occurrence within a system? | Event |
| Task 2 | What is described as a violation of security policies and practices? | Incident |
| Task 2 | Under which IR phase do organizations lay down their procedures? | Preparation |
| Task 2 | Under which phase will an organization resume operations and update capabilities? | Recovery and Lessons Learned |
| Task 3 | What group handles cybersecurity breach events with varied expertise? | cyber security incident response team |
| Task 3 | Which documents track evidence handling and investigation procedures? | chain of custody documents |
| Task 4 | What is a kit containing the necessary incident-handling tools called? | Jump bag |
| Task 5 | What is the Event ID for the File Created rule? | 11 |
| Task 5 | What is the default security level under Software Restriction Policies? | Unrestricted |
| Task 5 | What setting is assigned to Audit logon events? | Failure |
| Task 6 | No answer needed | N/A |

_Written by Mario Martinez Jr. (ku5e / Gary7) | _[_TryHackMe Profile_](https://tryhackme.com/p/ku5e)_ | _[_ku5e.com/blog_](https://ku5e.com/blog)
