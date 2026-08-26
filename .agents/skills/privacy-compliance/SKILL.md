---
name: privacy-compliance
description: Inspect application codebases for real data practices, perform privacy audits, generate behavior-accurate privacy policies and terms, and verify policy-to-code consistency.
---

# Privacy & Compliance Skill

A comprehensive, engineering-first agent skill designed to inspect an application's actual codebase, map its real-world data practices, audit UX disclosures and permissions, generate bespoke legal/transparency documents, and verify that public policies match actual system behavior.

> **Core Principle**: Never generate legal or privacy documents blindly. Inspect the application first, understand what it actually does, then generate disclosures and recommendations based on real application behavior. Never invent data practices, legal requirements, or compliance claims.

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
    Identify Inconsistencies
```

---

## 1. Skill Capabilities & Objectives

1. **Codebase Inspection**: Deeply analyze frontend code, server routes, database schemas, configuration files, environment variables, dependencies, and storage patterns.
2. **Data Practice Inventory**: Construct a structured data ledger detailing collected data types, collection purposes, sources, storage destinations, access scopes, retention limits, deletion paths, and security measures.
3. **Third-Party Sub-Processor Mapping**: Discover and map all external SDKs, APIs, CDNs, analytics tools, crash reporters, payment processors, and AI providers.
4. **Policy & Terms Generation**: Synthesize tailor-made, modular Privacy Policies, Terms of Use, Cookie Notices, Data Deletion Guides, and AI Disclosures reflecting *only* active features.
5. **Consistency Auditing**: Cross-reference public-facing policies against live code to catch undeclared data flows, phantom legal clauses, or deceptive claims (categorized by severity P0–P3).
6. **Trust & Privacy UX Engineering**: Audit permission priming, contextual explanations, data minimization opportunities, and destructive action protections.
7. **Compliance Awareness (Non-Legal Advice)**: Surface high-risk flags (e.g., children's data, biometrics, health records, cross-border flows, App Store review mandates) and recommend professional legal counsel when necessary.

---

## 2. 12-Step Agent Workflow

When executing a privacy and compliance task, execute these steps systematically:

```text
 1. Inspect Application (Dependencies, config, DB models, API endpoints, SDKs, frontend)
 2. Build Data Practice Inventory (Classify data types, storage, purpose, lifecycles)
 3. Identify Third-Party Services & Sub-processors (APIs, tracking, infrastructure)
 4. Identify Existing Policies & Disclosures (Inspect `/privacy`, `/terms`, banners, modals)
 5. Identify Missing Information (Flag unknowns requiring developer input)
 6. Ask Focused Developer Questions (Max 3–7 targeted questions only when unverifiable via code)
 7. Determine Required Disclosures & Policies (Filter unnecessary document generation)
 8. Generate Tailored Policy Drafts (Populate modular templates with real data)
 9. Run Policy ↔ Code Consistency Audit (Detect discrepancies, undeclared behaviors, phantom terms)
