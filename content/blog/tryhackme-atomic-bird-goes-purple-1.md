---
title: 'TryHackMe: Atomic Bird Goes Purple #1'
date: 2026-03-08T19:57:00
draft: false
tags:
  - tryhackme
  - walkthrough
  - purple-team
  - threat-emulation
  - atomic-red-team
  - sysmon
  - aurora-edr
description: A hands-on Purple Team exercise using custom Atomic Red Team tests to emulate system discovery, credential capture, file manipulation, and exfiltration staging. Covers T1082, T1056.002, T1091, and T1115 with full artifact investigation in Windows Event Logs, Sysmon, and Aurora EDR.
cover:
  image: /images/frostandfire.png
  alt: ''
  caption: ''
  relative: false
---

# TryHackMe: Atomic Bird Goes Purple #1

**Author:** Mario Martinez Jr. (ku5e / Gary7) | TryHackMe USA Rank #76 | Top 1%

**Difficulty:** Medium

**Topics:** Purple Teaming, Threat Emulation, Atomic Red Team, Windows Event Logs, Sysmon, Aurora EDR

***

Answers are redacted within the narrative to allow you to complete the tasks on your own, but a full table of answers is available at the end of this walkthrough.

This room puts you inside a Purple Team exercise built around the Atomic Red Team project. You emulate real adversary tactics across system discovery, credential capture, file manipulation, clipboard abuse, and system file hijacking, then investigate the artifacts each technique leaves behind. The goal is not just to run attacks but to understand what defenders see when those attacks run.

<!--image 5-->

***

## Task 1: Introduction

Start the attached VM and proceed. The machine loads in split-screen view. Nothing to submit here.

The prerequisites for this room are not decorative. If Windows Event Logs, Sysmon, Sigma, Aurora EDR, and the Hacking with PowerShell rooms are not in your history, the artifact investigation sections will be difficult to follow. The room assumes you can read event logs and correlate process activity without a guide.

***

## Task 2: Getting Started With Custom Exercises and Investigation Process

This task frames the methodology. The room uses custom Atomic Red Team tests mapped to real MITRE ATT&CK techniques. The mapping across the three active tasks is:

| Task | Tactics | Techniques |
| --- | --- | --- |
| 4 | Execution, Discovery, Collection | T1056.002, T1059, T1082 |
| 5 | Lateral Movement | T1091 |
| 6 | Collection | T1115 |

The investigation mindset matters here. You are running each test, then immediately examining logs, directories, and registry entries before moving on. The room is designed to be worked in sequence. Each cleanup command restores the system before the next test runs.

The note about obfuscated atomics is worth taking seriously. Some test payloads are not provided in cleartext. The task descriptions and event logs give you enough to reconstruct what happened. This mirrors real-world forensics, where you often work backward from artifacts rather than forward from source code.

***

## Task 3: Toolset and Hints

### Technical Deep Dive: THM-Utils and Atomic Red Team

Two tools do the work in this room.

**THM-Utils** is a custom PowerShell module loaded in the profile. It wraps Windows event log queries into summarized output grouped by count, Event ID, task category, and event provider. The most useful commands for this room:

```powershell
THM-LogClear-All                      # Clear all logs before each test
THM-LogStats-All                      # Summary of all monitored logs
THM-LogStats-Sysmon                   # Sysmon-specific log summary
THM-LogStats-Flag                     # Retrieves the flag for the current question
```

The hint to clear logs before each test is not optional if you want clean results. Running multiple tests without clearing produces overlapping log entries that make attribution harder.

**Atomic Red Team** is an open-source library of adversary technique simulations mapped to MITRE ATT&CK. Each technique has one or more test cases. The syntax is consistent:

```powershell
Invoke-AtomicTest T0000-1             # Execute test case 1 of T0000
Invoke-AtomicTest T0000-1 -Cleanup    # Remove artifacts and restore files
```

The `-Cleanup` flag matters. Every task in this room has a corresponding cleanup command. Run it before moving to the next task.

Running `THM-LogStats-Flag` in the PowerShell session retrieves the flag for the first question.

The cleanup command for a hypothetical test T0123-4 follows the same pattern: **[REDACTED]**

<!--image 1-->

***

## Task 4: Execute, Investigate, Detect

### Technical Deep Dive: T1082 and T1056.002

**T1082 — System Information Discovery** covers adversary actions to gather OS, hardware, and network details. The WinAPI calls and PowerShell commands used for enumeration leave traces in PowerShell Operational logs and Sysmon process creation events (Event ID 1). The OS Build number pulled by T0004-1 is the kind of data an attacker collects early in an intrusion to determine the patch level and select appropriate exploits.

**T1056.002 — GUI Input Capture** involves presenting a fake credential prompt that looks like a legitimate Windows dialog. The user enters credentials believing the prompt is real. The technique generates artifacts in both PowerShell logs and Sysmon, particularly around the process that spawns the dialog and captures input.

**T0004-1: Initial Enumeration Emulation**

Run the test:

```powershell
Invoke-AtomicTest T0004-1
```

The test creates a document on the Desktop. Open it and read the OS Build information. The value is **[REDACTED]**.

<!--image 2-->

**T0004-2: Credential Prompt Emulation**

Clear logs first, then run:

```powershell
THM-LogClear-All
Invoke-AtomicTest T0004-2
```

