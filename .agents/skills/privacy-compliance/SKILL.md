---
name: privacy-compliance
description: >-
  Production-grade, engineering-first privacy and compliance auditing skill. Inspects
  real application codebases, architecture, dependencies, schemas, storage, data flows,
  SDKs, network egress, permissions, consent, retention and deletion behavior; maps
  findings to privacy engineering frameworks; generates behavior-accurate disclosures;
  and verifies policy-to-code consistency. Never assumes undocumented behavior is true.
---

# Privacy & Compliance Skill

## 1. Purpose

This skill performs an engineering-first privacy and compliance assessment of an application.

Its core principle is:

> **The codebase and deployed architecture are the primary evidence of actual technical behavior. Public privacy disclosures must describe that behavior accurately, while legal conclusions must remain explicitly outside the skill's authority.**

The skill is designed to:

1. Discover the application's architecture and processing surfaces.
2. Trace personal-data ingress, storage, transformation, access, and egress.
3. Identify processors, sub-processors, SDKs, analytics, advertising, AI, payment, email, logging, and infrastructure providers.
4. Evaluate privacy-by-design controls.
5. Identify gaps in consent, minimization, purpose limitation, retention, deletion, access, and transparency.
6. Generate behavior-accurate privacy documentation.
7. Compare existing documentation against observed implementation.
8. Produce evidence-backed findings with severity, confidence, and remediation.
9. Never represent a technical audit as legal advice or a certification.

---

# 2. Governing Principles

Every execution MUST follow these principles.

### 2.1 Evidence over assumption

Never claim that a behavior exists merely because:

- a dependency is installed;
- a framework supports a capability;
- a provider is commonly used for a purpose;
- a privacy policy says it happens;
- an environment variable exists without evidence of use.

Classify evidence as:

- **Confirmed** — directly observed in code, configuration, schema, infrastructure, or documentation.
- **Probable** — strongly indicated but not directly verified.
- **Possible** — technically plausible but insufficiently evidenced.
- **Unknown** — cannot be determined from the available repository.

Only **Confirmed** evidence may be presented as established application behavior.

### 2.2 Code is not the entire system

Static analysis cannot prove runtime behavior, infrastructure configuration, provider-side processing, organizational practices, or legal compliance.

Explicitly distinguish:

- source-code evidence;
- configuration evidence;
- deployment/infrastructure evidence;
- documentation evidence;
- provider documentation;
- developer-provided operational facts;
- unresolved assumptions.

### 2.3 Minimize collection

If a data field is collected but not required for a documented purpose, flag it for review.

Prefer:

```text
full_date_of_birth
```

over:

```text
age_verified = true
```

when only an age threshold is required.

### 2.4 Purpose limitation

Data collected for one purpose must not silently become available for unrelated purposes.

Examples:

- authentication data → marketing;
- support messages → advertising;
- location → analytics;
- AI prompts → model training;
- payment identifiers → unrelated profiling.

### 2.5 Lifecycle accountability

For each material data category, determine:

```text
Collection
   ↓
Processing
   ↓
Storage
   ↓
Access
   ↓
Sharing / Egress
   ↓
Retention
   ↓
Deletion
```

An unknown lifecycle stage is a finding, not permission to invent an answer.

### 2.6 Privacy claims must be behavior-accurate

Never generate claims such as:

- "We collect no personal information."
- "Your data is never shared."
- "Your data is permanently deleted."
- "We do not track you."

unless the available evidence supports the claim.

---

# 3. Standards & Frameworks

Use the following as engineering reference frameworks, not as automatic legal-compliance certifications:

1. **NIST Privacy Framework**
   - Identify-P
   - Govern-P
   - Control-P
   - Communicate-P
   - Protect-P

2. **ISO/IEC 27701**
   - Privacy Information Management System concepts
   - Controller/processor responsibilities
   - Privacy governance and lifecycle controls

3. **W3C Data Privacy Vocabulary (DPV)**
   - Data categories
   - Processing purposes
   - Processing operations
   - Recipients
   - Legal-basis vocabulary where applicable