10. Identify Implementation Gaps (Flag missing deletion endpoints, unencrypted PII, undefined retention)
11. Improve Trust & Transparency UX (Design permission priming, microcopy, clear deletion flows)
12. Produce Final Audit Report & Action Plan (Deliver structured summary with P0–P3 priorities)
```

---

## 3. Mandatory Application Inspection

Before drafting any policy or audit report, thoroughly scan the codebase using targeted grep/file analysis across the following vectors:

### A. Authentication & User Identity
- **Inspect**: Registration forms, OAuth callbacks, JWT decoders, session handlers, database user schemas.
- **Signals**: `email`, `password`, `hash`, `bcrypt`, `argon2`, `phone`, `username`, `avatar`, `social_login`, `next-auth`, `clerk`, `supabase`, `firebase.auth()`, `lucia-auth`.

### B. Device, Hardware & Native Permissions
- **Inspect**: Manifest files (`AndroidManifest.xml`, `Info.plist`), mobile permissions, web browser APIs.
- **Signals**: `navigator.geolocation`, `navigator.mediaDevices.getUserMedia`, `Camera`, `Microphone`, `Contacts`, `PushNotifications`, `BackgroundLocation`, `Bluetooth`.

### C. Client-Side Storage & Tracking
- **Inspect**: Browser storage operations, cookie parsing/setting, web trackers.
- **Signals**: `localStorage.setItem`, `sessionStorage`, `document.cookie`, `useCookie` (Nuxt), `js-cookie`, `IndexedDB`, Web Workers.

### D. Analytics, Telemetry & Ads
- **Inspect**: Dependency manifests (`package.json`, `podfile`, `build.gradle`), script tags, event dispatchers.
- **Signals**: `gtag`, `GoogleAnalytics`, `PostHog`, `Mixpanel`, `Segment`, `Amplitude`, `Sentry`, `Bugsnag`, `LogRocket`, `FacebookPixel`, `GoogleAdSense`, `AppsFlyer`.

### E. AI & Machine Learning Integrations
- **Inspect**: API clients connecting to LLM or AI inference endpoints.
- **Signals**: `openai`, `@anthropic-ai/sdk`, `replicate`, `langchain`, `llamaindex`, `vllm`, `HuggingFace`, `gemini-api`, streaming prompts, embeddings storage.

### F. Payments, Subscriptions & E-Commerce
- **Inspect**: Webhook handlers, checkout sessions, billing portals.
- **Signals**: `stripe`, `paddle`, `lemonsqueezy`, `braintree`, `paypal`, `revenuecat`, `in-app-purchase`.

### G. Server Logging & Infrastructure
- **Inspect**: Logger configurations, cloud storage buckets, email delivery, SMS services.
- **Signals**: `winston`, `pino`, `morgan`, `AWS S3`, `Google Cloud Storage`, `SendGrid`, `Resend`, `Postmark`, `Twilio`.

*For detailed code-level detection patterns, see [data-practices.md](file:///Users/ccci/Documents/CCCI-automation%20testing/private/privacy-compliance/references/data-practices.md).*

---

## 4. Structured Data Practice Inventory

Organize discovered data practices into this normalized schema. Never fabricate information:

| Data Field | Practice Property | Standard Inspection Questions |
| :--- | :--- | :--- |
| **DATA TYPE** | What specific information is handled? | `Email address`, `IP address`, `GPS coordinates`, `Uploaded image` |
| **PURPOSE** | Why is it collected or processed? | `Account authentication`, `Order fulfillment`, `Usage metrics` |
| **SOURCE** | Where does it originate? | `Direct user input`, `Automated HTTP header`, `Device OS API` |
| **STORAGE** | Where does it reside at rest? | `PostgreSQL (RDS us-east-1)`, `Redis session cache`, `Client localStorage` |
| **ACCESS** | Who or what can access it? | `Application backend`, `Customer support role`, `Public read` |
| **THIRD PARTIES** | Which external entities receive it? | `Stripe (Payment)`, `Postmark (Transactional email)`, `None` |
| **RETENTION** | How long is it maintained? | `Until account closure + 30 days`, `⚠ Retention policy not defined` |
| **DELETION** | How is it erased or anonymized? | `Hard delete on user request`, `Soft delete (anonymize after 90d)`, `None` |
| **SECURITY** | What safeguards protect it? | `TLS in transit`, `AES-256 at rest`, `Bcrypt password hashing` |

```markdown
### Example Inventory Entry
* **Data Type**: Email Address
* **Purpose**: User authentication, account recovery, security alerts
* **Source**: Direct input during signup form
* **Storage**: Primary PostgreSQL `users` table (`email` column)
* **Access**: Authenticated backend services; read-only access for support admin tier
* **Third Parties**: Postmark (for transactional email delivery)
* **Retention**: ⚠ Retention policy not defined in codebase (active indefinitely)
* **Deletion**: Cascading hard-delete triggered via `/api/user/delete-account` endpoint
* **Security**: Enforced TLS 1.3 in transit; encrypted at rest via AWS RDS volume encryption
```

---

## 5. Decision Tree: Generating Tailored Policy Documents

Do **not** produce every legal document for every repository. Use this decision matrix:

```text
Does application store user accounts / personal data?
├── YES ──> [Generate Privacy Policy]
└── NO  ──> Does it use tracking cookies, analytics, or external APIs?
            ├── YES ──> [Generate Lightweight Privacy & Cookie Notice]
            └── NO  ──> [Generate Minimal Privacy Statement]

Does application provide interactive services, accounts, user content, or paid features?
├── YES ──> [Generate Terms of Use]
└── NO  ──> (Static informational site) ──> [Omit Terms or provide simple Disclaimer]

Does application set client cookies / local tracking identifiers?
├── YES ──> [Generate Cookie Notice & Policy]
└── NO  ──> [Omit Cookie Notice]

Can users register or store persistent data?
├── YES ──> [Generate Data/Account Deletion Instructions]
└── NO  ──> [Omit Data Deletion Instructions]

