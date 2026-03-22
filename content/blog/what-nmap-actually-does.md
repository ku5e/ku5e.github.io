---
title: What Nmap Actually Does
date: 2026-03-08T14:17:00
draft: false
tags:
  - cybersecurity
  - tools
  - nmap
  - network-security
  - penetration-testing
description: Most people run Nmap. Fewer understand what the packets are doing and what the responses actually mean.
cover:
  image: /images/hero-2026-03-19-what-nmap-actually-does.png
  alt: ''
  caption: ''
  relative: false
---

Nmap sends packets and listens for what comes back. What comes back tells you more about a network than most administrators know about their own infrastructure.

Gordon Lyon released Nmap in 1997 in a Phrack magazine article. It has been in active development since then and has appeared in over a dozen films, including The Matrix Reloaded, Die Hard 4.0, and Bourne Ultimatum, because filmmakers use it when they need a terminal to look like actual hacking. It is one of the most widely used security tools in existence, and most people who run it do not fully understand what it is doing.

## The Default Scan

Running `nmap 192.168.1.1` without any flags performs a SYN scan against the 1,000 most common ports. Nmap sends a TCP SYN packet to each port and waits for a response.

Three things can come back:

**SYN-ACK** means the port is open. The service accepted the connection request. Nmap records the port as open and moves on without completing the three-way handshake. Because the handshake never completes, the connection never fully establishes, which is why the SYN scan is called a half-open scan. Many older logging configurations did not log incomplete connections, which made this scan quieter than a full connect scan. Modern IDS and firewall configurations have largely closed that gap.

**RST** means the port is closed. The host received the packet but nothing is listening on that port. The host itself is up and reachable.

**No response** means the port is filtered. A firewall dropped the packet. Nmap marks these as filtered and may retry depending on configuration.

The distinction between closed and filtered matters. A closed port tells you the host is reachable on that address. A filtered port tells you something is between you and the host that is making decisions about traffic.

## OS Detection

Adding `-O` to the scan tells Nmap to attempt operating system fingerprinting. It does this by analyzing characteristics of the TCP/IP stack in the responses it gets back.

Different operating systems implement TCP/IP with small variations. The initial TTL value, the TCP window size, whether the DF (Don't Fragment) bit is set, how the stack handles specific TCP option combinations: these differ between Windows, Linux, macOS, and embedded systems. Nmap has a database of over 2,600 OS fingerprints. It compares the behavior it observes to that database and returns a match with a confidence percentage.

OS detection requires at least one open and one closed port to work accurately. Without both, Nmap cannot gather enough response variation to make a confident match.

## Service and Version Detection

`-sV` tells Nmap to go beyond identifying open ports and attempt to determine what is running on them. It does this by sending service-specific probes and analyzing the banners or responses that come back.

Port 80 being open does not mean it is running HTTP. It might be running a custom application that happens to listen on port 80. Service detection sends probes and compares the responses to a database of service signatures. A web server will respond differently than a raw TCP socket. Apache responds differently than Nginx, and both respond differently than IIS.

Banner grabbing is part of this process. Many services send an identification string when a connection is made. SSH servers typically send a string that includes the software version. FTP servers often do the same. Those banners are information that attackers use to identify unpatched versions and that defenders should be using to audit what is actually running on their network.

## The Nmap Scripting Engine

The Nmap Scripting Engine (NSE) runs Lua scripts against scan targets to automate specific enumeration and detection tasks. There are over 600 scripts in the default installation, organized into categories including auth, brute, default, discovery, exploit, safe, and vuln.

Running `nmap --script=default` executes the scripts in the default category. Running `nmap --script=vuln` runs vulnerability detection scripts. Running `nmap -sV --script=banner` grabs service banners across all open ports.

Scripts exist to enumerate SMB shares, check for specific CVEs, detect default credentials on services, pull SSL certificate details, brute force authentication, and dozens of other specific tasks. The scripting engine turns Nmap from a port scanner into a lightweight vulnerability assessment tool.

## Why Defenders Need to Run It

Attackers run Nmap against your network before they do anything else. They know what ports are open, what services are running, what operating systems are in use, and where the filtering gaps are.

If you have not run Nmap against your own network, you do not know what they know. Administrators routinely discover ports open that should not be, services running versions that were supposed to be updated, and hosts reachable from segments that should be isolated, all from running a scan they should have run months earlier.

Nmap is not just a reconnaissance tool. It is an audit tool that every network defender should run on a scheduled basis. The output is a current picture of your external and internal attack surface. If the picture surprises you, that is useful information. If it does not surprise you, you have confirmation that your network is what you think it is.

---

*Written by Mario Martinez Jr. (ku5e / Gary7) | [TryHackMe Profile](https://tryhackme.com/p/ku5e) | [ku5e.com/blog](https://ku5e.com/blog)*
