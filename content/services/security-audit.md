---
title: "Security Audit — Starting at $499"
description: "Code security audit with a written findings report, specific fixes, and a round-by-round delivery. For indie devs launching a product, small businesses handling customer data, and teams blocked by a client's security requirements."
weight: 10
aliases: ["/audit"]
priceRange: "$499-$2500"
faq:
  - q: "What is the difference between a vulnerability scanner and a manual security audit?"
    a: "A vulnerability scanner finds known CVEs in dependencies and flags common patterns. A manual audit reads your authentication flow, tests your password reset logic, and checks whether an attacker can manipulate URL parameters to access another user's data. Those logic flaws are what this audit finds."
  - q: "What does the Entry Scan cover?"
    a: "A one-round review of five critical endpoints covering authentication, authorization, data exposure, input validation, and session management. Turnaround is 72 hours. Price is $499."
  - q: "What do I receive when the audit is complete?"
    a: "A written findings report with severity ratings (Critical, High, Medium, Low), exact file and line locations for each finding, and specific fixes written clearly enough that a developer can implement them without guessing. Standard and Full tiers also include GitHub releases with tagged branches for each audit round."
  - q: "What kinds of vulnerabilities do you find?"
    a: "Broken access control, insecure authentication flows, logic flaws in password reset and account recovery, exposed sensitive data, injection points, and OWASP Top 10 issues that automated scanners miss because they require reading business logic."
  - q: "How do I start a security audit?"
    a: "Email mario@ku5e.com with a description of your codebase and the tier you want. Scope confirmation within 24 hours. Project starts within 48 hours after payment."
---

**Three tiers. Prices below. All audits include a written findings report with severity ratings, exact file and line locations, and fixes specific enough that a developer can implement them without guessing.**

***

### What an Audit Is Not

A vulnerability scanner is not an audit. A scanner finds known CVEs in your dependencies and flags common patterns. It does not read your authentication flow, test your password reset logic, or check whether an attacker can change a URL parameter and read another company's financial data.

Those are the findings that matter. Those are what this audit looks for.

***

### What I Find

**Account Takeover via Logic Flaw.** A client's password reset function did not expire old links. Trigger a reset, wait for the user to update their password, then use the original link to take the account anyway. Five thousand user accounts were exposed to this. One code change closed it.

**Broken Access Control.** A user changed their Organization ID in a URL parameter and could read another company's billing data without admin credentials. The app had no server-side check that the requested resource belonged to the requesting user. This class of vulnerability is in the OWASP Top 10 for a reason. It shows up in production codebases regularly.

***

### The Three Tiers

**Entry Scan — $499**

A one-round review of your five most critical endpoints: authentication, password reset, session handling, file upload, and your primary API surface. You get a written report with severity ratings (CRITICAL / HIGH / MEDIUM / LOW), exact locations, and specific fixes. Turnaround: 72 hours.

Right for: Indie devs and solo founders preparing for a public launch who need confirmation that the obvious doors are closed before the internet finds out they were not.

***

**Standard Audit — $1,200 to $2,500**

Ten rounds, each covering a separate category: dependencies, authentication, authorization, input validation, cryptography, file handling, rate limiting, infrastructure config, logging, and a final sweep. Each round runs on its own branch with a pull request documenting findings and fixes. At the end of each round, a tagged release is cut so you can download a zip of exactly where the code stands without knowing git.

Final deliverables: `AUDIT_REPORT.md` covering all ten rounds consolidated, and `MANUAL.md` documenting the hardened application and explaining the security decisions so your team does not undo them accidentally.

Price depends on codebase size and complexity. Quote on request after a brief intake.

Right for: Small businesses handling customer data, e-commerce stores, and local clinics that need to know their exposure before a breach notification letter becomes a business problem.

***

**Full Certification — Contact for Quote**

Everything in the Standard Audit plus a formal PDF report formatted for third-party review, a "Audited by ku5e" badge for your site, and a one-page executive summary written for a non-technical audience.

Right for: Teams that need to satisfy an enterprise client's legal department, respond to a VC's technical due diligence request, or begin a SOC 2 or HIPAA compliance process. If someone outside your company needs to read the report and act on it, this is the tier.

***

### What You Receive

Every audit includes:

- A written findings report. Every finding includes a severity rating, the exact file and line where it was found, an explanation of what an attacker can do with it, and a specific fix. No "consider improving your input validation."
- For Standard and Full tiers: ten tagged GitHub releases, each downloadable as a zip. You do not need git to access them.
- For Standard and Full tiers: `AUDIT_REPORT.md` and `MANUAL.md` in the repo root.
- Code comments written for humans. Every function touched during the audit gets a docstring explaining what it does, what it expects, and why the security decision was made the way it was.

***

### How to Order

Email [ku5e@ku5e.com](mailto:ku5e@ku5e.com) with the subject line **Security Audit**. Include the language and framework your app uses, a brief description of what it does, and whether you have source code to share. If you are asking about the Entry Scan, include the five endpoints you want reviewed. I will confirm scope and next steps within 24 hours.

For Standard and Full tiers, I will create a private GitHub repository, add you as a collaborator, and begin Round 1 within 48 hours of payment.

***

[Back to Services](/#services) | [Contact](mailto:ku5e@ku5e.com)
