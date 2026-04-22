---
title: The Karpathy Loop
date: 2026-04-22T19:32:00
draft: false
tags:
  - article
  - AI
  - agentic-AI
  - optimization
  - business
  - cybersecurity
  - draft
description: ku5e.com blog article on the Karpathy Loop — the three-component auto-improvement pattern that lets an AI agent run hundreds of optimization experiments without human intervention, and what it means for small teams and business owners.
cover:
  image: /images/karpathy.png
  alt: ''
  caption: ''
  relative: false
---

March 8, 2026: Andrej Karpathy dropped a 630-line Python script, aimed an AI agent at his own training code with a single metric to chase, and went to bed. Two days later the agent had run 700 experiments, found 20 genuine improvements, and cut training time by 11%. It also found a bug in Karpathy's attention implementation that he had missed — not because the agent is smarter, but because it tried more things faster without getting bored after the 15th failed attempt.

That last part is the whole point.

---

## What the Loop Actually Is

The pattern has exactly three components. AI researcher [Nate B Jones](https://natesnewsletter.substack.com/) named them the Karpathy Triplet, and the name fits.

**One editable surface.** A single file the agent can modify. One file means the agent reads full context in a single pass without truncation, without fragmented state, without guessing which part of the system matters.

**One objectively testable metric.** Not "is this better?" but a number. Evaluation is either correct or wrong. There is no partial credit, no human judgment required per cycle, no committee.

**One fixed time budget per experiment.** The loop runs once, produces a score, loops again. Hundreds of times. The time budget prevents any single run from consuming the whole session.

That structure is not an accident. One metric means the agent cannot diffuse effort across competing goals. One time budget means it cannot spiral into a single experiment for three hours. One file means it always operates with complete information. Remove any component and the loop degrades into a research assistant that requires hand-holding. Keep all three and it runs unsupervised.

---

## The Data Behind the Pattern

Karpathy's result is the cleanest, but it is not isolated.

Shopify CEO Toby Lutke ran 37 experiments on internal company data in 8 hours and recorded a 19% performance gain. Sky Pilot ran 910 experiments in 8 hours on a 16-GPU Kubernetes cluster, spending under $300 in total compute. Sky Pilot's agent discovered that scaling model width mattered more than any individual parameter, and it spontaneously taught itself to use faster GPUs for validation runs. Nobody programmed that behavior. It emerged from the agent analyzing its own failure traces.

A single human researcher manages 8 to 10 experiment cycles per working day, and most of that time is waiting for a GPU. The loop does not sleep. It does not get frustrated. It does not stop at 6pm.

---

## What Third Layer Found

On April 2, 2026, a small YC startup called Third Layer applied the same pattern to a different target: the harnesses that control how agents behave. A meta-agent rewrote the task agent's entire scaffolding overnight. Third Layer published claimed scores of 96.5% on SpreadsheetBench and 55.1% on TerminalBench, first place on both.

Those scores have not appeared on the official leaderboards as of this writing. The highest verified SpreadsheetBench entry is Claude Opus 4.6 at approximately 34%. Third Layer's numbers should be treated as unverified until they are independently confirmed. The direction of the result is credible. The specific numbers are not yet established.

What Third Layer did confirm in their writeup is more interesting than the benchmark anyway.

The meta-agent was given failure traces, not just scores. When they removed the traces and gave it only outcome data (score went up, score went down), the improvement rate dropped significantly. The agent needed to read the reasoning behind each attempt to make targeted edits instead of random mutations. That is the infrastructure insight the benchmark obscures.

---

## Same-Model Pairings

Third Layer ran the meta-agent and task agent on the same model family. That was not obvious at the start. It turned out to matter.

A Claude meta-agent writes better harnesses for a Claude task agent than for a GPT-4 task agent. The meta-agent has implicit knowledge of how its inner model reasons, where it tends to fail, how it handles long context. That knowledge does not transfer cleanly across model families. Domain expertise and meta-improvement are separate skills. Same-model pairings close that gap in a way cross-model setups cannot match.

The meta-agent invented spot-checking on its own — running single tasks instead of the full benchmark whenever an edit was small enough not to warrant the full run. It built verification loops, pushed the task agent to write its own unit tests, solved context overflow by inventing progressive disclosure, and assembled sub-agents with handoff logic when scope required it. Third Layer specified none of this. The meta-agent found these strategies by reading its own failure traces.

---

## Local Hard Takeoff

This phrase has a meaning in AI safety that does not apply here. Local hard takeoff in a business context means something specific and bounded.

An optimization loop closes on one system inside your organization. Your pricing engine spends the weekend rewriting its own heuristics and comes back 30% more accurate. Your fraud detection model discovers patterns a human analyst would not try because the search space is too wide to explore manually. Your customer service agent builds verification loops that cut resolution time in half. The improvement is steep, fast, and compounding. It is also bounded. One domain. One metric. One sandbox. It does not escape into adjacent systems. It does not generalize. It gets very good at one thing very fast, and it stops there.

That is the shape of what Karpathy ran on his training code. 700 experiments, one metric, one file, contained to one system, 11% improvement in two days on code he had already spent months optimizing.

---

## I Built This Before I Had a Name for It

The KU5E Academy grader at academy.ku5e.com runs an AI agent against student short-answer and coding responses. The loop: agent evaluates a response, checks correctness against a rubric metric, commits the grade or flags it for human review. One night it ran against 88 responses without interruption.

That is the Karpathy triplet applied to educational assessment. One editable surface (the grading output), one objective metric (rubric criteria), one bounded scope per response. The agent does not drift. It does not fatigue at response 60. It flags ambiguous cases for me instead of forcing a decision it should not make.

I built this before Karpathy published that script. I did not have a name for the pattern. Now I do.

---

## What Most Organizations Get Wrong

Auto-improvement amplifies what already exists in your system. If your measurement infrastructure is weak, the loop finds ways to score well on a proxy metric while actual business value drifts. If your execution environment has no sandbox, silent degradation is possible before anyone notices. If your trace logging is shallow, the meta-agent is flying without instruments.

Four specific failure modes worth naming:

**Metric gaming.** The agent optimizes a proxy that diverges from the real goal. Response time goes down; resolution quality goes down with it. The score improves while the system gets worse.

**Silent degradation.** Subtle policy drifts accumulate across hundreds of edits. Nobody designed monitoring for autonomous changes at that cadence. The system erodes before the next human review cycle.

**Contamination.** The optimization loop influences the data it is evaluated against. Grading your own homework.

**Compounding errors.** A bad optimization in one interconnected system cascades into adjacent processes. This is the sandbox failure mode. If the loop runs on production, cascades are possible.

The prerequisites that most teams skip: structured external memory so every session knows what done means, eval harnesses that measure outcomes instead of activity, sandboxed execution environments, and a governance answer to who owns the output of an auto-improvement loop at 3am.

Build those before the loop. Not after.

---

## The Small Team Advantage

Karpathy's auto-research script: built by one person. Third Layer: a small YC startup. Sky Pilot's 910-experiment run: under $300 in compute.

Enterprise organizations trying to deploy this pattern face procurement cycles, compliance reviews, approval gates, and organizational complexity that add months between idea and running loop. A three-person team with $500 in compute can run the same optimization pass faster than a 20-person enterprise team can finish the requirements document.

On the specific dimension of rapid iterative optimization, small teams have a structural advantage that enterprise scale cannot overcome by default. The advantage erodes when the small team lacks eval infrastructure or trace logging. It holds when those are in place.

Human judgment does not disappear in this architecture. It concentrates. The human designs the experimental framework. The human defines the metric. The human decides what goes to production. The loop runs in between those decisions, faster than any human could run it.

---

## Implementation Path

1. Pick the most measurable business system you run. Not the most important. The most measurable.
2. Define the triplet: one file the agent can touch, one number that defines improvement, one time budget per run.
3. If you cannot define all three clearly, that is your first project.
4. Build the eval infrastructure before the loop: scoring function, test suite, sandboxed execution.
5. Do not start with customer-facing systems or anything inside a compliance workflow.
6. Log everything: experiments run, edits made, metric trajectory, ability to revert any change.
7. Set a review cadence. Weekly minimum. Read the traces, not just the scores.

The loop is not difficult to build once the triplet is defined. Most teams skip straight to building the loop before they can answer what the loop should optimize. That sequence fails every time.

---

If you are mapping a path into cybersecurity or AI security work and need to know which skills to build first, the [Cybersecurity Career Roadmap](https://ku5e.com/roadmap) covers the full sequence for $47. It is the document I wish I had when I was starting.

*Written by Mario Martinez Jr. (ku5e / Gary7) | [TryHackMe Profile](https://tryhackme.com/p/ku5e) | [ku5e.com/blog](https://ku5e.com/blog)*
