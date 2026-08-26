# Privacy Policy Architecture Guide

A developer and agent guide for composing accurate, modular Privacy Policies based strictly on inspected codebase behavior.

---

## 1. Core Drafting Principles

1. **Behavior-Bound Accuracy**: A privacy policy must only describe what the software *actually* does. Never copy-paste boilerplate clauses for systems (e.g., ad networks, biometric sensors, cookies) that do not exist.
2. **Clear Plain-Language Communication**: Avoid obscure legalese where straightforward language can accurately describe the engineering realities.
3. **Explicit Sub-Processor Attribution**: Disclose specific third-party service providers (e.g., Stripe, PostHog, AWS, OpenAI) and their exact processing functions.
4. **Actionable Developer Placeholders**: Any variable requiring human business input (registered address, DPO contact, explicit retention windows) must be marked with `[Developer action required: ...]`.

---

## 2. Section Inclusion Decision Matrix

| Privacy Policy Section | Inclusion Criteria (Codebase Evidence) | When to OMIT |
| :--- | :--- | :--- |
| **1. Identity & Controller Info** | Always required. | Never omitted. |
| **2. Information We Collect** | Always required (customized to discovered data fields). | Never omitted. |
| **3. Account & Profile Data** | Included if registration, user DB tables, or auth providers exist. | Static sites, CLI tools, read-only utilities without accounts. |
| **4. Payment & Billing Data** | Included if Stripe, LemonSqueezy, Paddle, PayPal, or IAP SDKs exist. | Completely free apps with no transaction handling. |
| **5. Device, Log & Telemetry** | Included if server logging (Winston/Pino) or telemetry SDKs exist. | Zero-telemetry, offline-only applications. |
| **6. Cookies & Tracking Tech** | Included if `document.cookie`, cookies middleware, or web storage are used. | Pure API backends, CLI tools, cookie-less static sites. |
| **7. Third-Party Sub-processors** | Included if external API calls, SDKs, or cloud services are detected. | 100% self-hosted, air-gapped systems with zero external egress. |
| **8. AI & Machine Learning** | Included if OpenAI, Anthropic, Gemini, or custom ML models process user data. | Applications with no AI/ML inference features. |
| **9. Location Information** | Included if browser Geolocation API or mobile GPS permissions exist. | Apps that never request or process latitude/longitude/IP-geolocation. |
| **10. User-Generated Content** | Included if users can upload images, documents, publish posts, or comment. | Read-only applications or non-interactive dashboards. |
| **11. Data Retention Periods** | Always required (state specific timelines or flag as undefined). | Never omitted. |
| **12. Deletion & User Rights** | Always required (detail self-serve buttons or contact mechanisms). | Never omitted. |
| **13. Security Safeguards** | Always required (reflect real technical controls: TLS, encryption, bcrypt). | Never omitted. |
| **14. Children's Privacy** | Always required (state standard age thresholds like 13/16+). | Never omitted. |
| **15. Contact & Inquiries** | Always required. | Never omitted. |

---

## 3. Composing Specific Sections: Rules & Examples

### Section: Information We Collect
* **Rule**: Categorize by source: (a) Information provided directly by users, (b) Information collected automatically, and (c) Information from third-party sources (e.g. Google OAuth).
* **Code Alignment**: If the signup form collects only `email` and `username`, do NOT claim to collect "full legal name, physical address, and employer information".

### Section: Third-Party Service Providers (Sub-processors)
* **Rule**: Group by operational purpose with direct identification of the provider.
* **Example Clause**:
```markdown
We share data with trusted third-party service providers solely to operate our service:
* **Hosting & Infrastructure**: Amazon Web Services (Cloud compute and PostgreSQL database hosting in AWS us-east-1 region).
* **Payment Processing**: Stripe, Inc. (Card processing and subscription management; we never receive or store your raw credit card number).
* **Product Analytics**: PostHog, Inc. (Self-hosted / Cloud analytics to measure feature utilization).
* **Transactional Email**: Resend, Inc. (Delivery of password reset and system notification emails).
```

### Section: Artificial Intelligence & Machine Learning Processing
* **Rule**: Specify which model providers receive prompt data and whether prompts are retained or used for foundation model training.
* **Example Clause**:
```markdown
When you use our AI document summary features, your submitted text is transmitted to our AI model provider, OpenAI LLC, for processing. 
* OpenAI processes this data via their enterprise API tier under zero-data-retention terms and does not use customer API inputs to train future models.
* Generated responses may occasionally be inaccurate or incomplete.
```

### Section: Data Retention
* **Rule**: Be honest about retention lifecycles. If active accounts persist until manual deletion, state that plainly. Never claim "we delete your data immediately after every session" if database records are retained.
* **Example Clause**:
```markdown
We retain your personal information for as long as your account remains active. If you request account deletion, your profile information and uploaded files are permanently deleted from our primary production databases within 30 days. Encrypted database backups are automatically overwritten and purged on a 30-day rolling rotation cycle.
```

### Section: User Choices & Deletion Rights
* **Rule**: Detail the exact technical pathway available for users to exercise their rights (e.g. self-serve UI button vs. support ticket).
* **Example Clause**:
```markdown
You have the right to access, update, export, or delete your personal data at any time:
* **Account Deletion**: You can permanently delete your account and associated data directly within the application under **Settings > Account > Delete Account**.
* **Data Export**: You can request a machine-readable export of your data by emailing [Developer action required: insert privacy contact email].
```

---

## 4. Handling Unknowns and Developer Prompts

When inspecting code, if certain corporate or policy parameters cannot be verified from the codebase, use strict placeholder formatting:

```markdown
<!-- UNVERIFIED VARIABLE: Business Legal Entity Name -->
[Developer action required: Insert your registered company/organization legal name and registered address.]

<!-- UNVERIFIED VARIABLE: Support / Privacy Contact Email -->
[Developer action required: Insert official privacy contact email (e.g., privacy@yourdomain.com).]

<!-- UNVERIFIED VARIABLE: Inactive Account Purge Window -->
[Developer action required: Define if inactive accounts are automatically pruned after X months, or retained until explicit user deletion.]
```
