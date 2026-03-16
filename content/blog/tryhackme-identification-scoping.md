---
title: 'TryHackMe: Identification & Scoping'
date: 2026-03-15T20:29:00
draft: false
tags:
  - tryhackme
  - walkthrough
  - cybersecurity
  - incident-response
  - phishing
  - blog
  - ku5e
description: ''
cover: null
---

**Author:** Mario Martinez Jr. (ku5e / Gary7) | TryHackMe USA Top 1%

**Difficulty:** Easy

**Topics:** Incident Response, Security Alerts, Asset Inventory, IoC Management

---

Answers are redacted within the narrative to allow you to complete the tasks on your own, but a full table of answers is available at the end of this walkthrough.

This room positions you as an incident responder at SwiftSpend Financial (SSF), a fictional organization facing a potential security compromise. You work through a series of support tickets in Outlook, cross-reference an Asset Inventory, and populate a Spreadsheet of Doom (SoD) with indicators of compromise. The room frames itself around identification, scoping, and the feedback loop between the two. In practice, it functions as an email reading exercise where you extract IoCs from ticket exchanges and map them to organizational assets.

<!-- IMAGE #7: Hero/Banner - SwiftSpend Financial incident response scenario with Outlook tickets and tracking spreadsheets - see Image Proposals.md -->

The scenario is clean and linear. No log analysis. No EDR telemetry. No SIEM correlation. You open Outlook, sort oldest to newest, and work through the tickets.

---

## Task 2: Identification: Unearthing the Existence of a Security Incident

The VM loads with Outlook already running. Ignore the activation prompts. The room emphasizes the "People, Process, Technology" triad as the foundation of incident identification. Staff report anomalies through proper channels, security tools generate alerts, and procedures guide the response. The concept is sound but delivered as exposition rather than practice.

<!-- IMAGE #1: Outlook Inbox - Outlook inbox displaying SwiftSpend Financial support tickets in chronological order - see Image Proposals.md -->

Your first ticket is **Ticket#2023012398704232**, reporting a "Weird Error in Outlook." The user forwarded a suspicious email, and your colleague John chimes in with his assessment.

<!-- IMAGE #2: Ticket Email Thread - Email thread for Ticket#2023012398704232 showing phishing report and colleague analysis - see Image Proposals.md -->

- **What is the Subject of Ticket#2023012398704232?**

    **ANSWER: [REDACTED]**

John suspects the issue stems from improperly configured email authentication. SPF, DKIM, and DMARC records prevent domain spoofing. If these are misconfigured or absent, attackers can send emails that appear to originate from legitimate internal addresses.

- **According to your colleague John, the issue outlined on Ticket#2023012398704232 could be related to what?**

    **ANSWER: [REDACTED]**

John requests web proxy logs for the affected workstation to determine if the user clicked any malicious links.

- **Your colleague requested what kind of data pertaining to the machine WKSTN-02?**

    **ANSWER: [REDACTED]**

The room introduces the concept of security alerts and event notifications here, but the actual alert is a forwarded email. No SIEM dashboard. No EDR detection. Just a user forwarding something suspicious through the ticketing system.

---

## Task 3: Scoping: Understanding the Extent of a Security Incident

Scoping determines which systems are affected, what data is at risk, and how far the compromise has spread. The room provides two tools for this: the Asset Inventory and the Spreadsheet of Doom.

The **Asset Inventory** is a simple table listing organizational assets, their IP addresses, operating systems, and owners. SwiftSpend Financial operates a small environment: one domain controller, one mail server, one web server, one proxy, and a handful of workstations and laptops.

<!-- IMAGE #3: Asset Inventory - SwiftSpend Financial Asset Inventory spreadsheet with device details and ownership - see Image Proposals.md -->

The **Spreadsheet of Doom (SoD)** tracks indicators of compromise. Each row contains an indicator type (IP, domain, email address, file hash), the indicator itself, the threat type, and the source. This is a lightweight threat intelligence repository. In a real environment, this would be a MISP instance or a TIP platform, but the concept translates.

<!-- IMAGE #4: Spreadsheet of Doom - Spreadsheet of Doom showing indicators of compromise with threat types and sources - see Image Proposals.md -->

**Ticket#2023012398704231** reports that a machine needs endpoint protection definitions updated. Cross-referencing the Asset Inventory with the ticket details reveals the asset owner.

- **Based on Ticket#2023012398704231 and Asset Inventory shown in this task, who owns the computer that needs Endpoint Protection definitions updated?**

    **ANSWER: [REDACTED]**

Back to **Ticket#2023012398704232**. The phishing email prompted the user to submit credentials. The SoD already contains an entry for the phishing domain.

- **Based on the email exchanges and SoD shown in this task, what was the phishing domain where the compromised credentials in Ticket#2023012398704232 were submitted?**

    **ANSWER: [REDACTED]**

**Ticket#2023012398704233** introduces a second phishing domain. This one is not yet in the SoD and needs to be added.

