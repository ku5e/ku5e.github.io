---
title: "Security Audit — Starting at $499"
description: "Code security audit with a written findings report, specific fixes, and a round-by-round delivery. For indie devs launching a product, small businesses handling customer data, and teams blocked by a client's security requirements."
weight: 1
---

**Three tiers. Prices below. All audits include a written findings report with severity ratings, exact file and line locations, and fixes specific enough that a developer can implement them without guessing.**

---

### What an Audit Is Not

A vulnerability scanner is not an audit. A scanner finds known CVEs in your dependencies and flags common patterns. It does not read your authentication flow, test your password reset logic, or check whether an attacker can change a URL parameter and read another company's financial data.

Those are the findings that matter. Those are what this audit looks for.

---

### What I Find

**Account Takeover via Logic Flaw.** A client's password reset function did not expire old links. Trigger a reset, wait for the user to update their password, then use the original link to take the account anyway. Five thousand user accounts were exposed to this. One code change closed it.

**Broken Access Control.** A user changed their Organization ID in a URL parameter and could read another company's billing data without admin credentials. The app had no server-side check that the requested resource belonged to the requesting user. This class of vulnerability is in the OWASP Top 10 for a reason. It shows up in production codebases regularly.

---

### The Three Tiers

**Entry Scan — $499**

A one-round review of your five most critical endpoints: authentication, password reset, session handling, file upload, and your primary API surface. You get a written report with severity ratings (CRITICAL / HIGH / MEDIUM / LOW), exact locations, and specific fixes. Turnaround: 72 hours.

Right for: Indie devs and solo founders preparing for a public launch who need confirmation that the obvious doors are closed before the internet finds out they were not.

---

**Standard Audit — $1,200 to $2,500**

Ten rounds, each covering a separate category: dependencies, authentication, authorization, input validation, cryptography, file handling, rate limiting, infrastructure config, logging, and a final sweep. Each round runs on its own branch with a pull request documenting findings and fixes. At the end of each round, a tagged release is cut so you can download a zip of exactly where the code stands without knowing git.

Final deliverables: `AUDIT_REPORT.md` covering all ten rounds consolidated, and `MANUAL.md` documenting the hardened application and explaining the security decisions so your team does not undo them accidentally.

Price depends on codebase size and complexity. Quote on request after a brief intake.

Right for: Small businesses handling customer data, e-commerce stores, and local clinics that need to know their exposure before a breach notification letter becomes a business problem.

---

**Full Certification — Contact for Quote**

Everything in the Standard Audit plus a formal PDF report formatted for third-party review, a "Audited by ku5e" badge for your site, and a one-page executive summary written for a non-technical audience.

Right for: Teams that need to satisfy an enterprise client's legal department, respond to a VC's technical due diligence request, or begin a SOC 2 or HIPAA compliance process. If someone outside your company needs to read the report and act on it, this is the tier.

---

### What You Receive

Every audit includes:

- A written findings report. Every finding includes a severity rating, the exact file and line where it was found, an explanation of what an attacker can do with it, and a specific fix. No "consider improving your input validation."
- For Standard and Full tiers: ten tagged GitHub releases, each downloadable as a zip. You do not need git to access them.
- For Standard and Full tiers: `AUDIT_REPORT.md` and `MANUAL.md` in the repo root.
- Code comments written for humans. Every function touched during the audit gets a docstring explaining what it does, what it expects, and why the security decision was made the way it was.

---

### How to Order

Email [ku5e@ku5e.com](mailto:ku5e@ku5e.com) with the subject line **Security Audit**. Include the language and framework your app uses, a brief description of what it does, and whether you have source code to share. If you are asking about the Entry Scan, include the five endpoints you want reviewed. I will confirm scope and next steps within 24 hours.

For Standard and Full tiers, I will create a private GitHub repository, add you as a collaborator, and begin Round 1 within 48 hours of payment.

---

[Back to Services](/#services) | [Contact](mailto:ku5e@ku5e.com)
