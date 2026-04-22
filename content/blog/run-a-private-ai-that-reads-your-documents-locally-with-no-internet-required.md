---
title: Run a Private AI That Reads Your Documents, Locally, With No Internet Required
date: 2026-04-22T18:58:00
draft: false
tags:
  - article
  - walkthrough
  - AI
  - RAG
  - ollama
  - privacy
  - anamnesis
  - draft
description: ku5e.com tutorial — run a private local AI with RAG using PrivateGPT and Ollama. No internet, no API keys, all local.
cover:
  image: /images/homeAIServer.png
  alt: ''
  caption: ''
  relative: false
---

The way RAG works is easier to understand if you stop thinking about AI memory. Think about a dictionary instead. You do not memorize every definition before you need one. Look up the word when you need it. RAG does the same thing with your files — chunks them, embeds them into a vector database, and pulls back only what matches your question. The model never sees the whole library.

I have been building this architecture by hand for months before I knew the term for it. Anamnesis, my local AI memory system, started as a flat set of Markdown files. Version 2 runs a short-term memory file with tag links to domain-specific MD files, plus a Python script that searches those files for relevant context before sending a prompt to the model. Version 3, when it gets built, moves mid-term memories that have not been accessed within a set window into a vector database, still linked from the same index. The Python script queries the database when the mid-term files come up empty. I built all of that from first principles before I learned that what I was describing has a name, and that someone has already packaged it as a ready-to-run product.

That product is PrivateGPT.

---

## Why the Context Window Has a Hard Ceiling

The Claude context window spec is public. I looked at the numbers, ran the extrapolation, and set a hard size limit on my soul.md file before it became a problem. That is why the vera-diary system exists as a separate git repo: overflow memory needs somewhere permanent to live, outside the active context window. A full memory dump does not scale. It is slow, it is expensive on token count, and once your files grow past a few thousand lines, you are wasting the model's attention on irrelevant material. RAG solves this by making retrieval a query instead of a dump.

---

## What You Need Before Starting

You need Ollama installed and a model pulled. If you have not done that, the setup walkthrough on this blog covers it. Then run:

```bash
ollama pull llama3.1
```

Windows users: type `wsl` in a terminal. Ubuntu installs itself. Every command from here runs inside that WSL environment.

---

## Setting Up PrivateGPT

Clone the repo and move into it:

```bash
git clone https://github.com/zylon-ai/private-gpt && cd private-gpt
```

PrivateGPT runs on Python 3.11. Use pyenv so you are not messing with whatever Python your system already has. Install it:

```bash
curl https://pyenv.run | bash
```

Drop these into your `.bashrc` and reload the shell:

```bash
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"
```

Pull 3.11.14 and pin it to this directory:

```bash
pyenv install 3.11.14
pyenv local 3.11.14
```

Now install Poetry, which handles the dependency environment:

```bash
curl -sSL https://install.python-poetry.org | python3 -
```

The installer prints the bin path — add it to your PATH. Then point Poetry at 3.11:

```bash
poetry env use python3.11
```

Install the dependencies with the extras that wire up Ollama and the Qdrant vector store:

```bash
poetry install --extras "ui llms-ollama embeddings-ollama vector-stores-qdrant"
```

Crack open `settings.yaml` and set the model to `llama3.1`. The environment name field is optional.

Start the server:

```bash
poetry run python -m private_gpt
```

Open a browser to `http://localhost:8001`.

---

## What Happens When You Upload a Document

Drag a PDF or text file in. PrivateGPT chunks it into segments, runs each chunk through an embedding model, and stores the vector representations in the local Qdrant database. Nothing leaves your machine. When you ask a question, the system converts your question into the same vector space, finds the closest matching chunks, and passes them to llama3.1 as context. The model generates an answer grounded in your document, not in its training data.

The interface gives you two modes. RAG mode queries your document library. Basic chat mode bypasses the vector store entirely and answers from training data only. Switching between them takes one click. The distinction matters when you want to know whether the answer came from your files or from the model's general knowledge.

---

## The nomic-embed-text Question

I already have `nomic-embed-text` installed on Phosphor, my Jetson Orin Nano. It is the embedding model that would power the Anamnesis RAG layer when that phase gets built. PrivateGPT uses the same class of model internally. The embedding model is not the one generating answers. It is the one converting text into vectors so the retrieval system can do its job. Those are separate concerns, and keeping them separate is what makes the architecture swappable.

---

## On Connecting a Personal Vault

My Obsidian vault is not connected to any RAG system and I have not decided if it will be. Teaching materials, health data, financial notes, years of project files — all of it is in there. A local RAG layer would make Anamnesis significantly more capable. It would also define the blast radius if the inference layer ever got compromised. A misconfigured API endpoint, a prompt injection in an uploaded document, a dependency with a known CVE: any of those could expose everything the RAG system can reach. The useful thing and the risky thing are the same connection. I have not resolved that tradeoff and I am not pretending I have.

If your use case is a folder of work documents, research papers, or public-facing notes, the risk profile is completely different. PrivateGPT is well-suited to that. Feed it your documents, ask it questions, everything stays local.

---

If you are building a portfolio in cybersecurity or just starting to figure out what direction to go, the Cybersecurity Career Roadmap lays out a tested path for $47. [ku5e.com/roadmap](https://ku5e.com/roadmap)

*Written by Mario Martinez Jr. (ku5e / Gary7) | [TryHackMe Profile](https://tryhackme.com/p/ku5e) | [ku5e.com/blog](https://ku5e.com/blog)*
