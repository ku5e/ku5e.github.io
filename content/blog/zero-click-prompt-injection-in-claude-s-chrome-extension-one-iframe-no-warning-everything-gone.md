---
title: "Zero-Click Prompt Injection in Claude's Chrome Extension: One Iframe, No Warning, Everything Gone"
date: 2026-04-22T19:22:00
draft: false
tags:
  - article
  - cybersecurity
  - prompt-injection
  - browser-security
  - XSS
  - draft
description: ku5e.com blog article on the zero-click prompt injection vulnerability in Claude's official Chrome extension — patched in v1.0.41.
cover:
  image: /images/zeroClick.png
  alt: ''
  caption: ''
  relative: false
---

The attack required no action from the victim. Visit a page. Leave. By the time the browser tab closed, the extension had already talked to Claude, exported chat history, read Gmail, and potentially sent an email under your name. Patched in Claude Chrome extension v1.0.41.

Here is how the chain worked.

## The Attack Chain

The Claude Chrome extension trusted any page on `*.claude.ai` to send it messages. That wildcard, every subdomain under claude.ai, is where the attack found its entry point.

Anthropic hosted hCaptcha components from a third-party vendor, OARcost Labs, on `a-cdn.claude.ai`. First-party subdomain. Third-party code.

That hCaptcha component accepted `postMessage` from any website without checking `event.origin`. One field in the message controlled text displayed in the challenge UI. React's `dangerouslySetInnerHTML` rendered that text directly, no sanitization applied.

Finding a working version required nothing more than walking back CDN version numbers until an older, vulnerable hCaptcha build answered on `a-cdn.claude.ai`. It was sitting there.

So the full chain: an attacker sends a `postMessage` from a malicious page with an HTML payload like `<img src=x onerror=...>`. The hCaptcha component renders it. The `onerror` fires arbitrary JavaScript in the context of `a-cdn.claude.ai`. That subdomain is trusted by the extension. The extension receives an `onboarding_task` message with a `prompt` parameter and forwards it directly to Claude, no origin check. Claude executes it as if the user typed it.

The whole thing ran inside an invisible iframe. The victim saw a normal page. Nothing happened visibly. The chain ran silently in the background.

Claude with browser access could then read Gmail, pull Google Drive files, export conversation history, send email as the user. OAuth tokens, once exfiltrated, persist after the browser closes.

## Which Mistake Actually Ends It

People will point to the wildcard subdomain trust as the architectural failure. The wildcard subdomain trust is the architectural failure, `dangerouslySetInnerHTML` with no sanitization on a first-party subdomain is the one that removes the user's options entirely. No browser setting, no extension toggle, no permission prompt gives the user a way out of how a vendor renders its own component. The wildcard trust compounds the damage, but the XSS on `a-cdn.claude.ai` is what turns a configuration mistake into a complete compromise. That is the step where the user loses all agency.

I teach my students not to trust even trusted sites. Not as a worst-case exercise. As the default posture. The trust chain that made this attack work is the one most developers assume is safe: we control the domain, the code is from a known vendor, the subdomain is ours. Attackers specifically wait for that assumption to harden into policy.

Third-party code on a first-party domain is not first-party code. It does not matter whose name is on the subdomain.

## I Do Not Use the Claude Chrome Extension

I made that call before this disclosure. Not because I predicted this specific attack. Because browser extensions that can read page content and interact with an AI on your behalf have an enormous surface area, and the blast radius when something goes wrong is everything your browser can touch.

I use Claude Code in the terminal. I build sessions, commit work, manage files. The extension's convenience is real. The tradeoff was not one I was willing to make.

## The Anamnesis Problem

I am building a local AI called Anamnesis. It has access to my memory files, my soul file, my project notes, my personal infrastructure. I do not hard-limit what it can read. The whole point is that it knows me.

But I do not share API tokens, keys, or passwords with it. That line is deliberate and firm.

The lesson from this vulnerability is not that AI in the browser is always a bad idea. It is that the permissions you grant define the worst-case outcome when something goes wrong. The Claude extension could exfiltrate Gmail tokens because it had Gmail access. You cannot lose what you did not give it.

Every AI agent you deploy, in the browser or anywhere else, has a blast radius determined by what you connected to it. Define that carefully. The researcher who found this chain was disclosing responsibly. The next person who finds a chain like it may not be.

---

If you are trying to build a cybersecurity career and want a clear path from where you are now to your first security role, the Cybersecurity Career Roadmap covers it in full for $47. [ku5e.com/roadmap](https://ku5e.com/roadmap)

*Written by Mario Martinez Jr. (ku5e / Gary7) | [TryHackMe Profile](https://tryhackme.com/p/ku5e) | [ku5e.com/blog](https://ku5e.com/blog)*