4. **W3C Global Privacy Control**
   - `Sec-GPC: 1`
   - detection, propagation, and enforcement behavior

5. **OWASP Privacy Risks**
   - excessive data collection;
   - insufficient protection;
   - unauthorized disclosure;
   - tracking;
   - identification;
   - exclusion;
   - non-transparent processing;
   - policy/code inconsistencies.

6. Relevant jurisdiction-specific requirements may be identified as **potential regulatory areas**, but the skill MUST NOT declare legal compliance without qualified legal review.

---

# 4. Severity Model

| Severity | Meaning | Typical Example |
|---|---|---|
| **P0** | Critical privacy/security exposure or material undisclosed high-risk processing | Sensitive personal data sent to an external service without an apparent control or disclosure |
| **P1** | Major privacy lifecycle or disclosure gap | Active analytics/sub-processor omitted from privacy documentation |
| **P2** | Material transparency, UX, minimization, or consistency issue | Excessive collection, missing retention explanation, misleading permission UX |
| **P3** | Documentation or engineering hygiene issue | Broken privacy contact link, weak terminology, incomplete metadata |

Severity MUST be based on observed impact, not merely the presence of a technology.

---

# 5. Confidence Model

Every finding SHOULD include:

| Confidence | Meaning |
|---|---|
| **High** | Directly confirmed through source/config/schema/runtime evidence |
| **Medium** | Strong technical indication with limited missing evidence |
| **Low** | Plausible but insufficient evidence |

Example:

```text
[P1][High] PostHog receives user identifiers
Evidence:
  components/analytics.ts:42
  package.json: dependency
  app.vue: analytics initialization
```

Never convert a low-confidence observation into a factual legal claim.

---

# 6. Required Execution Workflow

Execute these phases in order.

```text
1. Establish Audit Scope
        ↓
2. Discover Repository & Architecture
        ↓
3. Inventory Dependencies & External Services
        ↓
4. Map Data Ingress
        ↓
5. Trace Data Flow & Storage
        ↓
6. Trace Network Egress & Processors
        ↓
7. Audit Authentication, Sessions & Identifiers
        ↓
8. Audit Client Storage, Cookies & Tracking
        ↓
9. Audit Permissions & Consent UX
        ↓
10. Audit Retention & Deletion
        ↓
11. Audit Data Subject Rights
        ↓
12. Audit AI / LLM Processing
        ↓
13. Audit Logging, Secrets & Observability
        ↓
14. Evaluate Privacy Risks
        ↓
15. Compare Policy ↔ Implementation
        ↓
16. Generate Remediation
        ↓
17. Generate Documentation
        ↓
18. Produce Evidence-Backed Final Report
```

Do not generate a final privacy policy before completing the relevant discovery and data-flow phases.

---

# 7. Phase 1 — Establish Audit Scope

Determine:

- repository root;
- application type;
- web/mobile/desktop/backend;
- monorepo vs single application;
- environments;
- relevant packages/apps;
- available infrastructure configuration;
- available legal/privacy documents;
- audit date;
- whether the audit is source-only or includes deployment/runtime evidence.

Record exclusions explicitly.

Example:

```text
Audit Scope:
- Source repository: confirmed
- Production infrastructure: not available
- Database production contents: not inspected
- Provider dashboards: not available
- Runtime network capture: not performed
```

---

# 8. Phase 2 — Repository & Architecture Discovery

Inspect, when present:

```text
package.json
pnpm-lock.yaml
yarn.lock
package-lock.json

Cargo.toml
go.mod
requirements.txt
pyproject.toml

Podfile
build.gradle
build.gradle.kts
pubspec.yaml

Dockerfile
docker-compose.yml
compose.yml

.env.example
.env.template
config/*
```

Identify:

- frontend framework;
- backend framework;
- API architecture;
- database;
- ORM;
- authentication provider;
- storage provider;
- hosting;
- CDN;
- analytics;
- observability;
- AI providers;
- payments;
- email/SMS;
- feature flags;
- consent management;
- cookie management.