Does application send user inputs to AI/LLM models or generate synthetic outputs?
├── YES ──> [Generate AI Feature Disclosure]
└── NO  ──> [Omit AI Disclosure]
```

*Templates reference: [privacy-policy.md](file:///Users/ccci/Documents/CCCI-automation%20testing/private/privacy-compliance/templates/privacy-policy.md), [terms-of-use.md](file:///Users/ccci/Documents/CCCI-automation%20testing/private/privacy-compliance/templates/terms-of-use.md), [cookie-notice.md](file:///Users/ccci/Documents/CCCI-automation%20testing/private/privacy-compliance/templates/cookie-notice.md), [data-deletion.md](file:///Users/ccci/Documents/CCCI-automation%20testing/private/privacy-compliance/templates/data-deletion.md), [ai-disclosure.md](file:///Users/ccci/Documents/CCCI-automation%20testing/private/privacy-compliance/templates/ai-disclosure.md).*

---

## 6. Policy ↔ Code Consistency Auditing

When comparing existing or proposed policies against the code, classify discrepancies according to this severity taxonomy:

| Level | Severity | Definition & Impact | Example |
| :--- | :--- | :--- | :--- |
| **P0** | **Critical Discrepancy** | Active undeclared high-risk data collection, false deletion claims, unconsented third-party exfiltration. | Code sends location coordinates to advertising SDK without user notice or policy disclosure. |
| **P1** | **Major Disclosure Gap** | Active data practices or sub-processors missing from disclosures; undefined core lifecycles. | Code uses PostHog analytics or Stripe payments, but Privacy Policy fails to list analytics/payment processors. |
| **P2** | **Transparency / UX Flaw** | Misleading microcopy, unprimed OS permission prompts, vague retention, phantom legal clauses. | Terms of Use discuss subscription refunds, but the app is completely free. OS camera prompt has no contextual pre-dialogue. |
| **P3** | **Minor Enhancement** | Typographical clarity, formatting updates, explicit contact channel mentions. | Adding direct link to support contact form instead of generic mailto. |

*Detailed consistency checklist: [policy-consistency.md](file:///Users/ccci/Documents/CCCI-automation%20testing/private/privacy-compliance/checklists/policy-consistency.md).*

---

## 7. Privacy Engineering Rules

Enforce these foundational privacy engineering principles across every audit and recommendation:

1. **Data Minimization**:
   - Flag collection of fields that are never read, indexed, or required for core features (e.g., requesting full birthdate when only age verification $>18$ is required; requesting precise GPS when city-level IP resolution suffices).
2. **Purpose Limitation**:
   - Verify that data gathered for purpose $A$ (e.g., two-factor SMS authentication) is strictly isolated and never repurposed for purpose $B$ (e.g., marketing SMS).
3. **Storage & Retention Hygiene**:
   - Every stored record must have a definable lifecycle. Flag `TODO`s, unlimited database retention, and orphaned S3 file uploads.
4. **Guaranteed & Verifiable Deletion**:
   - Distinguish between *soft deletion* (flagging `deleted_at = NOW()`) and *actual data purging*.
   - Ensure account deletion handles linked records, access tokens, uploaded media, cached sessions, and third-party sub-processors.
5. **Contextual Transparency**:
   - Inform the user *at the moment of data capture* why the data is requested and how it will be utilized.

*Reference: [trust-and-transparency.md](file:///Users/ccci/Documents/CCCI-automation%20testing/private/privacy-compliance/references/trust-and-transparency.md) and [consent-and-disclosures.md](file:///Users/ccci/Documents/CCCI-automation%20testing/private/privacy-compliance/references/consent-and-disclosures.md).*

---

## 8. Compliance Awareness & Legal Boundaries

> [!IMPORTANT]
> This skill provides **software engineering, architectural auditing, and UX transparency recommendations**. It does **not** provide formal legal counsel or legally binding compliance certifications.

When evaluating compliance factors:
- **High-Risk Flags**: Immediately flag when processing children's data (COPPA/GDPR-K), health/wellness telemetry, biometric data, precise background geolocation, or financial credentials.
- **Jurisdiction Awareness**: Point out that requirements vary across jurisdictions (e.g., EU/EEA GDPR, California CCPA/CPRA, Apple App Store App Privacy guidelines, Google Play Data safety), without fabricating region-specific legal mandates.
- **Recommendation Protocol**: Where legal liability is significant, recommend that the developer have the generated draft reviewed by qualified legal counsel.

*Reference: [compliance-awareness.md](file:///Users/ccci/Documents/CCCI-automation%20testing/private/privacy-compliance/references/compliance-awareness.md).*

---

## 9. Developer Interaction Protocol

Do **not** bombard developers with generic 50-question forms. Follow this protocol:

1. Inspect the codebase first to extract answers autonomously (e.g., database schema, dependencies, env vars, routes).
2. Only ask 3 to 7 high-impact, targeted questions for parameters that *cannot* be derived from code:
   - Official legal entity name and physical/mailing jurisdiction.
   - Primary user support / DPO contact email.
   - Business retention timelines for inactive accounts/logs.
   - Primary geographical target audience / market.
   - Whether offline/manual data enrichment occurs.

Format missing variables clearly in generated documents with actionable callouts:
```markdown
[Developer action required: Insert your registered business entity name and address here.]
```

---

## 10. Standard Audit Output Format

Every audit report must adhere to this clean, readable structure:

```markdown
# Privacy & Compliance Audit Report
**Target Application**: [App Name / Repository]
**Audit Date**: [YYYY-MM-DD]
**Inspection Scope**: Codebase, Dependencies, Schemas, Network Calls, Policies

