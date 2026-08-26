# Policy ↔ Code Consistency Checklist

A cross-verification matrix to rigorously compare the application's actual technical implementation against its public disclosures, Privacy Policy, Terms of Use, and Cookie Notices.

---

## 1. Discrepancy Severity Classification Matrix

| Level | Severity | Definition | Example Finding | Action Required |
| :--- | :--- | :--- | :--- | :--- |
| **P0** | **Critical** | Undeclared high-risk data collection; false deletion guarantees; deceptive security claims. | App transmits GPS coordinates to third-party ad tracker with zero disclosure in policy. | Immediate code fix or immediate policy emergency revision before deployment. |
| **P1** | **Major** | Active third-party sub-processors missing from policy; undefined core data retention; missing account deletion flow. | PostHog analytics or Stripe payments active in app, but omitted from sub-processor table. | Update public policy draft; implement missing deletion endpoint. |
| **P2** | **Moderate** | Phantom legal clauses describing non-existent features; unprimed OS permission prompts; vague contact details. | Terms of Use discuss subscription refunds on an entirely free app; raw OS location prompt without explanation. | Clean up phantom clauses in terms; add UX permission priming sheet. |
| **P3** | **Minor** | Typographical inconsistencies; outdated version dates; missing direct support links. | Missing direct mailto link on privacy contact section. | Polish documentation and links. |

---

## 2. Step-by-Step Consistency Verification

### A. Data Collection Disclosures vs. Database & Forms
- [ ] **Email & Identity**:
  - *Code*: Signup collects email and password.
  - *Policy*: Declares email and credential collection.
  - *Check*: Does the policy claim to collect phone numbers, physical addresses, or full names when the code does not? (Remove phantom fields).
- [ ] **Device & IP Data**:
  - *Code*: Server logs store client IP addresses and user agents.
  - *Policy*: Explicitly discloses automated collection of log and diagnostic data.
- [ ] **Sensitive Hardware**:
  - *Code*: Camera or GPS APIs used in frontend components.
  - *Policy*: Explicitly discloses hardware sensor access and states the operational purpose.

### B. Third-Party Sub-Processors vs. Active Dependencies
- [ ] **Analytics**:
  - *Code*: PostHog / Google Analytics initialized in client bundle.
  - *Policy*: Lists PostHog / Google Analytics under third-party sub-processors.
- [ ] **Payment Providers**:
  - *Code*: Stripe / LemonSqueezy SDKs integrated for checkout.
  - *Policy & Terms*: Lists payment processor; outlines recurring billing if subscriptions exist.
- [ ] **Transactional Communications**:
  - *Code*: Resend / Postmark / Twilio API keys present in backend.
  - *Policy*: Discloses transactional email/SMS service providers.
- [ ] **AI & LLM Services**:
  - *Code*: OpenAI / Anthropic API calls present in server routes.
  - *Policy & Disclosures*: Discloses AI model provider, prompt data handling, and accuracy limitations.

### C. Cookies & Storage vs. Client Implementation
- [ ] **Cookie Table Accuracy**:
  - *Code*: Cookies set (`auth_token`, `theme_pref`, `_ga`).
  - *Cookie Notice*: All active cookies correctly categorized into Essential, Functional, or Analytics.
- [ ] **Consent Gating**:
  - *Code*: Non-essential analytics scripts do NOT fire until consent is granted.
  - *Cookie Policy*: Claims users can opt out or control cookie preferences.

### D. User Rights & Deletion Mechanisms vs. Backend Endpoints
- [ ] **Account Deletion Reality**:
  - *Policy*: Claims "You can delete your account and personal data at any time."
  - *Code*: Verify that an actual working deletion endpoint or UI button exists in the application.
  - *Consistency Flag*: If no deletion route exists, either implement the endpoint or adjust policy to state: "To request deletion, contact support at [email]".
- [ ] **Retention Schedules**:
  - *Policy*: States retention period (e.g. "Deleted within 30 days").
  - *Code*: Verify if database cron or S3 lifecycle rule actually purges or anonymizes records within that window.

### E. Terms of Use vs. Real Features (Phantom Clause Elimination)
- [ ] **Subscriptions & Payments**:
  - *Terms*: Contains terms for recurring billing, cancellation, and refund policies.
  - *Code*: Does the app actually charge users? If app is 100% free, **delete the payment clauses**.
- [ ] **User-Generated Content**:
  - *Terms*: Contains clauses granting a content license for user posts and media.
  - *Code*: Can users actually create or upload content? If read-only, **delete the UGC clauses**.
- [ ] **API Access**:
  - *Terms*: Contains API rate-limiting rules and developer restrictions.
  - *Code*: Is there a public API? If not, **delete the API terms**.
