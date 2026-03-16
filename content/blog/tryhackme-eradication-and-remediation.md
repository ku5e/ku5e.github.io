---
title: 'TryHackMe: Eradication and Remediation'
date: 2026-03-15T20:22:00
draft: false
tags:
  - 'tags:'
  - tryhackme
  - walkthrough
  - cybersecurity
  - blog
  - ku5e
  - incident-response
  - eradication
  - remediation
  - jenkins
  - mitre-attack
description: ''
cover: null
---

**Author:** Mario Martinez Jr. (ku5e / Gary7) | TryHackMe USA Top 1%
**Difficulty:** Medium
**Topics:** Incident Response, Eradication, Remediation, MITRE ATT&CK, Jenkins, Cyber Kill Chain

***

Answers are redacted within the narrative to allow you to complete the tasks on your own, but a full table of answers is available at the end of this walkthrough.

This is the fourth room in the Live IR Module, picking up after Preparation, Identification and Scoping, and Threat Intel and Containment. By this point the scope is set and the bad guys are identified. The job now is to remove them cleanly, patch what let them in, and bring systems back online without handing the attacker a warning signal in the process. The room tests your sleuthing ability as much as your IR theory — and the MITRE ATT&CK Framework proves its worth again.

***

![](/images/Screenshot%202026-03-15%20201607.png)

***

## Task 1: Introduction

This room picks up mid-incident. Scoping is complete, systems are contained, and the only work left is removing attacker presence and recovering operations. The room covers the thought process behind eradication, not just the mechanics — which is where most walkthroughs stop.

No answer required.

***

## Task 2: Considerations

Phase 4 is described as both the most important and the easiest to execute incorrectly. That combination should get your attention.

The central risk is premature eradication — moving to cleanup before fully understanding the scope of the compromise. Management pressure, internal urgency, and fear of data loss all push teams toward acting too fast. The problem is that a threat actor who detects cleanup in progress does not sit still. They expedite exfiltration, destroy evidence, and seed additional persistence across systems you have not found yet. You end up worse off than before you moved.

The informal term for what happens next is **[REDACTED]** — you find something, eradicate it, find it again elsewhere, and repeat the cycle indefinitely. Some teams call this progress. It is not. It is a symptom of incomplete scoping.

The answer to premature eradication triggering this cycle is one specific thing: **[REDACTED]**.

Even with proper scoping, initial eradication attempts may fail. The room is direct about this: do not be discouraged. The feedback loop between scoping and eradication exists precisely for this reason. Threat actors who get caught once will return with more sophisticated methods and better detection evasion. The process is cyclic by design.

The first of the two main goals of this phase is **[REDACTED]**.

- **What is it that may cause an attacker to think that you already have a complex and detailed eradication plan in motion?**
    **ANSWER: [REDACTED]**
- **What is an informal term used to describe the cycle wherein you keep discovering and identifying bad, eradicating it, finding it elsewhere, and doing it all over again?**
    **ANSWER: [REDACTED]**
- **Of the two main goals of this phase, what is the first one?**
    **ANSWER: [REDACTED]**

***

## Task 3: Eradication Techniques

Three techniques are covered, each appropriate to a different scenario.

**Automated Eradication** uses AV and EDR tooling to quarantine and remove known malicious artifacts. It works well against commodity threats using well-known tooling. Against purpose-built malware from a sophisticated actor, it falls short — those tools are designed specifically to bypass automated detection. The value of automated eradication is that it frees analysts to focus on the harder problems.

The most direct method is **[REDACTED]** — a complete wipe and rebuild of the compromised system. Clean slate, no attacker traces remaining. The cost is absolute: every application reinstalled, every configuration restored, every piece of data recovered from backup. The other cost is **[REDACTED]** for the system. In environments where even minutes of downtime translates to significant financial loss, this technique may be off the table entirely regardless of how thorough it is.

Targeted cleanup is the third option, used when a full rebuild is not possible and alerting the attacker to the cleanup operation carries too much risk. It requires speed, precision, and a solid intelligence foundation. The room makes the dependency explicit: success in targeted cleanup is heavily reliant on how well **[REDACTED]** has been done. Rushed scoping produces failed targeted cleanup. There is no shortcut around it.

- **What technique is most effective on less sophisticated threats that employ well-known malicious tooling?**
    **ANSWER: [REDACTED]**