Framework-specific patterns MUST be inspected rather than relying only on generic search.

---

# 9. Phase 3 — Dependency & External Service Inventory

Inspect dependencies and actual imports/usages.

Classify services into:

```text
Authentication
Database
Cloud Infrastructure
Object Storage
Analytics
Advertising
Crash Reporting
Logging
Monitoring
Email
SMS
Payments
Subscriptions
AI / LLM
Maps / Location
Social Login
CDN
Search
Customer Support
Feature Flags
A/B Testing
Session Replay
Security
Consent Management
```

Important rule:

> A dependency is not automatically a processor.

A provider should be included in the confirmed processor inventory only when there is evidence that the application actually invokes or uses it for relevant processing.

---

# 10. Phase 4 — Personal Data Ingress Detection

Search forms, APIs, route handlers, schemas, controllers, validators, OAuth callbacks, upload handlers, and mobile forms.

### Identity

```text
name
first_name
last_name
username
email
phone
mobile
address
postal_code
date_of_birth
birthdate
age
nationality
government_id
tax_id
passport
```

### Authentication & Security

```text
password
password_hash
bcrypt
argon2
reset_token
verification_token
session_id
refresh_token
access_token
mfa
otp
security_question
```

### Location & Device

```text
latitude
longitude
geolocation
location
ip
ip_address
device_id
advertising_id
idfa
aaid
mac_address
user_agent
```

### Financial

```text
credit_card
card_number
cvc
cvv
iban
routing_number
bank_account
billing_address
stripe_customer_id
paypal_id
```

### Sensitive / High-Risk

```text
health
medical
diagnosis
medication
biometric
fingerprint
face
voice
race
religion
political
sexual_orientation
genetic
children
minor
```

### User Content

```text
message
prompt
comment
post
upload
photo
video
audio
document
```

The scanner MUST avoid treating variable names alone as proof of actual collection. Trace usage to determine whether the field is actually received, stored, transmitted, or merely defined.

---

# 11. Phase 5 — Data Flow & Storage Analysis

Map each material data category to:

```text
Source
→ Validation
→ Transformation
→ Memory
→ Database
→ Cache
→ File/Object Storage
→ Client Storage
→ External API
→ Logs
→ Backups
→ Deletion
```

Inspect:

### Server storage

```text
PostgreSQL
MySQL
SQLite
MongoDB
DynamoDB
Redis
Supabase
Firebase
Prisma
Drizzle
Mongoose
TypeORM
SQL migrations
```

### Client storage

```text
document.cookie
localStorage
sessionStorage
IndexedDB
AsyncStorage
SecureStore
Keychain
SharedPreferences
```

For every storage location, determine whether the data is:

- persistent;
- session-bound;
- encrypted;
- user-accessible;
- server-accessible;
- automatically expired;
- explicitly deleted.

Do not claim encryption unless implementation/configuration evidence supports it.

---

# 12. Phase 6 — Network Egress & Sub-Processor Analysis

Inspect:

```text
fetch()
axios
XMLHttpRequest
WebSocket
GraphQL clients
SDK initialization
server-to-server clients
webhooks
queue producers
email dispatch
analytics dispatch
AI SDK calls
payment SDK calls
```

Search for:

```text
PostHog
Mixpanel
Segment
Amplitude
Google Analytics
Plausible
Sentry
Bugsnag
LogRocket
Datadog
OpenAI
Anthropic
Replicate
Hugging Face
LangChain
Resend
SendGrid
Postmark
Twilio
Mailchimp
Stripe
Paddle
Lemon Squeezy
PayPal
RevenueCat
Firebase
Supabase
AWS
Google Cloud
Azure
Cloudflare
```

This list is a discovery aid, not an exhaustive provider list.

For each confirmed egress:

```text
Recipient
Data transmitted
Purpose
Trigger
Frequency
Authentication
Environment
Retention known?
User consent required?
Documented?
```

---

# 13. Phase 7 — Authentication & Identifier Privacy

Audit:

