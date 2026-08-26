# 🛡️ Privacy & Compliance Agent Skill

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Status: Production Ready](https://img.shields.io/badge/Status-Production--Ready-brightgreen.svg)](#)
[![Framework Agnostic](https://img.shields.io/badge/Stack-Framework--Agnostic-orange.svg)](#)

> **An engineering-first, behavior-driven privacy, compliance, and product transparency agent skill for coding assistants and developer workflows.**

The full skill definition and resources reside under [`.agent/skills/privacy-compliance/`](.agent/skills/privacy-compliance/).

---

## 🎯 Core Principle

> **Never generate legal or privacy documents blindly. Inspect the application first, understand what it actually does, then generate disclosures and recommendations based on real application behavior.**

Traditional privacy generators produce bloated, generic legalese full of **phantom clauses** for features that do not exist (e.g. subscription terms on a free static app) or omit high-risk data flows that *do* exist (e.g. unconsented camera access or analytics exfiltration). 

This skill bridges the gap between **codebase reality** and **public disclosures**.

```text
Application Codebase & Architecture
                 ↓
      Inspect Actual Behavior
                 ↓
     Identify Data Practices
                 ↓
   Identify Third-Party Services
                 ↓
Identify Privacy/Compliance Risks
                 ↓
  Generate Bespoke Policy Drafts
                 ↓
Compare Policies ↔ Actual Behavior
                 ↓
    Identify Inconsistencies (P0–P3)
```

---

## 📂 Structure & Sitemap

```text
.agent/skills/privacy-compliance/
├── SKILL.md                          # Primary agent instruction file
├── README.md                         # Detailed skill guide
├── LICENSE                           # MIT License
│
├── references/                       # In-depth technical guides
│   ├── data-practices.md             # Codebase inspection patterns & signatures
│   ├── privacy-policy-guide.md       # Modular privacy policy architecture
│   ├── terms-of-use-guide.md         # Capability-mapped terms (no phantom clauses)
│   ├── consent-and-disclosures.md    # Permission priming, cookie banners & UX
│   ├── trust-and-transparency.md     # Operator credibility & destructive action UX
│   └── compliance-awareness.md       # High-risk vectors, regulations & legal limits
│
├── templates/                        # Modular, tokenized policy templates
│   ├── privacy-policy.md             # Conditional privacy policy template
│   ├── terms-of-use.md               # Capability-bound terms of use
│   ├── cookie-notice.md              # Cookie policy & UI banner copy
│   ├── data-deletion.md              # Account & data deletion instructions
│   └── ai-disclosure.md              # AI feature & model usage notice
│
└── checklists/                       # Actionable verification checklists
    ├── privacy-audit.md              # Step-by-step codebase inspection checklist
    ├── policy-consistency.md         # Policy-to-code verification matrix (P0–P3)
    └── release-checklist.md          # Pre-deployment web & app store checklist
```

---

## 🛠️ Quick Start

To use with Antigravity or compatible coding agents:
1. Place `.agent/skills/privacy-compliance` in your workspace or global config.
2. Prompt: `"Perform a privacy audit of our codebase, identify data practices, and verify our public policies."`

---

## ⚠️ Non-Legal Advice Disclaimer

This agent skill provides **software engineering, architectural auditing, and UX transparency guidance**. It does **not** constitute formal legal advice, nor does it provide legally binding regulatory certifications.

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).