- **Based on Ticket#2023012398704233, what phishing domain should be added to the SoD?**

    **ANSWER: [REDACTED]**

The task structure is straightforward. Read the ticket. Check the Asset Inventory. Update the SoD. Repeat.

---

## Task 4: Identification and Scoping Feedback Loop: An Intelligence-Driven Incident Response Process

The feedback loop concept is familiar to anyone with an engineering background. You identify an issue, document it, collect evidence, analyze artifacts, discover new pivot points, and loop back to refine your understanding. Rinse and repeat until you have full situational awareness.

<!-- IMAGE #6: Feedback Loop Diagram - Incident response feedback loop diagram illustrating iterative scoping process - see Image Proposals.md -->

The room frames this as: **Event Notification → Documentation → Evidence Collection → Artefact Identification → Pivot Point Discovery → Documentation (loop)**.

The loop plays out across the tickets. **Ticket#2023012398704232** starts with a user reporting a phishing email. John's analysis reveals that the domain **emkei.cz** was used for email spoofing and should be added to the SoD.

- **Concerning Ticket#2023012398704232 and according to your colleague John, what domain should be added to the SoD since it was used for email spoofing?**

    **ANSWER: [REDACTED]**

Digging through the email exchanges, you discover that another user received the same phishing email but never reported it. This is a common scenario. Users either ignore suspicious emails or assume someone else will handle it.

- **Concerning the available artefacts gathered for analysis of Ticket#2023012398704232, who is the other user that received a similar phishing email but did not open a ticket nor report the issue?**

    **ANSWER: [REDACTED]**

The phishing email originated from an external Gmail account. This is a pivot point. The attacker's email address becomes a new IoC to track.

- **Concerning Ticket#2023012398704232, what additional IoC could be added to the SoD and be used as a pivot point for discovery?**

    **ANSWER: [REDACTED]**

The email exchanges also contain the compromised user's credentials in cleartext. This is a confidentiality breach. The password was submitted to the phishing domain and is now exposed.

<!-- IMAGE #5: Phishing Email - Phishing email exchange revealing compromised credentials in cleartext - see Image Proposals.md -->

- **Based on the email exchanges and attachments in those exchanges, what is the password of the compromised user?**

    **ANSWER: [REDACTED]**

This task would have been stronger if framed around the CIA triad. The room could have explicitly called out the confidentiality breach (password in cleartext), highlighted the integrity risk (email spoofing via emkei.cz), and discussed availability implications if the attacker used the credentials to lock accounts or disrupt services. The DAD triad (Disclosure, Alteration, Destruction) would have driven the point home. Instead, the room delivers the information in a more abstract "feedback loop" structure.

---

## Task 5: Conclusion

The room wraps with a summary of the Identification and Scoping phase and directs you to the next module: Intel Creation and Containment.

What this room delivers: a structured introduction to asset tracking, IoC management, and the concept of iterative scoping through a feedback loop. The scenario is simple. The tickets flow logically. The tools are lightweight but representative of real-world equivalents.

What this room does not deliver: hands-on log analysis, SIEM queries, EDR artifact extraction, or network traffic inspection. The scoping process is reading emails and filling in spreadsheets. Real incident response would involve correlating proxy logs with the phishing domain, checking authentication logs for failed login attempts using the compromised credentials, and hunting for lateral movement from WKSTN-02.

The room positions itself as an introduction. It succeeds at that. If you are looking for a technical deep dive into scoping a phishing compromise, this is not it. If you want a clean walkthrough of how Asset Inventories and IoC tracking fit into the IR workflow, this delivers.

---

## Final Flag Summary Table

| **Task** | **Category/Question** | **Flag/Answer** |
|---|---|---|
| **2** | Subject of Ticket#2023012398704232 | **Weird Error in Outlook** |
| **2** | Issue could be related to what (John's assessment) | **SPF, DKIM & DMARC records** |
| **2** | Data requested for WKSTN-02 | **Web Proxy logs** |
| **3** | Owner of computer needing Endpoint Protection update (Ticket#2023012398704231) | **Derick Marshall** |
| **3** | Phishing domain where compromised credentials were submitted (Ticket#2023012398704232) | **b24b-158-62-19-6.ngrok-free.app** |
| **3** | Phishing domain to add to SoD (Ticket#2023012398704233) | **kennaroads.buzz** |
| **4** | Domain used for email spoofing (Ticket#2023012398704232) | **emkei.cz** |
| **4** | User who received phishing email but did not report it | **alexander.swift@swiftspend.finance** |
| **4** | Additional IoC to add to SoD (pivot point) | **sales.tal0nix@gmail.com** |
| **4** | Password of compromised user | **Passw0rd!** |

---

*Written by Mario Martinez Jr. (ku5e / Gary7) | [TryHackMe Profile](https://tryhackme.com/p/ku5e) | [ku5e.com/blog](https://ku5e.com/blog)*