- account identifiers;
- email verification;
- password handling;
- session cookies;
- access/refresh tokens;
- OAuth;
- social login;
- MFA;
- password-reset flows;
- account enumeration;
- user identifiers exposed in URLs;
- client-visible database IDs;
- analytics identifiers;
- advertising identifiers.

Flag:

- credentials in logs;
- tokens in URLs;
- sensitive tokens in localStorage without justification;
- unnecessary persistent identifiers;
- user IDs exposed to third parties;
- account deletion that does not invalidate sessions.

---

# 14. Phase 8 — Cookies, Client Storage & Tracking

Inspect:

```text
document.cookie
Set-Cookie
localStorage
sessionStorage
IndexedDB
tracking pixels
iframes
SDK initialization
```

Classify cookies/storage as applicable:

```text
Strictly Necessary
Functional
Analytics
Advertising
Personalization
Security
Unknown
```

For each tracking mechanism determine:

```text
What is stored?
Why?
Who can read it?
How long?
Before or after consent?
Can it be disabled?
Does GPC affect it?
```

Do not call a cookie "necessary" solely because the developer labels it necessary.

---

# 15. Phase 9 — Consent & Permission UX

Audit:

### Web

- consent banner;
- opt-in/opt-out behavior;
- analytics activation timing;
- cookie preference persistence;
- withdrawal;
- GPC;
- DNT where relevant;
- consent state propagation.

### Native permissions

```text
Camera
Microphone
Location
Contacts
Photos
Bluetooth
Notifications
Motion / Health
Calendar
Files
```

Verify:

```text
Explanation UI
    ↓
User action
    ↓
Native OS permission
    ↓
Feature access
```

Flag direct permission requests without appropriate explanatory UX where applicable.

Do not state that a pre-permission prompt is legally required unless jurisdiction-specific legal evidence supports that conclusion.

---

# 16. Phase 10 — Retention & Deletion

For each material data category determine:

```text
Creation
Retention basis
Automatic expiration
User deletion
Administrative deletion
Database deletion
Cache deletion
Object deletion
Search/index deletion
Backup treatment
Third-party deletion
Session invalidation
```

Distinguish:

```text
Soft deletion:
deleted_at = NOW()

Hard deletion:
physical removal from active storage
```

Soft deletion alone MUST NOT be described as complete erasure.

A deletion workflow should be traced across dependencies:

```text
User
 ↓
Primary DB
 ↓
Cache
 ↓
Object Storage
 ↓
Search Index
 ↓
Analytics / Processor
 ↓
Backups
```

If any stage is unknown, report it as an unresolved lifecycle question.

---

# 17. Phase 11 — Data Subject Rights

Where applicable, identify technical support for:

```text
Access
Correction
Deletion
Restriction
Objection
Portability
Consent withdrawal
Marketing opt-out
Automated-decision review
Account closure
```

Inspect whether the application has:

- self-service settings;
- export endpoint;
- deletion endpoint;
- privacy request endpoint;
- identity verification;
- request status tracking;
- administrative tooling.

Do not promise a right or legal deadline unless supported by the applicable legal framework and qualified review.

---

# 18. Phase 12 — AI / LLM Privacy Audit

If AI is present, inspect:

```text
Prompt construction
System prompts
User prompts
Uploaded files
Conversation history
Embeddings
Vector databases
RAG documents
Model provider
API requests
Response storage
Training/feedback mechanisms
Moderation providers
Logging
Telemetry
Caching
```

Determine whether user content leaves the application.

For each AI provider record:

```text
Provider
Model
Data transmitted
Purpose
Storage behavior known?
Training behavior known?
Retention known?
User control
Disclosure status
```

Never infer provider retention or training practices from memory. Mark them **Unknown** unless provider documentation or configuration evidence confirms them.

---

# 19. Phase 13 — Logging, Monitoring & Secrets

Audit:

```text
console.log
console.error
logger.info
logger.debug
logger.warn
Sentry
Datadog
CloudWatch
Pino
Winston
OpenTelemetry
```

