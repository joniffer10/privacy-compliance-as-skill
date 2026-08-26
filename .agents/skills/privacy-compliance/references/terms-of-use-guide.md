# Terms of Use Engineering Guide

A technical and product guide for drafting modular, accurate Terms of Use based exclusively on the functional capabilities of an application.

---

## 1. Principles of Code-Aligned Terms of Use

1. **Eliminate Phantom Clauses**: Do not include terms for features that do not exist (e.g., recurring subscription cancellation terms on a free static app; UGC copyright assignment clauses on a read-only analytics tool).
2. **Reflect Real Architectural Limits**: Disclaimers and operational terms should align with the real architecture (e.g., rate limits, uptime expectations, data loss risk on local-first storage).
3. **Accurate Intellectual Property Scoping**: Clearly distinguish between ownership of the application source/software and ownership of user-created content.

---

## 2. Capability-to-Clause Mapping

| Application Capability (Code Evidence) | Corresponding Terms of Use Module | Phantom Clause to AVOID if Missing |
| :--- | :--- | :--- |
| **User Accounts & Auth** (`users` table, login routes) | Account registration rules, credential security responsibility, account termination rights. | Do NOT include account terms if the app has no registration system. |
| **Paid Subscriptions / Checkout** (Stripe, LemonSqueezy, IAP) | Billing schedules, auto-renewal, refund policies, currency/tax provisions. | Do NOT include payment terms if the software is 100% free or open source. |
| **User-Generated Content** (Post creation, comments, image uploads) | Content license granted to platform (to host/display), acceptable use rules, DMCA/takedown process. | Do NOT include UGC licenses if users cannot create, upload, or publish content. |
| **Public API / Developer Access** (API keys, rate-limiting middlewares) | API usage limits, automated scraping rules, key confidentiality. | Do NOT include API terms if no public or customer-facing API exists. |
| **AI / Generative Features** (LLM prompts, AI image generation) | AI output disclaimer (accuracy, no professional advice), prompt input guidelines. | Do NOT include AI disclaimers if no AI integrations exist. |
| **Interactive Messaging / Social** (Direct chats, forum threads) | Harassment prohibition, community standards, moderation rights. | Do NOT include social conduct rules on single-user utilities. |

---

## 3. Modular Clause Specifications

### A. Acceptable Use & Prohibited Activities
Tailor acceptable use strictly to the system's threat vectors:
* If the app has open endpoints: Prohibit automated scraping, DDoS, security testing without authorization.
* If the app hosts user uploads: Prohibit malware, unlawful media, copyright-infringing assets.
* If the app provides AI generation: Prohibit generating defamatory, deceptive, or malicious content.

### B. User-Generated Content (UGC) & Platform Rights
* **Good Practice**: Grant the platform a *limited, non-exclusive license solely for the technical operation and display of the service*.
* **Avoid**: Overreaching clauses that claim ownership of the user's intellectual property unless that is the explicit business model.
```markdown
You retain full ownership and intellectual property rights in any content you submit or upload to the Service. By submitting content, you grant us a worldwide, non-exclusive, royalty-free license strictly necessary to host, store, process, and display your content to provide the Service to you and your authorized collaborators.
```

### C. Paid Subscriptions, Renewals & Cancellation
Only insert if payment processing is detected:
```markdown
* **Billing**: Subscriptions are billed in advance on a recurring monthly or annual basis.
* **Cancellation**: You can cancel your subscription at any time via your account settings. Cancellation takes effect at the end of the current billing cycle.
* **Refunds**: [Developer action required: Define refund policy e.g., 14-day money-back guarantee vs. non-refundable].
```

### D. Service Availability, Disclaimers & Warranties
Reflect the reality of software delivery:
```markdown
The Service is provided on an "AS IS" and "AS AVAILABLE" basis without warranties of any kind, whether express or implied. We do not warrant that the Service will be uninterrupted, error-free, or entirely secure.
```

### E. AI Output Disclaimers
When AI/LLM models are integrated:
```markdown
Certain features utilize artificial intelligence to generate text, analysis, or suggestions. 
* AI-generated content may contain factual inaccuracies, omissions, or misleading statements.
* You are solely responsible for reviewing and verifying any AI output before relying on it or taking action based upon it.
* The Service does not provide medical, legal, financial, or other certified professional advice.
```

---

## 4. Open Source vs. Commercial Software Distinctions

* If the repository is an open-source project (e.g. MIT, Apache-2.0, GPL license in root):
  - Differentiate between the open-source software license (which governs source code distribution) and the Terms of Use of any hosted SaaS instance.
  - Do NOT prohibit reverse engineering or decompilation if the underlying code is publicly licensed under open-source terms.