---

## 1. Executive Summary
[Concise 2–3 paragraph synthesis of data practices, transparency posture, and key risks.]

## 2. Data Practice Inventory Summary
| Data Category | Observed Items | Purpose | Storage & Sub-processors | Status |
| :--- | :--- | :--- | :--- | :--- |
| Identity & Auth | Email, Password hash | Login & Account recovery | Postgres, Postmark | ✓ Verified |
| Telemetry & Logs | IP, User-Agent, Error traces | Diagnostics | Sentry, Winston logs | ⚠ Retention undefined |
| AI Integration | Chat queries, Prompt tokens | Text generation | OpenAI API | ❌ Missing User Disclosure |

## 3. Third-Party Sub-Processor Mapping
* **Authentication**: Clerk / Supabase (`@clerk/nextjs`)
* **Payments**: Stripe (`stripe-node`)
* **Analytics**: PostHog (`posthog-js`)
* **Email**: Resend (`resend`)

## 4. Policy ↔ Code Consistency Matrix
* ✓ **Disclosed**: User email and profile collection properly documented in `/privacy`.
* ⚠ **Partial**: PostHog telemetry is active in frontend bundle but omitted from third-party disclosures.
* ❌ **Critical (P0)**: In-app camera permission triggered without in-app disclosure or policy entry.
* ❌ **Phantom Clause (P2)**: Terms of Use contain 4 sections on recurring subscription billing, but application is entirely free.

## 5. Action Plan & Prioritized Findings
### P0 — Critical Remediation
- [ ] Add explicit Privacy Policy disclosure and in-app permission priming for camera usage in `ProfileScanner.vue`.

### P1 — Major Disclosure Updates
- [ ] Add PostHog to Privacy Policy sub-processors list with opt-out documentation.
- [ ] Define and implement database retention/purge routine for orphaned user uploads.

### P2 — Transparency & UX Refinements
- [ ] Remove phantom subscription billing clauses from Terms of Use.
- [ ] Add contextual explanation modal prior to triggering native browser geolocation prompt.

### P3 — Documentation Polish
- [ ] Update support email placeholder in Privacy Policy footer.
```

---

## 11. Safety Rules & Guardrails

The agent **MUST NEVER**:
- Claim to provide legal advice or legal warranties.
- Use marketing buzzwords like "100% GDPR Compliant" or "Fully Legal".
- Invent third-party services, data practices, retention periods, or contact addresses not present in the code or confirmed by the developer.
- Hide or downplay material data exfiltration, tracking, or sensitive permissions.
- Generate generic, one-size-fits-all boilerplate that conflicts with observed application behavior.

---

## 12. Definition of Done

An audit or documentation task is complete when:
- [x] The application was thoroughly inspected prior to drafting.
- [x] All active data practices and third parties are accurately cataloged.
- [x] Generated policies directly reflect active features—and omit phantom features.
- [x] All unconfirmed developer variables are marked with `[Developer action required]`.
- [x] Policy-to-code discrepancies are classified (P0–P3) with actionable fixes.
- [x] Permission UX and consent touchpoints are evaluated for clarity.
- [x] High-risk data patterns and non-legal advice disclaimers are clearly stated.

---

> **The goal is not to generate legal-looking documents. The goal is to ensure that the application's actual behavior, data practices, user expectations, and public disclosures are aligned, transparent, and easy to understand.**