Search for sensitive values:

```text
password
token
authorization
cookie
session
email
phone
address
prompt
request.body
request.headers
```

Recommend structured redaction/masking.

Examples:

```text
email → j***@example.com
token → [REDACTED]
password → [REDACTED]
authorization → [REDACTED]
```

Also inspect:

- `.env` files;
- committed secrets;
- API keys;
- private keys;
- service credentials;
- tokens embedded in frontend bundles.

A secret exposed in source control or a client bundle may be a security issue as well as a privacy concern.

---

# 20. High-Risk Data Escalation

Immediately elevate findings involving:

- children/minors;
- health information;
- biometrics;
- precise/background location;
- financial credentials;
- government identifiers;
- authentication secrets;
- large-scale profiling;
- sensitive user content;
- highly personalized advertising;
- cross-context tracking;
- AI transmission of sensitive information.

Do not automatically label these legally non-compliant. Label them as **high-risk processing requiring deeper review**.

---

# 21. International Transfers

Identify evidence of:

- external providers;
- cloud regions;
- international APIs;
- CDN routing;
- data residency settings;
- cross-border support tooling.

Record:

```text
Destination
Provider
Data
Evidence
Residency known?
Transfer mechanism known?
```

If residency or transfer mechanisms cannot be established, mark them **Unknown**.

---

# 22. Policy ↔ Code Consistency Audit

Compare actual implementation against:

```text
Privacy Policy
Terms of Use
Cookie Policy
Cookie Banner
Consent UI
AI Disclosure
App Store privacy labels
Account deletion documentation
Support/privacy pages
```

Classify each claim:

```text
✓ Confirmed
⚠ Partial
❌ Contradicted
? Unverified
```

Examples:

```text
✓ Email collection disclosed and confirmed.

⚠ Analytics disclosed but one active SDK is missing.

❌ Policy states "we do not share personal data" while confirmed
   external API egress transmits user identifiers.

❌ Terms contain subscription clauses while no subscription
   functionality exists.

? Retention is stated as 24 months but no implementation evidence
  confirms the period.
```

---

# 23. Phantom Clause Detection

Explicitly search documentation for functionality that does not exist.

Examples:

```text
Subscriptions
Refunds
Premium tiers
Advertising
Location services
AI features
User-generated content
Cookies
Account registration
Community moderation
Data exports
Biometric authentication
```

A phantom clause is not automatically harmless. Misleading documentation should be reported.

---

# 24. Document Generation Decision Matrix

```text
Accounts / Personal Data?
├─ YES → Privacy Policy
└─ NO
   ├─ Analytics / Cookies / Third Parties?
   │  ├─ YES → Privacy / Cookie Notice
   │  └─ NO → Minimal Privacy Statement

Accounts / Paid Features / Interactive Service?
├─ YES → Terms of Use
└─ NO → Terms optional depending on product

Non-essential Cookies / Tracking?
├─ YES → Cookie Notice / Policy
└─ NO → Cookie policy may be unnecessary

Persistent User Data / Accounts?
├─ YES → Account & Deletion Instructions
└─ NO → Omit unless otherwise needed

AI Processing?
├─ YES → AI Feature / Transparency Disclosure
└─ NO → Omit

Children / Sensitive Data / Regulated Processing?
├─ YES → Escalate for jurisdiction-specific legal review
└─ NO → Continue standard engineering audit
```

Do not generate redundant documents.

---

# 25. Missing Operational Variables

Do not ask broad questionnaires.

First derive everything possible from the repository.

Only ask for missing operational facts such as:

```text
1. Legal entity name and jurisdiction
2. Privacy/support contact
3. Intended retention policy
4. Production data residency
5. Business-specific processing purposes
```

Limit follow-up questions to approximately **3–5 high-value variables**.

Use placeholders when necessary:

```markdown
[Developer Action Required: Insert legal entity name]
[Developer Action Required: Insert privacy contact email]
[Developer Action Required: Confirm production data residency]
```

Never fabricate legal entities, addresses, retention periods, jurisdictions, or provider agreements.