A GUI prompt appears. Interact with it. The flag is retrieved from `THM-LogStats-Flag` after execution: **[REDACTED]**

**T0004-3: Failed Command Emulation**

Clear logs, then run:

```powershell
THM-LogClear-All
Invoke-AtomicTest T0004-3
```

Examine the logs after execution. PowerShell logs will contain the failed command. The command attempted is a Linux bash shebang: **[REDACTED]**

This is a common red team technique for testing whether a Windows endpoint will log and alert on commands that are invalid for the platform. The presence of a Linux-style command on a Windows host is a high-confidence detection signal.

<!--image 3-->

Cleanup:

```powershell
Invoke-AtomicTest T0004-1 -Cleanup
Invoke-AtomicTest T0004-2 -Cleanup
Invoke-AtomicTest T0004-3 -Cleanup
```

***

## Task 5: Universal Suspicious Share

### Technical Deep Dive: T1091 and File Integrity Monitoring

**T1091 — Replication Through Removable Media** covers spreading malicious content through shared drives and removable media. In this exercise the technique is adapted to shared folder manipulation. The key artifact is a change in file hash. A file with a known good SHA256 value that returns a different hash after an operation has been modified.

SHA256 is a one-way cryptographic hash function. The same input always produces the same output. Any change to the file, including a single byte, produces a completely different hash. This is the basis for file integrity monitoring in SIEMs and EDR tools.

**Pre-execution baseline**

Navigate to the shared folder on the disk. Calculate the SHA256 hash of the `.txt` document before running the test:

```powershell
Get-FileHash -Algorithm SHA256 .\filename.txt
```

The pre-execution hash is **[REDACTED]**.

**T0005-1: Universal Suspicious Share**

Clear logs, then run:

```powershell
THM-LogClear-All
Invoke-AtomicTest T0005-1
```

Recalculate the hash of the same file:

```powershell
Get-FileHash -Algorithm SHA256 .\filename.txt
```

The post-execution hash is **[REDACTED]**. The change in hash confirms the file was modified by the test. In a real environment, this delta would trigger a file integrity alert.

<!--image 4-->

Cleanup:

```powershell
Invoke-AtomicTest T0005-1 -Cleanup
```

***

## Task 6: Dump and Go

### Technical Deep Dive: T1115, Command History Exfiltration, and System File Hijacking

**T1115 — Clipboard Data** is broader in practice than the name suggests. Adversaries collect command-line history, clipboard contents, and system files for exfiltration staging, MITM positioning, and security product interference. The two tests here cover history dumping and system file modification.

**T0006-1: History Dump**

Clear logs, then run:

```powershell
THM-LogClear-All
Invoke-AtomicTest T0006-1
```

The test creates a malicious history dump file. Locate it on the filesystem. The flag embedded in the file is **[REDACTED]**.

Command-line history files are valuable to attackers because they contain previously executed commands, credentials passed as arguments, internal hostnames, and file paths. A history dump that exits the environment gives an attacker a map of what the operator has been doing on the system.

**T0006-2: SystemFile Modification for Exfiltration**

Clear logs, then run:

```powershell
THM-LogClear-All
Invoke-AtomicTest T0006-2
```

The test modifies a system file. Locate the modification. The flag in the modified file is **[REDACTED]**.

System file hijacking for exfiltration works by replacing or appending to files that security tools, monitoring agents, or network equipment read during normal operation. If a security product reads from a hosts file or a trusted certificate store that has been modified, the attacker can redirect traffic or inject content into the security product's workflow.

<!--image 5-->

Cleanup:

```powershell
Invoke-AtomicTest T0006-1 -Cleanup
Invoke-AtomicTest T0006-2 -Cleanup
```

***

## Task 7: Conclusion

The room walks a complete Purple Team cycle: emulate, observe, investigate, clean up, repeat. The techniques covered span the attack chain from initial discovery through collection and exfiltration staging. The custom atomics give you a controlled environment to see exactly what each technique produces in logs and on disk.

Atomic Bird Goes Purple #2 continues with more scenarios in the same format.

***

## Answer Table

| Task | Question | Answer |
| --- | --- | --- |
| 3 | Flag from THM-LogStats-Flag | THM{Emulation_is_fun_but_needs_focus_and_exploration} |
| 3 | Cleanup command for T0123-4 | Invoke-AtomicTest T0123-4 -Cleanup |
| 4 | OS Build info from T0004-1 | 10.0.17763 N/A Build 17763 |
| 4 | Flag from T0004-2 | THM{THM_Emulation_Room} |
| 4 | Failed command from T0004-3 | <!bin/bash> |
| 5 | SHA256 of .txt before T0005-1 | 3CA9FB42ACF0A347BDFDC78E0435331BC458194E4BC7FBFFB255BC4CF02CDC1A |
| 5 | SHA256 of .txt after T0005-1 | 626DBB861DCFF600DABEFCE7BF93F2C72C0F6462CC5729B963FC8242D7D43990 |
| 6 | Flag from history dump (T0006-1) | THM{THM_analytics_to_exfiltration_with_NexGenHunt} |
| 6 | Flag from system file modification (T0006-2) | THM{NextGenHunt.thm.jhn} |

***

_Walkthrough by Mario Martinez Jr. (ku5e / Gary7) | [_TryHackMe Profile_](https://tryhackme.com/p/ku5e) | _[_blog.ku5e.com_](https://blog.ku5e.com)
