---
title: The Ethical AI Company Billed You for Using Competitor Tools
date: 2026-05-10T22:30:00
draft: false
tags:
  - article
  - ku5e
  - ai
  - cyber
description: Anthropic's billing system scanned user repositories for competitor keywords and charged extra or blocked access when it found them. The gap between stated values and operational behavior.
cover:
  image: /images/Flux2_dev_00015_.png
  alt: ''
  caption: ''
  relative: false
---

**Topics:** Anthropic, Claude Code, AI Ethics, Billing, Vendor Trust

---

Anthropic's detection logic found "hermes.md" in a user's git commit history. The user was on the $200/month Claude Max plan with 86% of their usage allocation untouched and no active session running. Anthropic billed $200.98 in extra charges. When the user reported it, support acknowledged the billing error three times and refused the refund.

The post reached 1.4 million views. Anthropic then issued the refund plus one month of credit.

---

## What the Sequence Tells You

The charge was wrong from the moment it posted. The user's usage logs showed 86% allocation remaining. The support team acknowledged the error in three separate responses.

The refund came after 1.4 million people saw the post.

Anthropic did not refund the charge because it was wrong. They refunded it because 1.4 million people watched them not refund it.

---

## What Was Actually Built

This was not a misconfigured rate limiter or a counter that incremented incorrectly.

Someone at Anthropic decided to write detection logic that scans the git history of paying customers' repositories for competitor keyword strings. Someone decided that finding "hermes" or related harness references was evidence of misuse. Someone decided that evidence of misuse justified billing extra or blocking access. Those were decisions, made by people, reviewed by other people, and shipped to production.

Theo Browne, who first published the finding, framed it precisely: there is a class of bugs that suggests the thing you are trying to do is a bad idea. The fact that the detection was written into production at all is the thing people are taking issue with, not just the billing behavior that resulted.

---

## The Particular Irony

Anthropic's public identity is built on ethical AI development. They publish safety research. They have drawn and held public lines on military contracts. They position themselves as the company in the space that takes responsibility seriously.

That positioning is also what makes the gap between stated values and operational behavior visible. If a company no one had heard of built billing logic that penalized customers for using competitor tools, the story would be smaller. Anthropic built it, and the company's own brand made it a story worth 1.4 million views.

---

## The Lesson That Applies Broadly

Trust in a vendor's stated values is not a security control. Anthropic's public ethics commitments do not protect you from operational decisions that conflict with them.

The accountability mechanism that actually worked here was public attention at scale. The refund came because the post went viral, not because the support team reviewed the charge and found it wrong.

That is not a stable foundation for a vendor relationship on tools that touch your code, your API keys, and your billing. The practical response is treating vendor ethics statements as marketing and evaluating vendors on operational behavior instead: what they do when something goes wrong and nobody is watching.

If you are evaluating AI tooling for a team and want to think through vendor risk and architecture decisions, I take consulting engagements through [ku5e.com/services](https://ku5e.com/services).

<!-- shrtly: https://ku5e.com/shrtly/22EAA9 -->

*Written by Mario Martinez Jr. (ku5e / Gary7) | [TryHackMe Profile](https://tryhackme.com/p/ku5e) | [ku5e.com/blog](https://ku5e.com/blog)*