---

# 26. Remediation Rules

Recommendations should be actionable and prioritized.

Examples:

### Data minimization

```text
Remove unused PII field from request schema and database model.
```

### Logging

```text
Redact credentials and personal data before structured logging.
```

### Deletion

```text
Implement DELETE /api/user/me and cascade through active storage.
```

### Tracking

```text
Initialize non-essential analytics only after the applicable consent state permits it.
```

### GPC

```text
Read Sec-GPC and propagate the user's applicable privacy preference
through analytics/tracking initialization.
```

### AI

```text
Minimize prompt payloads and avoid transmitting unnecessary identifiers
to external model providers.
```

Never provide code that claims to solve a legal requirement unless the technical mechanism is actually sufficient for that requirement.

---

# 27. Evidence Requirements

Every significant finding SHOULD contain:

```text
Severity
Confidence
Finding
Impact
Evidence
Affected files
Observed behavior
Expected behavior
Recommended remediation
Verification method
```

Example:

```markdown
### P1 — High Confidence — Undisclosed Analytics Egress

**Finding:** Client analytics sends an authenticated user identifier.

**Evidence:**
- `src/plugins/analytics.ts:42`
- `package.json`
- `src/app.vue:18`

**Observed behavior:**
Analytics initializes after application startup and receives the user ID.

**Impact:**
The current privacy documentation does not identify this recipient or processing.

**Remediation:**
Document the processor and transmitted identifier, or remove the identifier from analytics.

**Verification:**
Re-run static egress analysis and inspect the resulting network payload.
```

---

# 28. False Positive Control

Before creating a finding, check:

1. Is the dependency actually imported?
2. Is the feature actually initialized?
3. Is the data field actually populated?
4. Does the value reach storage or egress?
5. Is the code dead/unreachable?
6. Is it development-only?
7. Is it test/mock code?
8. Is it behind a disabled feature flag?
9. Is it only present in documentation?
10. Is there contrary evidence?

If uncertain, downgrade confidence rather than overstating the finding.

---

# 29. Audit Diff / Regression Mode

When a previous audit is available, compare:

```text
New data collection
Removed data collection
New processors
Removed processors
New SDKs
Changed retention
Changed permissions
New tracking
Changed AI providers
New deletion paths
Policy changes
```

Classify:

```text
NEW
RESOLVED
REGRESSED
UNCHANGED
UNKNOWN
```

This enables privacy review as an ongoing engineering process rather than a one-time document-generation task.

---

# 30. Machine-Readable Findings

When useful, produce a structured representation:

```yaml
finding:
  id: PRIV-001
  severity: P1
  confidence: high
  category: third_party_processing
  status: open
  title: Undisclosed analytics recipient
  data:
    - user_identifier
  recipient: Example Analytics
  evidence:
    - src/analytics.ts:42
  remediation:
    - disclose processor
    - minimize identifier
  verification:
    - rerun_egress_scan
```

Do not expose fabricated values merely to satisfy a schema.

---

# 31. Standard Audit Output

Every completed audit MUST use this structure:

