---
title: I Built AI Software in 2009. Here Is What ASU's New Learning Platform Gets Wrong.
date: 2026-04-28T21:15:00
draft: false
tags:
  - AI
  - education
  - curriculum
  - ASU
  - teaching
  - computer-science
description: Arizona State University launched Atomic, a platform that converts faculty lectures into AI-generated learning modules without faculty consent. The modules are factually wrong in places. This is what happens when institutions treat AI as a credential to sell instead of a discipline to teach.
cover:
  image: /images/AISchool.png
  alt: ''
  caption: ''
  relative: false
---

In 2009, I created the math algorithm for VIPRE and VASIS. The work used K-nearest neighbor pattern recognition applied to audio samples, trained to classify stress signatures with enough precision for production use. Nobody gave me a syllabus for that. My team and I built it by reading papers, writing code, testing against real data, and keeping a failure log that was longer than the documentation.

That was seventeen years before Arizona State University launched a platform called Atomic.

Atomic takes faculty lecture videos, feeds them through an AI model, and generates student learning modules from the output. Faculty say they were not told before their recorded lectures were processed. Independent testers found the resulting modules academically thin and factually wrong in places. ASU did not respond to those concerns with a curriculum review. It launched the platform.

The platform is a symptom of a pattern that runs through every institution that has decided to offer AI education without first asking whether anyone in the room has built anything with AI.

By the 2017-18 school year I was teaching the technical lineage from autocorrect to GPT-1 in a classroom: n-gram prediction, recurrent networks, the attention mechanism, transformers, scaled parameter counts. I taught that sequence three years before the word transformer entered the general vocabulary. In 2012 I had been following a research project at the Georgia Institute of Technology called LuminAI, the world's first AI designed to improvise movement with a human dancer in real time. I followed it because the technical approach was specific and testable, not because it had a user base.

When I built KU5E Academy, my own student assessment and learning platform, I made two decisions before writing a single feature. No personally identifiable information from students. No assessments accessible without content review, so a student who skipped the material could not reach the test. Those decisions came from seventeen years of watching what learning software does when it optimizes for completion metrics instead of actual learning.

ASU's Atomic platform made the opposite decisions at every one of those points.

The failure mode is not hypothetical. I have watched students use AI to generate code that runs but that they cannot explain, cannot debug when it breaks, and cannot extend when the next requirement arrives. The AI produced technically correct code. The student produced nothing. When requirements changed, they had no starting point.

The clearest version of this: a student turned in code I had written myself and posted publicly on GitHub. They did not know it was mine when they submitted it. They did not know when I pointed it out. They had no model of what the code did, only that it passed the tests. That student had been using AI for months.

The question is not whether AI belongs in a curriculum. It belongs there, and it will be there regardless of whether educators are ready. The question is who designs the curriculum and what they have actually built.

A platform that chops lecture video and reassembles it with an algorithm is a content pipeline. The label learning module does not change what the content does to a student who has no framework to evaluate whether the output is correct. The problem is not that ASU used AI. It is that the people who made the decision had no way to know what they were building, because they had never built anything like it.

Real AI education requires at least one person in the room who has a failure log. Who has trained a model on real data, watched it fail on edge cases, rebuilt the training set, and tested until the results were defensible. That experience is not the credential. It is what produces the judgment to know what students actually need to learn.

If you are responsible for AI curriculum at a school, district, or training program and want a practitioner's assessment of what is worth building and what will fail in year two, Curriculum Consulting starts at $500. [ku5e.com/services](https://ku5e.com/services)

*Written by Mario Martinez Jr. (ku5e / Gary7) | [TryHackMe Profile](https://tryhackme.com/p/ku5e) | [ku5e.com/blog](https://ku5e.com/blog)*