- **What technique is the most straightforward way to eradicate attacker traces?**
    **ANSWER: [REDACTED]**
- **What downside does the complete system rebuild technique have? This approach entails what for the system?**
    **ANSWER: [REDACTED]**
- **Success of a targeted system cleanup is heavily reliant on how well the what has been done?**
    **ANSWER: [REDACTED]**

***

## Task 4: Remediation

Eradication removes the attacker. Remediation closes the door they used. Both need to be planned together and executed in sequence — remediation that lags behind eradication leaves the environment re-exploitable before it finishes recovering.

An effective **[REDACTED]** is what makes the effects of eradication last.

Three remediation categories are covered:

**Network Segmentation** reduces the attack surface by restricting communication between systems to only what is operationally necessary. A well-designed segmentation plan also improves the security team's network visibility, making lateral movement detectable earlier in the next incident.

**Identity and Access Management Review** covers two areas. Compromised accounts need their mode of compromise patched, not just their access revoked. User accounts should be reviewed against the **[REDACTED]** — access only to what the role requires, nothing more. Privileged accounts, specifically domain administrators, should be gated behind request-and-approval workflows with time-limited grants. An attacker with domain admin access has free reign across the environment. Getting there is their goal on every engagement.

**Patch Management** addresses the root cause. Eradication removes the tool the attacker used. Patching removes the vulnerability that made the tool useful. If the vulnerable application remains unpatched after cleanup, it is still a low-hanging fruit. Patching should roll out across the entire environment, not just the affected endpoints.

- **What should take place in conjunction with Eradication techniques in order for its effects to last?**
    **ANSWER: [REDACTED]**
- **What remediation step ensures only absolutely necessary communication takes place between computers and subnets?**
    **ANSWER: [REDACTED]**
- **What do you call the principle that posits that a user account should have access to only the absolutely necessary pieces of data, applications, or resources?**
    **ANSWER: [REDACTED]**

***

## Task 5: Recovery

Recovery is where the remediated systems return to production. The goal is normal business operations, and the work does not stop once systems are back online.

**Continuous Testing and Monitoring** validates that the remediation actually held. Penetration tests and attack simulations run against the remediated environment confirm whether the changes close the attack vectors that were exploited. This creates a feedback loop: test, find gaps, remediate, test again. Only after this loop produces clean results should systems be trusted back in production. The room makes a broader point here — continuous testing should extend to the rest of the environment, not just the systems being reintroduced.

Changes done during remediation are geared toward strengthening the **[REDACTED]** of the organization.

**Backups** matter most during a complete rebuild. Documented configurations are useful. Automated setup scripts built from those configurations are better. In cloud environments, maintaining updated baseline images of systems achieves the same outcome.

Recovery happens across near, mid, and long-term action plans. Near-term items are the most critical and get started immediately. It is a continuous process, not a sprint to a finish line.

- **Changes done during the remediation phase are geared towards strengthening the what of the organization?**
    **ANSWER: [REDACTED]**
- **What kind of tests should be employed to check if the remediation tactics actually work?**
    **ANSWER: [REDACTED]**

***

## Task 6: Targeted System Cleanup — Practical Exercise

This is where the theory becomes a real hunt.

The scenario: a Linux server running Jenkins has been compromised through the `swiftspend_admin` account. The account's plaintext password was found in a misconfiguration on another system and is being reused across platforms. Your job is to determine the extent of the compromise on this server and develop an eradication and remediation plan.

SSH into the server using the provided credentials and start digging.

***

![](/images/image-7.png)

The account that gave the threat actors their initial foothold is **[REDACTED]**.

### Jenkins Dashboard

Navigate to the Jenkins service in the browser. The server administrator has not changed the default admin password — that default password is **[REDACTED]**.

***

![](/images/image-8.png)

Inside the Jenkins dashboard you will find a second account. Its email address is **[REDACTED]**.

The project in the dashboard reveals a suspicious command being invoked: **[REDACTED]**. That path is worth examining in full on the filesystem.

***

![](/images/image-9.png)

The project has been run **[REDACTED]** times — it was planted and staged, not yet triggered. That is a meaningful find. The attacker set the mechanism but had not executed it at the time of discovery.

### The Suspicious IP

Read everything. List every directory. Change into every folder and look at every file. THM rooms occasionally surface sidequests through thorough enumeration, and this is exactly the kind of habit that separates analysts who find everything from analysts who find most things.