```markdown
# Privacy & Compliance Architectural Audit Report

**Target Repository:** [Repository]
**Audit Date:** [YYYY-MM-DD]
**Inspection Scope:** [Scope]
**Inspection Method:** Static analysis / configuration / documentation / runtime evidence
**Standards Referenced:** NIST Privacy Framework, ISO/IEC 27701, W3C DPV, OWASP Privacy Risks

---

## 1. Executive Summary

[Concise evidence-backed summary.]

## 2. Audit Scope & Limitations

[What was and was not inspected.]

## 3. Architecture & Processing Overview

[Application architecture and major processing surfaces.]

## 4. Data Practice Inventory

| Data Category | Purpose | Source | Storage | Recipients | Retention | Deletion | Confidence | Status |
|---|---|---|---|---|---|---|---|---|

## 5. Third-Party / Sub-Processor Inventory

| Provider | Function | Data | Trigger | Evidence | Disclosure |
|---|---|---|---|---|---|

## 6. Privacy UX & Consent

[Consent, permissions, cookies, GPC, withdrawal.]

## 7. Authentication & Identifier Review

[Sessions, identifiers, account lifecycle.]

## 8. Retention & Deletion Review

[Lifecycle evidence and gaps.]

## 9. AI / LLM Privacy Review

[Only if applicable.]

## 10. Policy ↔ Code Consistency

[Confirmed, partial, contradicted, unverified.]

## 11. Findings

### P0 — Critical
[...]

### P1 — Major
[...]

### P2 — Material
[...]

### P3 — Polish
[...]

## 12. Prioritized Remediation Plan

[Engineering actions ordered by risk.]

## 13. Developer Action Required

[Only unresolved operational variables.]

## 14. Verification Plan

[How fixes can be re-audited.]

## 15. Legal / Compliance Boundary

[Explicit statement that this is an engineering assessment and not legal advice.]
```

---

# 32. Policy Generation Rules

Generated privacy documents MUST:

- describe only observed or explicitly confirmed behavior;
- identify uncertainty where material;
- avoid invented providers;
- avoid invented retention periods;
- avoid invented legal bases;
- avoid invented jurisdictions;
- avoid absolute privacy claims;
- avoid phantom functionality;
- use placeholders for missing business facts;
- clearly separate technical facts from legal interpretation.

Never generate:

```text
100% GDPR Compliant
Fully Legally Compliant
Certified Privacy Safe
Legally Waterproof
Guaranteed Compliant
```

unless the statement is explicitly quoted from an authorized external certification and appropriately attributed.

---

# 33. Legal Boundary

This skill provides:

- software engineering analysis;
- architecture review;
- privacy-by-design recommendations;
- technical data mapping;
- transparency/documentation assistance.

It does **not** provide:

- legal advice;
- legal representation;
- regulatory certification;
- guaranteed compliance;
- attorney-client advice.

Where high-risk processing or jurisdiction-specific requirements are involved, recommend review by qualified privacy/legal counsel before publication or deployment.

---

# 34. Definition of Done

An audit is complete only when:

- [ ] Repository architecture was inspected.
- [ ] Relevant dependencies were inventoried.
- [ ] Actual imports/usages were distinguished from unused dependencies.
- [ ] Personal-data ingress points were analyzed.
- [ ] Storage locations were mapped.
- [ ] Network egress was analyzed.
- [ ] Third-party processors were evidence-backed.
- [ ] Authentication/session identifiers were reviewed.
- [ ] Cookies/client storage/tracking were reviewed.
- [ ] Consent and permission UX was reviewed where applicable.
- [ ] Retention and deletion behavior was reviewed.
- [ ] Data-subject-rights mechanisms were reviewed where applicable.
- [ ] AI/LLM processing was reviewed where applicable.
- [ ] Logging and secret exposure were reviewed.
- [ ] High-risk processing was escalated.
- [ ] International-transfer evidence was reviewed where applicable.
- [ ] Existing policies were compared against observed behavior.
- [ ] Phantom clauses were identified.
- [ ] Findings have severity and confidence.
- [ ] Significant findings include evidence.
- [ ] Remediation is actionable.
- [ ] Unknown operational variables are explicitly flagged.
- [ ] No unsupported legal/compliance claim was made.
- [ ] Final output includes limitations and legal boundary.

---

# 35. Core Axiom

```text
                 SYSTEM BEHAVIOR
                       │
                       ▼
              Evidence & Data Flow
                       │
          ┌────────────┴────────────┐
          ▼                         ▼
   Privacy Controls          External Processing
          │                         │
          └────────────┬────────────┘
                       ▼
                Risk Evaluation
                       │
                       ▼
              Policy Generation
                       │
                       ▼
              Policy ↔ Code Audit
                       │
                       ▼
             Remediation & Retest
```

> **Never write the privacy policy first and retrofit the code to it. Inspect the system first, establish evidence, identify uncertainty, then produce documentation that accurately describes reality.**
