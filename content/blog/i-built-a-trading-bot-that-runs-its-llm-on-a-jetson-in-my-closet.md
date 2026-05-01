---
title: I Built a Trading Bot That Runs Its LLM on a Jetson in My Closet
date: 2026-04-30T21:03:00
draft: false
tags:
  - article
  - ku5e
  - ai
  - infra
  - hardware
  - freelance
description: How I built a paper trading bot that runs its LLM on a Jetson Orin Nano in my home lab — architecture, the bug that would have been catastrophic, and why the model is explicitly blocked from touching execution.
cover:
  image: /images/Jetson1.png
  alt: ''
  caption: ''
  relative: false
---

**Topics:** Python, Alpaca, Ollama, Jetson Orin Nano, Trading Automation

***

The trading bot watches XNDU every five minutes during market hours. XNDU is a photonic quantum computing company. Photonics means room temperature operation. The cooling infrastructure that makes quantum computing prohibitively expensive at scale is not part of the design. XNDU had solid financials this week and got upgraded to a strong buy. I queued 100 paper shares for the 9:31 AM open on April 30, 10% trailing stop, $5,000 position cap.

The order filled at $30.32.

By 3:55 PM the price was $29.27. Down 3.4% from the fill. The floor sits at $27.29. The bot did not exit. The trailing stop does not move down, only up. The position stays open until XNDU either recovers past $30.32 or drops through the floor.

This is a paper trading account at Alpaca. No real money at stake. Every behavior running today will behave exactly the same when the account is live.

***

## What Broke on the First Real Run

Three bugs surfaced on the first real order. The one that would have done damage is the fill price mismatch.

Before an order fills, the bot estimates the trade price from the latest quote. I used that estimate to initialize the trailing stop state. After the order fills, Alpaca returns the actual fill price. On that first trade the gap was five cents. On a volatile open with a wide spread, that gap could be dollars. The trailing stop floor would be anchored to a price that was never the actual entry. The stop might trigger early, trigger late, or never trigger at all.

The fix: poll `get_fill_price()` after order confirmation and reinitialize trailing state with the actual fill. Obvious in retrospect. Invisible until the first real order came back.

The other two bugs were a deprecated API call (`BarSet` indexing replaced by `get_latest_trade`) and a missed 9:31 AM catchup job when the scheduler started after market open. Both were straightforward. The fill price mismatch was the one that would have corrupted the position management logic while the bot reported everything as normal.

***

## The LLM Boundary

The language model does not make trading decisions. It does not touch execution. It does not do math.

This was a deliberate sitsec and opsec call, not a response to a bad model output. LLMs make confident errors. Confident errors are harder to catch than obvious ones. Trailing stop math is simple. Whether the current price minus 10% exceeds the current floor is a three-line calculation with no ambiguity. There is no version of that logic where a language model should be involved.

At 3:55 PM each trading day, the model gets the day's data: position, P&L, number of checks run, any exits triggered, any copy trades queued. It writes a summary paragraph. That paragraph goes into an email. That is the full scope of what the model touches.

The consequence of this design: the bot does not degrade when the model is slow or fails. The scheduler keeps running. If Ollama returns a 500 error, the daily summary logs the failure and continues. The position is never at risk from an inference timeout.

***

## The Politician Copy Strategy

Capitol Trades aggregates congressional stock disclosures. When a congressman files a transaction, the site makes it available through an API. The bot polls that API on a 15-minute interval.

When a disclosure matches a configured politician, the bot queues a copy trade for the next 9:31 AM open at the configured position cap with a configurable delay. Currently: Michael McCaul, $5,000 cap, delay 0.

Delay 0 means the trade queues immediately on disclosure. Some disclosures come in days after the transaction date. The delay parameter exists to let you choose how much of that signal decay you want to absorb. A disclosure that arrives 72 hours after the trade executed is a different signal than one that arrives in two hours.

What people do not expect: the bot does not replicate the congressman's position size. A $250,000 purchase queues a $5,000 copy. The logic is that you are copying the signal, not the bet. The congressperson's bet size is information about their conviction. It is not a sizing instruction.

***

## What It Runs On

A Jetson Orin Nano 8GB. Unified memory architecture: 7.4GB shared between CPU and GPU. That ceiling is lower than it looks. The CUDA allocator and the OS share the same physical pool. A model that technically fits on paper gets evicted under the pressure of OS overhead plus inference load.

deepseek-r1:14b is the primary reasoning model at 9GB. Does not fit. llama3.1:8b at 4.9GB fits most of the time and crashes at the edge. llama3.2:3b runs at roughly 2GB and stays stable. That is the model the bot uses.

The scheduler starts four jobs at launch: trailing stop every five minutes during market hours, politician copy check every 15 minutes, pending orders at 9:31 AM, daily summary at 3:55 PM. The whole thing runs in a tmux session. After market close the bot is idle. The hardware is available for other inference tasks overnight.

***

## Where It Is Now

XNDU is down 3.4% on the first paper trade. The trailing floor is $27.29. The bot is holding.

The email delivery took longer to figure out than the trading logic. Brevo accepted the connection but emails did not deliver. Synology Mail Server direct delivery was blocked by Spamhaus PBL, which is standard for any residential IP attempting direct MX delivery. Mailjet relay through port 587 works. That is the current setup.

None of that was in the original spec.

***

If you want a Python automation tool built for your workflow, I build these as a freelance developer. Rates and availability at [ku5e.com/services](https://ku5e.com/services).

<!-- shrtly: https://ku5e.com/shrtly/A08045 -->

_Written by Mario Martinez Jr. (ku5e / Gary7) |_ [_TryHackMe Profile_](https://tryhackme.com/p/ku5e) _|_ [_ku5e.com/blog_](https://ku5e.com/blog)