That process surfaces a suspicious IP address hosted in **[REDACTED]**. Run it through AbuseIPDB to confirm.

![](/images/image-10.png)

### Mapping to MITRE and the Kill Chain

The MITRE ATT&CK Framework earns its place here. The tactic being applied by the threat actor maps to **[REDACTED]** — the attacker has already moved past initial access and execution. They are positioned to move data out.

Against the Lockheed Martin Cyber Kill Chain, the threat actor is already in the **[REDACTED]** phase. The backup script is the mechanism. The staged IP is the destination. They were ready.

![](/images/image-11.png)

- **Which account gave the threat actors a foothold on the server?**
    **ANSWER: [REDACTED]**
- **What is the default password for the admin account of the Jenkins service?**
    **ANSWER: [REDACTED]**
- **What is the email address of the other account within the Jenkins service?**
    **ANSWER: [REDACTED]**
- **What is the command being invoked by the project found in the Jenkins dashboard?**
    **ANSWER: [REDACTED]**
- **How many times has the project been run before?**
    **ANSWER: [REDACTED]**
- **You will find a suspicious IP address. Which country is it hosted in?**
    **ANSWER: [REDACTED]**
- **Based on the MITRE ATT&CK Matrix, which Tactic is being applied by the threat actor here?**
    **ANSWER: [REDACTED]**
- **Based on the Lockheed Martin version of the cyber kill chain, in what phase is the threat actor already in on this server?**
    **ANSWER: [REDACTED]**

***

## Task 7: Conclusion

The room closes the loop on Phase 4 of the IR framework. Eradication removes attacker presence. Remediation closes the vulnerabilities that made the compromise possible. Recovery brings operations back to normal and validates that the fixes held. All three are planned together and executed in sequence.

***

![](/images/image-13.png)

The Jenkins lab makes the abstract concrete. Password reuse on a service account gave the attacker a foothold. Default credentials on the Jenkins admin account gave them elevated access inside the service. A staged backup script pointed at a Russian IP was the exfiltration mechanism waiting to fire. None of that surfaces without methodical enumeration — reading everything, listing everything, opening every file.

MITRE ATT&CK named the tactic. The Kill Chain named the phase. Both frameworks did exactly what they are designed to do.

No answer required.

***

## Final Flag Summary Table

| **Task** | **Question** | **Answer** |
| --- | --- | --- |
| **2** | What may cause an attacker to think you have a detailed eradication plan in motion? | Premature eradication |
| **2** | Informal term for the cycle of discovering, eradicating, finding again, repeating? | Whack-a-mole |
| **2** | First main goal of this phase? | Eradicate the bad guys |
| **3** | Most effective technique against less sophisticated threats using well-known tooling? | Automated Eradication |
| **3** | Most straightforward way to eradicate attacker traces? | Complete System Rebuild |
| **3** | What downside does complete system rebuild have? | Downtime |
| **3** | Targeted system cleanup success relies heavily on what? | Scoping |
| **4** | What must take place with eradication for its effects to last? | Remediation and Recovery strategy |
| **4** | Remediation step ensuring only necessary communication between subnets? | Network Segmentation |
| **4** | Principle that limits user access to only what is necessary? | Principle of least privilege |
| **5** | Remediation strengthens the what of the organization? | Security Posture |
| **5** | What tests verify remediation tactics actually work? | Penetration tests and attack simulations |
| **6** | Which account gave the threat actors a foothold? | swiftspend_admin |
| **6** | Default password for the Jenkins admin account? | f4fe137aeb154299ab1b7349952f6088 |
| **6** | Email address of the other Jenkins account? | infra_admin@swiftspend.finance |
| **6** | Command invoked by the project in Jenkins? | /bin/bash /var/lib/jenkins/backup.sh |
| **6** | How many times had the project been run? | 0 |
| **6** | Country hosting the suspicious IP? | Russian Federation |
| **6** | MITRE ATT&CK Tactic being applied? | Exfiltration |
| **6** | Lockheed Martin Kill Chain phase? | Actions on Objectives |

***

_Written by Mario Martinez Jr. (ku5e / Gary7) | [_TryHackMe Profile_](https://tryhackme.com/p/ku5e) | _[_ku5e.com/blog_](https://ku5e.com/blog)
