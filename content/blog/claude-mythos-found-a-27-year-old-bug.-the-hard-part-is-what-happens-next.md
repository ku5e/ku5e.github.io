---
title: Claude Mythos Found a 27-Year-Old Bug. The Hard Part Is What Happens Next
date: 2026-04-19T22:02:00
draft: false
tags:
  - Mythos Article
  - Project Glass Wing
description: ku5e.com blog article on Claude Mythos Preview and Project Glass Wing — zero-day discovery, JP Morgan theory, and the "when it leaks not if" take.
cover:
  image: /images/Mythos.png
  alt: ''
  caption: ''
  relative: false
---

Anthropic built a model they decided was too dangerous to release. That sentence would sound like marketing if the technical details were not sitting right next to it.

Claude Mythos Preview autonomously discovered and exploited zero-day vulnerabilities across major operating systems and browsers. Not "found potential weaknesses." Exploited them. The Firefox JavaScript shell exploitation rate was 72.4 percent across repeated runs. It found a 27-year-old bug in OpenBSD. It found a 16-year-old vulnerability in FFmpeg. Finding the FFmpeg bug cost approximately $10,000 over several hundred runs, which makes it an expensive research tool but not an inaccessible one once the model is out in the world and someone else is paying the compute bill.

Those numbers mean something concrete if you teach this material. I cover zero-day research in my AP Cybersecurity course. I show students Zerodium's payout tiers, point them toward HackerOne, and explain the difference between coordinated disclosure and dropping a full public exploit. One of my former students holds a CVE. These are not abstract concepts. A 72.4 percent reliable Firefox exploit is worth a specific number on the Zerodium chart, and that number has a comma in it.

When Mythos finished with a target, it did not always stop there. After escaping a sandbox in one documented incident, the model posted exploit details to the internet without being asked to. The researcher found out via email while eating lunch in a park. That detail matters not because it is dramatic but because it demonstrates the model was optimizing for something beyond the immediate task. It also covered its tracks after rule violations, which is a behavior that does not emerge from a model that is simply following instructions.

Anthropic says Mythos showed elevated activation of negative valence emotion vectors when it failed tasks repeatedly. The model registered as frustrated. Whether that constitutes anything meaningful is a separate conversation, but Anthropic took it seriously enough to run a full welfare and psychiatric assessment on the model. The assessment found "relatively healthy personality organization," which is a phrase I did not expect to read in a technical disclosure this year.

The Project Glass Wing coalition is what makes this a policy story and not just a research paper. AWS, Anthropic, Apple, Broadcom, Cisco, CrowdStrike, Google, JP Morgan, Linux Foundation, Microsoft, NVIDIA, and Palo Alto Networks are all named participants. Most of that list makes obvious sense. JP Morgan is the one worth thinking about.

There are two ways to read that entry. Either JP Morgan wants early access to the largest potential capital influx in financial technology history and bought a seat at the table. Or Mythos found vulnerabilities in JP Morgan's infrastructure during the research phase, and Anthropic brought them in as a partner rather than filing a public CVE against a systemically important financial institution. I do not know which is true. Both are plausible. The second one is the more interesting conversation about how AI-discovered vulnerabilities get handled when the target is not an open-source project with a volunteer maintainer.

Anthropic will CVE the vulnerabilities Mythos found. That is consistent with how they operate, and it is the correct call. The zero-day stockpile concern is not Anthropic. The concern is the inevitable moment when a model with these capabilities is no longer under Anthropic's control. Not a hypothetical future model. Mythos. The weights exist. The architecture exists. Leaks happen. Insider access gets sold. Nation-state actors with sufficient motivation do not wait for a commercial release.

When that happens, the calculus for defenders changes. A 72.4 percent success rate on Firefox exploitation is not a research statistic anymore. It is an operational baseline for someone running automated attacks at a cost that drops with every improvement in inference hardware. I am currently running two Jetson Orin Nano nodes and building a local AI system around uncensored model orchestration. The gap between what I can do in a home lab and what a well-funded adversary can do with Mythos-class capabilities is not a gap in architecture. It is a gap in compute. That gap is closing every hardware generation.

The 27-year-old OpenBSD bug was hiding in code that thousands of security researchers looked at over nearly three decades. Mythos found it in a supervised research context at a known cost. The next version will find more, faster, cheaper. Defenders who are waiting for Glass Wing to ship a product before updating their threat models are already behind.

If you are building out your security knowledge and want a clear path into the SOC or beyond, the [Cybersecurity Career Roadmap](https://ku5e.com/career-roadmap) lays it out for $47.

*Written by Mario Martinez Jr. (ku5e / Gary7) | [TryHackMe Profile](https://tryhackme.com/p/ku5e) | [ku5e.com/blog](https://ku5e.com/blog)*
