# Privacy Audit Checklist

A step-by-step inspection checklist for coding agents and developers to systematically audit an application's codebase for privacy practices, data flows, and security posture.

---

## 1. Project Configuration & Dependency Audit

- [ ] **Dependency Manifests**: Inspect `package.json`, `requirements.txt`, `Podfile`, `build.gradle`, `Cargo.toml`, etc.
  - [ ] Search for analytics/tracking libraries (PostHog, Google Analytics, Mixpanel, Segment, Amplitude).
  - [ ] Search for crash reporters (Sentry, Bugsnag, LogRocket, Datadog).
  - [ ] Search for advertising SDKs (Facebook Pixel, TikTok Pixel, Google Ads).
  - [ ] Search for AI SDKs (`openai`, `@anthropic-ai/sdk`, `langchain`, `replicate`).
  - [ ] Search for payment libraries (`stripe`, `@lemonsqueezy/lemonsqueezy.js`, `paddle-sdk`).
  - [ ] Search for communication providers (`resend`, `@sendgrid/mail`, `postmark`, `twilio`).
- [ ] **Environment Variables**: Inspect `.env.example`, `config/*.ts`, server runtime configurations.
  - [ ] Identify all third-party API keys and external endpoints receiving user data.
  - [ ] Verify that no live production secrets, private keys, or API tokens are hardcoded in source files.

---

## 2. Database Models & Schema Audit

- [ ] **Entity / Schema Inspection**: Review `schema.prisma`, SQL migration scripts, TypeORM/Mongoose entities.
  - [ ] Identify all PII fields (name, email, phone, physical address, birth date, IP addresses).
  - [ ] Identify credential storage methods (verify passwords use bcrypt/argon2 with proper salt rounds).
  - [ ] Identify financial/payment data (verify that raw card numbers or CVVs are never stored locally).
  - [ ] Check for soft-delete flags (`deleted_at`) vs. hard-delete cascades.
  - [ ] Check for automated data retention / TTL indexes (e.g. session expiration, old log pruning).

---

## 3. Server Endpoints & API Route Audit

- [ ] **Authentication & Identity Endpoints**:
  - [ ] Check signup / registration handlers: what fields are mandatory vs. optional? (Enforce data minimization).
  - [ ] Check password reset flows: does password reset leak whether an email exists? (Prevent user enumeration).
  - [ ] Check session termination / logout: are tokens and session cookies properly invalidated server-side?
- [ ] **User Deletion & Export Endpoints**:
  - [ ] Is there an account deletion endpoint (`DELETE /api/user/account` or equivalent)?
  - [ ] Does deletion cascade across all related tables (e.g. comments, uploaded files in S3, payment profiles)?
  - [ ] Is there a data export endpoint (`GET /api/user/export`) providing machine-readable JSON/ZIP?
- [ ] **Server-Side Logging**:
  - [ ] Inspect logging middleware (`pino`, `winston`, `morgan`).
  - [ ] Verify that request body passwords, auth tokens, and raw credit card payloads are redacted from logs.
  - [ ] Verify server log retention windows (e.g. 30–90 days rotation).

---

## 4. Frontend & Client-Side Storage Audit

- [ ] **Cookie Management**:
  - [ ] Inspect `document.cookie`, `useCookie` (Nuxt), `cookies-next`.
  - [ ] Verify cookie security flags: `HttpOnly` (for auth cookies), `Secure` (HTTPS only), `SameSite=Lax/Strict`.
  - [ ] Check if non-essential tracking cookies fire before user consent is granted.
- [ ] **Local & Session Storage**:
  - [ ] Check `localStorage` / `sessionStorage` usage.
  - [ ] Verify that sensitive unencrypted credentials or private tokens are not left in persistent `localStorage`.
- [ ] **External Scripts & CDNs**:
  - [ ] Inspect `index.html`, `app.vue`, `layout.tsx` for third-party `<script>` tags.
  - [ ] Verify Subresource Integrity (SRI) hashes on third-party scripts.

---

## 5. Hardware Permissions & Device Sensors

- [ ] **Web Browser APIs**:
  - [ ] Check for `navigator.geolocation` calls: Is there an explanatory priming UI before invocation?
  - [ ] Check for `navigator.mediaDevices.getUserMedia` (Camera/Mic): Are these triggered only on user action?
- [ ] **Mobile Native Permissions** (`Info.plist`, `AndroidManifest.xml`):
  - [ ] Check `NSCameraUsageDescription`, `NSLocationWhenInUseUsageDescription`, `NSMicrophoneUsageDescription`.
  - [ ] Ensure permission strings contain clear, descriptive explanations of *why* the access is needed.

---

## 6. AI & Third-Party Egress Audit

- [ ] **AI Data Flows**:
  - [ ] What user data is packed into LLM prompts? (Check for inadvertent PII inclusion).
  - [ ] Is user consent or awareness clearly established prior to AI processing?
  - [ ] Are enterprise API endpoints used to ensure inputs are not used for public model training?
- [ ] **Cloud Storage & File Uploads**:
  - [ ] Are uploaded files stored in private S3 buckets accessed via short-lived presigned URLs?
  - [ ] Are uploaded files scanned for malware or stripped of sensitive EXIF metadata (GPS tags in photos)?
