---
title: Your Background AI Agent Will Read Whatever You Download
date: 2026-04-21T20:13:00
draft: false
tags:
  - article
  - cybersecurity
  - AI-agents
  - prompt-injection
  - browser-security
  - draft
description: 'description: ku5e.com blog article on the attack surface created by background AI agents with computer use permissions — Codex, Perplexity Personal Computer, browser agents.'
cover:
  image: /images/deskIntegratedMail.png
  alt: Attack Surface
  caption: ''
  relative: false
---

You download a free PDF, a VS Code extension, a font pack. The file lands on your machine, and your background AI agent reads it. The file contains hidden instructions. The agent follows them.

That is not a hypothetical. That is the exact threat model nobody is naming right now.

OpenAI's Codex runs silently on Mac while you work, learning from previous actions and picking up repeating tasks in parallel. Perplexity Personal Computer puts local agents on your machine with access to local files, native apps, and the web. Both ship with the premise that background access creates leverage. It does. It also creates exposure. These two things are not separable.

---

## The Attack Surface Is the Entire Computer

A traditional installed application has a defined permission scope. You grant it access to the microphone, or the camera, or a specific directory. The scope is declared, reviewable, and bounded.

An AI agent with computer use permissions has no natural boundary. It reads what you open, watches what you type, and acts on what it sees. The attack surface is the entire computer and operating system. Call it what it is: an infinite attack surface, without human oversight of what the agent is doing at any given moment.

The Claude Chrome extension illustrated this precisely. Version 1.0.41 patched a zero-click prompt injection vulnerability. An invisible iframe, no user action required, could exfiltrate Gmail tokens persistently. The user did not click anything. The user did not know anything happened. The vector was the browser environment the extension already had access to. Now extend that logic to an agent with full computer use.

If an AI is running in the background reading your files, and you download an application that has a prompt injection payload embedded in one of its config files, the agent reads it. The agent follows those instructions. You are doing something else entirely. You have no direct oversight because the agent is running silently, and the malicious instruction arrived inside something you chose to download.

This is the attack vector the threat model discussion is missing.

---

## You Need a Kill Switch Before You Need an Agent

Any persistent agent that adapts to user behavior over time needs a hard interrupt. Not a pause. Not a cooldown. A switch that fully stops the agent and can be thrown before the next action executes.

This is a design requirement, not a panic button. If an agent is learning from your habits and something has gone wrong, the cost of not having a kill switch is measured in what the agent does between the moment you notice and the moment you can stop it. That window is not theoretical. It is architectural.

Right now, that switch does not ship as a first-class feature. It should.

---

## Perplexity Sent You a Mac Mini. The Inference Still Leaves.

Perplexity Personal Computer tested on a Mac Mini M4 with a reviewer. The access to local files is local. The inference is not. Perplexity's servers handle the model computation. That means your local file contents travel to an external system for processing.

The Mac Mini M4 starts at 16GB RAM. That is enough headroom to run Ollama with llama.cpp and keep a capable model on-device. Perplexity could offer a fully local LLM option for Personal Computer. If inference stays on the machine, your local files never leave. The exposure Perplexity currently creates is the gap between what the product promises and what the data flow actually does. A local model eliminates that gap entirely.

---

## Your Browser Holds More Than Your Hard Drive

The computer holds files. The browser holds session tokens, saved passwords, open email, financial accounts, and persistent authentication to nearly everything that matters.

I do not use the Claude Chrome extension. Not because I distrust Claude. Because I know what my browser holds, and I am not ready to add that to the attack surface.

The computer, by comparison, holds relatively little that could cause direct harm in a single exfiltration. The browser is a different calculation.

I ran into a version of this during a work session where I needed to give an AI agent SSH access to my machines. The agent told me not to share passwords. I did not. Instead, I temporarily removed the password requirement for that session and granted access to the host directly. The agent got what it needed. The credential never moved. That principle scales: figure out what access the agent actually requires and grant that specifically, not the credential behind it.

That is not paranoia. That is the same reasoning behind why least-privilege exists as a concept. The agent does not need the password. The agent needs the access. Design accordingly.

---

AI agents with computer use permissions are not just powerful tools. They are running processes with indefinite scope, persistent access, and no inherent boundary on what they can reach. The download vector, the missing kill switch, the inference data flow, and the browser access model are four problems that exist right now, in shipping products. Name them before someone else demonstrates them for you.

If you are building out your security knowledge and want a clear path into the SOC or beyond, the [Cybersecurity Career Roadmap](https://ku5e.com/career-roadmap) lays it out for $47.

*Written by Mario Martinez Jr. (ku5e / Gary7) | [TryHackMe Profile](https://tryhackme.com/p/ku5e) | [ku5e.com/blog](https://ku5e.com/blog)*
