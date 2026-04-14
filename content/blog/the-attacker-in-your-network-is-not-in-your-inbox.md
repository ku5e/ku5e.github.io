---
title: The Attacker in Your Network Is Not in Your Inbox
date: 2026-04-13T20:18:00
draft: false
tags:
  - cybersecurity
  - SOC
  - zero-day
  - vulnerability
  - SIEM
  - threat-detection
  - InfoSec
description: 'description: Cisco Talos reported that 40% of all intrusions in Q4 2025 came from exploited vulnerabilities, not phishing. The monitoring infrastructure at most organizations was built for phishing. That design gap is where attackers are living.'
cover:
  image: /images/ZeroDaySOC.png
  alt: Network router in a server room with a SIEM dashboard in the background showing an anomalous traffic alert.
  caption: ''
  relative: false
---

Cisco Talos reported that 40% of all intrusions in Q4 2025 came from exploited vulnerabilities. Phishing dropped to second place. The security awareness training programs running at most organizations have not caught up.

Defenders are losing ground. The monitoring infrastructure was built for an attack pattern that is no longer the primary one.

## Where the Training Points

Phishing awareness training is calibrated for email-borne threats. A user who hovers before clicking, checks the sender domain, and reports a suspicious attachment is an asset. The training addresses a real threat category.

The problem is that it trains for one entry point while the current primary entry point is somewhere else entirely.

The attacker who is inside your network right now did not send an email. They found a zero-day in your edge infrastructure (a router, a firewall, a VPN concentrator), exploited it before a patch existed, and established a persistent foothold in a location your monitoring was not watching.

## The Ghost IP Problem

Here is a specific version of how this works.

A vulnerability in a perimeter router allows an attacker to bind to an IP address that does not appear in the network's topology tables. Call it a ghost IP. Traffic routed through that address is masked at the device level. The SIEM was not configured to watch for it because the network documentation does not know it exists.

The ghost IP does not generate alerts. There is no signature to match because no one has described this address as malicious. Traffic through it looks like nothing, because it does not appear in the flow data being ingested.

What eventually surfaces the attacker is exfiltration volume. When they start pulling data, the anomaly detection fires on outbound traffic spiking to an address that is not in the known-good list. The alert the SOC analyst receives is an anomaly flag on outbound volume to an unknown destination, logged weeks after initial access.

At that point, the attacker has been inside the network for weeks. Possibly months.

## What SOC Analysts Are Actually Watching

SOC analysts are watching email queues and flagged domains because that is what the tooling surfaces. The SIEM ingests the data sources it was configured to ingest. Alert rules fire on the patterns they were written to match. If the attack does not go through email, and it does not go through a known-malicious domain, and it does not match a signature, the analyst does not see it until something volume-based trips a threshold.

The work gets done inside the limits of what the tooling surfaces. That monitoring architecture was built for a threat landscape that has since moved on.

## What the Gap Requires

Edge devices need to be in the monitoring scope with the same discipline as endpoints. That means firmware version tracking on routers and firewalls, outbound traffic baselining per device, and watching for IP addresses that appear in traffic logs but not in the network documentation.

None of these are expensive controls, but they require someone to look at the edge infrastructure as an attack surface and configure monitoring accordingly. That conversation happens less often than it should because the edge is viewed as network infrastructure, and security monitoring is focused on endpoints and email.

The Security+ curriculum covers CIA triad, port numbers, and attack categories. Catching a ghost IP requires understanding how routing tables work, how traffic can be masked at the device level, and how to baseline normal behavior so that the abnormal stands out before exfiltration volume becomes the only signal.

That knowledge is in the labs. TryHackMe has the rooms. The hands-on work is where this skill set gets built.

If you are building toward a SOC analyst role and want a clear map of what to study, build, and prove before you apply, the Cybersecurity Career Roadmap covers that for $47. [Cybersecurity Career Roadmap](https://ku5e.com/services/cybersecurity-career-roadmap/)

_Written by Mario Martinez Jr. (ku5e / Gary7) |_ [_TryHackMe Profile_](https://tryhackme.com/p/ku5e) _|_ [_ku5e.com/blog_](https://ku5e.com/blog)
