---
title: 'TryHackMe: Threat Intel & Containment'
date: 2026-03-15T20:59:00
draft: false
tags:
  - tryhackme
  - walkthrough
  - cybersecurity
  - blog
  - ku5e
  - threat-intelligence
  - incident-response
  - containment
  - wireshark
description: Walkthrough for the TryHackMe Threat Intel & Containment room covering threat intelligence creation, containment strategies, and basic Wireshark packet analysis.
cover:
  image: /images/Threat_Intel.png
  alt: ''
  caption: ''
  relative: false
---

**Author:** Mario Martinez Jr. (ku5e / Gary7) | TryHackMe USA Top 1%
**Difficulty:** Easy
**Topics:** Threat Intelligence, Containment Strategies, Incident Response, Wireshark

---

Answers are redacted within the narrative to allow you to complete the tasks on your own, but a full table of answers is available at the end of this walkthrough.

This room is a lecture-heavy introduction to threat intelligence creation and containment strategies within the incident response cycle. Most tasks pair reading with a single comprehension question. The practical at the end drops a packet capture on the desktop and asks you to pull three specific values from the traffic. Wireshark filtering gets you there fast.

<!-- IMAGE #4: Hero banner - Hero/banner image for Threat Intel & Containment walkthrough - see Image Proposals.md -->

---

## Task 2: Pre-Containment

Before containment can begin, the response team needs to understand what happened and who is responsible. This task covers evidence collection from infrastructure like IDS and SIEM systems to build Indicators of Compromise.

The example walks through a workstation that downloaded an executable, likely from a user clicking a malicious PDF. ELK with Packetbeat surfaces the domain visit. From there, the team pulls the file hash using `Get-FileHash` on Windows or `sha256sum` on Linux.

```powershell
PS C:\Users\MichaelAscot\Downloads> Get-FileHash dropper.exe
```

That hash becomes the anchor for detection. Any host carrying that file is presumed compromised. The SIEM gets an alert. The hash also links this activity to prior campaigns or known threat actors.

- **What does the acronym IDS mean?**
    **ANSWER: [REDACTED]**

---

## Task 3: Containment Strategies

Two strategies are presented here, and the distinction matters.

**Isolation** cuts the infected device off completely through network segmentation or physical disconnection. It stops the bleeding fast. The downside: a watching adversary will notice immediately and may accelerate their action on objectives before losing access to the remaining systems they control.

**Controlled isolation** keeps the infected system accessible while the response team monitors everything the adversary does. The team can intervene before destructive actions — data wipes, exfiltration — and use a cover story like scheduled maintenance to explain any sudden access interruption. The intelligence gathered this way is significant. The risk is real too. It requires constant monitoring and enough staff to sustain it around the clock.

Choosing between them depends on how much you already know about the adversary, whether they are likely to escalate damage if they detect discovery, and whether the response team has the capacity for continuous surveillance.

- **What is the name of the containment strategy used when responders closely monitor the adversary?**
    **ANSWER: [REDACTED]**

- **What containment strategy is considered to be the most aggressive?**
    **ANSWER: [REDACTED]**

---

## Task 4: Creating Threat Intelligence

Threat intelligence takes several forms: IP addresses, file hashes, domains, filenames, and TTPs. The room breaks TTPs into three parts.

Tactics are high-level objectives. Is the adversary after data, money, or disruption? Techniques are the specific tools and methods used to get there: phishing, credential theft, exploitation of a specific service. Procedures are the full attack chain from initial access through action on objectives.

OpenCTI is presented as a collaborative platform for sharing intelligence across teams. Subscribing to public feeds like AlienVault, DigitalSide, and threatfeeds.io means your SIEM can have detection rules in place before an incident occurs.

The task also covers internal reporting. An employee who forwards a suspicious attachment to a coworker's personal email has created additional risk. Organizations need a defined reporting channel that does not require forwarding malicious content to surface it to the right person.

- **What is the term for a set of characters that can be used to give attribution to a file?**
    **ANSWER: [REDACTED]**

---

## Task 5: Threat Intelligence Creation Feedback Loop

Organizations that rush to eradicate a threat before fully scoping it will miss compromised systems. Restoring one infected machine without understanding the full breach leaves the adversary active on other systems. The room uses the whack-a-mole analogy for this pattern, and it fits.

Better intelligence leads to better scope. Better scope leads to a more targeted containment strategy. The loop continues after the incident closes. Post-incident review should surface what the SIEM missed, what noise obscured real signals, and what detection rules need updating before the next one.

- **What is the name of the classic arcade game referenced in this task?**
    **ANSWER: [REDACTED]**

---

## Task 6: Practical

A packet capture file sits on the desktop. Three values need to come out of it: the adversary's IP address, the filename pulled from the infrastructure, and the SHA-256 hash of the downloaded executable.

<!-- IMAGE #1: Wireshark - Wireshark showing packet capture with adversary traffic visible - see Image Proposals.md -->

Open the capture in Wireshark and filter by protocol or follow TCP streams to identify the outbound connection. The adversary IP appears in the traffic. The filename is visible in the HTTP request. For the hash, the file is already on the desktop. Run `sha256sum` or `Get-FileHash` against it directly.

Filter early. Narrowing to HTTP traffic or filtering by the suspicious IP cuts the noise fast. Do not scroll through raw packets hunting for values manually.

<!-- IMAGE #2: Hash output - Terminal showing SHA-256 hash output of dropper.exe - see Image Proposals.md -->

- **What is the IP address of the adversary?**
    **ANSWER: [REDACTED]**

- **What is the name of the file that gets downloaded from the adversary's infrastructure?**
    **ANSWER: [REDACTED]**

- **What is the SHA-256 hash value of the executable on the Desktop?**
    **ANSWER: [REDACTED]**

---

## Final Flag Summary Table

| **Task** | **Category/Question** | **Flag/Answer** |
|---|---|---|
| **2** | What does the acronym IDS mean? | **Intrusion Detection System** |
| **3** | What is the name of the containment strategy used when responders closely monitor the adversary? | **Controlled Isolation** |
| **3** | What containment strategy is considered to be the most aggressive? | **Entire Isolation** |
| **4** | What is the term for a set of characters that can be used to give attribution to a file? | **Hash** |
| **5** | What is the name of the classic arcade game referenced in this task? | **Whack-a-mole** |
| **6** | What is the IP address of the adversary? | **3.250.38.141** |
| **6** | What is the name of the file that gets downloaded? | **dropper.exe** |
| **6** | What is the SHA-256 hash value of the executable? | **463F1B1E11D4CA4C7A0C9AAC540513FF7E681D9E5144BDA2AF24B86E438D3F4F** |

---

*Written by Mario Martinez Jr. (ku5e / Gary7) | [TryHackMe Profile](https://tryhackme.com/p/ku5e) | [ku5e.com/blog](https://ku5e.com/blog)*
