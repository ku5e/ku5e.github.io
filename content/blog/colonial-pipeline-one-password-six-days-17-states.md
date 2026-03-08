---
title: 'Colonial Pipeline: One Password, Six Days, 17 States'
date: 2026-03-08T14:12:00
draft: false
tags:
  - cybersecurity
  - breach-analysis
  - ransomware
  - critical-infrastructure
description: DarkSide did not use a sophisticated zero-day to shut down 45 percent of the East Coast fuel supply. They used a leaked password and an account with no MFA.
cover:
  image: /images/hero-2026-03-17-colonial-pipeline.png
  alt: ''
  caption: ''
  relative: false
---

DarkSide did not use a sophisticated zero-day to shut down 45 percent of the East Coast fuel supply. They used a password found in a leaked credential database and an account that had no multi-factor authentication.

On May 7, 2021, Colonial Pipeline shut down 5,550 miles of pipeline after discovering a ransomware infection. That pipeline moves 100 million gallons of fuel per day and supplies gasoline, diesel, and jet fuel from Texas to New York. It stayed offline for six days.

## How They Got In

The FBI investigation traced the initial access to a single VPN account. The account was no longer in active use, but it had not been disabled. The password for that account appeared in a batch of leaked credentials found on the dark web, meaning it had been compromised in a previous breach at some other organization.

The account had no MFA configured. One valid credential was enough to authenticate.

Colonial did not detect the intrusion at the point of entry. DarkSide operators moved through the network, identified high-value systems, and exfiltrated approximately 100 gigabytes of data before deploying the ransomware. Colonial discovered the attack when they found the ransom note and saw encrypted files.

The decision to shut down the pipeline was Colonial's, not DarkSide's. The ransomware hit the IT network, not the operational technology network that directly controls the pipeline. Colonial chose to shut down operations proactively because they could not confirm the OT systems were clean and because the IT systems they needed to manage billing and operations were compromised.

## What It Cost

Colonial paid DarkSide 75 Bitcoin on May 8, 2021, one day after the attack. At the time of payment, that was approximately $4.4 million.

The U.S. Department of Justice recovered 63.7 Bitcoin of that payment in June 2021, worth approximately $2.3 million at the time of recovery. The FBI had obtained the private key for the wallet DarkSide used to receive the ransom, which allowed the recovery. DarkSide had already distributed a portion of the payment to affiliates before law enforcement seized it.

Gas prices hit a national average above $3.00 per gallon for the first time since 2014. Gas stations in 17 states reported shortages. Lines stretched around blocks in parts of the Southeast. The panic buying made the shortage worse than the pipeline outage alone would have caused.

## What Was Missing

The entry point was not a new class of vulnerability. It was a credential from a previous breach, a VPN account that was not deprovisioned when the employee stopped using it, and the absence of MFA on a remote access account.

Three specific failures:

**No MFA on the VPN.** A leaked password should not be sufficient to authenticate to a remote access account on critical infrastructure. MFA requirement on VPN access is a baseline control, not an advanced one.

**No deprovisioning process.** The account that DarkSide used was no longer active. If it was not in use, it should not have existed. Accounts that are not deprovisioned when they are no longer needed are a standing attack surface. Periodic access reviews would have caught this.

**No detection at the entry point.** VPN logins from unusual geographies or at unusual times without a prior authentication pattern are detectable. The intrusion went undetected until the ransomware deployed, which means there was time between initial access and execution that detection controls could have used.

## What It Changed

The Biden administration issued Executive Order 14028 on May 12, 2021, five days after the attack. It required federal agencies to implement MFA and encryption within 180 days, established baseline cybersecurity standards for software sold to the federal government, and created a Cyber Safety Review Board modeled on the National Transportation Safety Board.

CISA published the Known Exploited Vulnerabilities catalog in November 2021, partly driven by the policy push that followed Colonial. Federal agencies are required to remediate vulnerabilities on that list within defined windows.

The attack did not produce new security concepts. MFA, access reviews, and network segmentation between IT and OT environments were all documented controls before May 2021. Colonial is a case study in what happens when known controls are not applied to the systems that most need them.

---

*Written by Mario Martinez Jr. (ku5e / Gary7) | [TryHackMe Profile](https://tryhackme.com/p/ku5e) | [blog.ku5e.com](https://blog.ku5e.com)*
