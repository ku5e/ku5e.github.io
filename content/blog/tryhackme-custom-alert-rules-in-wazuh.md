---
title: "TryHackMe: Custom Alert Rules in Wazuh"
date: 2026-03-03
draft: false
tags: ["tryhackme", "walkthrough", "wazuh", "siem", "xdr", "detection"]
description: "Walkthrough covering Wazuh decoder analysis, rule logic, and custom local_rules.xml configuration."
---

**Author:** Mario Martinez Jr. (ku5e / Gary7) | TryHackMe USA Rank #76 | Top 1%

**Difficulty:** Medium

**Topics:** XDR/SIEM, Rule Syntax, Regex, Threat Detection

---

Answers are redacted within the narrative to allow you to complete the tasks on your own, but a full table of answers is available at the end of this walkthrough.

In this lab, we step into the role of a SOC analyst responsible for fine-tuning a Wazuh deployment. The default rule set captures many common threats, but specialized environments require custom detection logic to identify sophisticated adversary behavior. We focus on modifying the local rules configuration to trigger alerts based on specific log patterns and nested logic.

---

## Task 2: Decoders and Sysmon Analysis

Wazuh uses decoders to transform raw logs into structured data fields. When a Windows machine with Sysmon installed sends an event, the decoder identifies specific fields such as the process image, parent process, and command line.

In the provided Sysmon log, we are examining a Process Creation event. We look at the `sysmon.commandLine` field to see exactly what was executed on the host. This field provides the full context of the command, including the binary path and the specific script being called with its associated flags.

- **Looking at the Sysmon Log, what will the value of sysmon.commandLine be?**

  **ANSWER: [REDACTED]**

Next, we look at how Wazuh extracts data using Regex. If we encounter a log entry that contains a user identity, we can use a non-whitespace selector to grab just the name. The pattern `User: \S*` tells the engine to find the literal string "User: " and capture every character until it hits a space.

- **What would the extracted value be if the regex is set to "User: \S*"?**

  **ANSWER: [REDACTED]**

---

## Task 3: Understanding Ruleset Logic

Rules use the fields extracted by decoders to determine if an alert should fire. We often pivot off specific MITRE ATT&CK techniques or rule IDs to broaden detection coverage. Rule ID 184666 is part of the default Sysmon ruleset and is tied to a specific adversary technique (T1055).

- **From the Ruleset Test results, what is the MITRE ID of rule id 184666?**

  **ANSWER: [REDACTED]**

We also use the Wazuh Dashboard's "Ruleset Test" feature to simulate how the manager would react to a log if certain fields were changed. By manually altering the `sysmon.image` value to a common system process like `taskhost.exe`, we can observe which rule hierarchy takes over and how the manager assigns a Rule ID to that specific execution.

---

## Task 5: Implementing Custom Rules

When default rules are too broad, we modify `local_rules.xml` to add environment-specific logic. A common requirement is to monitor activity within temporary or sensitive directories. We use the `<regex>` tag within our rule blocks to look for keywords or paths within the log data to escalate the alert level. In the provided example, we are looking specifically at the current working directory.

- **What is the regex field name used in the local_rules.xml?**

  **ANSWER: [REDACTED]**

---

## Final Flag Summary Table

| Task | Category/Question | Flag/Answer |
|---|---|---|
| **2** | sysmon.commandLine value | **"C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" "-file" "C:\Users\Alberto\Desktop\test.ps1"** |
| **2** | Regex extracted value | **WIN-P57C9KN929H\Alberto** |
| **2** | sysmon.parentImage value | **C:\Windows\Explorer.EXE** |
| **3** | MITRE ID for rule 184666 | **T1055** |
| **3** | Rule 12 description | **High importance event** |
| **3** | Rule ID for taskhost.exe | **184736** |
| **4** | Parent Rule ID of 184717 | **184716** |
| **5** | Regex field name in local_rules | **audit.cwd** |
| **5** | Current working directory (cwd) | **/var/log/audit** |
| **6** | Rule ID for test.php | **100003** |
| **6** | malware-checker.sh level | **12** |

---

*Walkthrough by Mario Martinez Jr. (ku5e / Gary7) | [TryHackMe Profile](https://tryhackme.com/p/ku5e) | [ku5e.com](https://ku5e.com)*
