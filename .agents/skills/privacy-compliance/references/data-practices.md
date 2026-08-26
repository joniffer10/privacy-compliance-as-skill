# Data Practices Reference Guide

A technical handbook for inspecting, detecting, and categorizing real application data practices across modern web and mobile stacks (Nuxt, Vue, Next.js, React, Node.js, Express, Fastify, Python, Mobile React Native, iOS, Android).

---

## 1. The Data Lifecycle Model

Every data item handled by an application moves through six discrete lifecycle phases. During an inspection, trace each data item through this lifecycle:

```text
1. Ingestion (Forms, HTTP headers, WebSockets, OS sensors, third-party webhooks)
      ↓
2. Processing (Validation, transformation, tokenization, AI prompt composition)
      ↓
3. Storage (Relational DB, NoSQL, Cache/Redis, S3/Object Storage, LocalStorage)
      ↓
4. Egress / Sharing (Third-party SDKs, API requests, Webhooks, Telemetry sinks)
      ↓
5. Retention (Active lifecycle, archive periods, backup snapshots)
      ↓
6. Purging / Deletion (Hard delete, soft delete anonymization, S3 lifecycle rules)
```

---

## 2. Codebase Inspection Vectors & Code Signatures

### A. Authentication, Credentials & PII
Look for user onboarding, profile modification, and credential management flows:

| Vector | Detection Signals & File Patterns | Common Data Fields |
| :--- | :--- | :--- |
| **User Models / Schemas** | `prisma/schema.prisma`, `models/*.ts`, `schemas/*.sql`, `typeorm`, `mongoose` | `email`, `password_hash`, `first_name`, `last_name`, `phone_number`, `avatar_url`, `billing_address` |
| **Auth Providers** | `next-auth`, `@supabase/supabase-js`, `@clerk/nextjs`, `firebase/auth`, `passport`, `lucia` | Social profile IDs, OAuth access tokens, verified email status, session cookies |
| **Forms & Endpoints** | `POST /api/auth/register`, `POST /api/profile`, `v-model="userForm"` | Direct user-submitted identity data |

```typescript
// Example Prisma Schema Inspection
model User {
  id            String    @id @default(cuid())
  email         String    @unique // PII - Direct Identity
  passwordHash  String    // Credential
  phoneNumber   String?   // PII - Contact
  role          Role      @default(USER)
  createdAt     DateTime  @default(now())
  deletedAt     DateTime? // Indicates soft-deletion pattern
}
```

### B. Hardware, Device Sensors & Native Permissions
Look for hardware API invocations in web frontends or mobile native configurations:

| Hardware Vector | Web API Signatures | Mobile Native Signatures (`Info.plist` / `AndroidManifest.xml`) |
| :--- | :--- | :--- |
| **Geolocation** | `navigator.geolocation.getCurrentPosition`, `watchPosition` | `NSLocationWhenInUseUsageDescription`, `ACCESS_FINE_LOCATION`, `ACCESS_COARSE_LOCATION` |
| **Camera & Video** | `navigator.mediaDevices.getUserMedia({ video: true })` | `NSCameraUsageDescription`, `android.permission.CAMERA` |
| **Microphone & Audio** | `navigator.mediaDevices.getUserMedia({ audio: true })` | `NSMicrophoneUsageDescription`, `android.permission.RECORD_AUDIO` |
| **Contacts & Media** | `navigator.contacts.select` (Web Contacts API) | `NSContactsUsageDescription`, `READ_EXTERNAL_STORAGE`, `READ_MEDIA_IMAGES` |
| **Push Notifications** | `Notification.requestPermission()`, `firebase/messaging` | `UIBackgroundModes` -> `remote-notification`, `POST_NOTIFICATIONS` |

### C. Client-Side Storage & Cookie Persistence
Examine persistent client-side state across cookies, storage engines, and caching:

| Storage Type | Detection Patterns | Privacy Implication |
| :--- | :--- | :--- |
| **Cookies** | `document.cookie`, `useCookie` (Nuxt), `cookies-next`, `res.cookie()` (Express) | Session tokens, tracking IDs, consent flags, CSRF tokens. Must determine `SameSite`, `Secure`, and `HttpOnly` attributes. |
| **Local Storage** | `localStorage.setItem('key', val)`, `pinia-plugin-persistedstate` | Persists indefinitely across browser restarts; must inspect if unencrypted auth tokens or user draft data are stored. |
| **Session Storage** | `sessionStorage.setItem()` | Ephemeral; expires on tab closure. Lower privacy footprint. |
| **IndexedDB** | `idb`, `dexie`, `localforage` | Large-scale offline caching, user-generated drafts, offline databases. |

```javascript
// Example Nuxt 3 Cookie Inspection
const authCookie = useCookie('auth_token', {
  secure: true,
  sameSite: 'lax',
  maxAge: 60 * 60 * 24 * 7 // 7 days retention
});
```

### D. Analytics, Telemetry & Behavioral Tracking
Examine third-party telemetry libraries and script tags:

| Category | Common Libraries & SDKs | Typical Data Exfiltrated |
| :--- | :--- | :--- |
| **Product Analytics** | `posthog-js`, `mixpanel-browser`, `@segment/analytics-next`, `amplitude-js` | User ID, page paths, feature clicks, device metadata, custom user properties. |
| **Web Analytics** | `gtag.js`, `google-analytics-4`, `plausible-tracker`, `fathom-client` | IP address (anonymized or raw), referrers, screen resolution, session duration. |
| **Error / Crash Telemetry**| `@sentry/vue`, `@sentry/node`, `bugsnag`, `logrocket` | Stack traces, breadcrumbs, user IP/ID, HTTP request payloads (may inadvertently leak PII in traces). |
| **Advertising Pixels** | `react-facebook-pixel`, `tiktok-pixel`, `google-tag-manager` | Conversion events, hashed emails, purchase amounts, ad interaction identifiers. |

### E. AI, LLM & Machine Learning Services
Inspect whether user-supplied content, prompts, or files are transferred to external model inference engines:

| Category | Detection Patterns | Key Privacy Questions |
| :--- | :--- | :--- |
| **Direct Model APIs** | `import OpenAI from 'openai'`, `@anthropic-ai/sdk`, `replicate` | Are user prompts sent to OpenAI/Anthropic? Are zero-data retention (ZDR) agreements active? |
| **Embedding Stores** | `pinecone`, `weaviate`, `chromadb`, `pgvector` | Are user documents embedded and stored persistently in vector databases? |
| **Streaming Prompts** | `ai/react` (Vercel AI SDK), `StreamingTextResponse` | Is live user input streamed to external servers? |

```typescript
// Code Inspection: OpenAI Prompt Route
import OpenAI from 'openai';
const openai = new OpenAI({ apiKey: process.env.OPENAI_API_KEY });

export async function POST(req: Request) {
  const { userMessage, userDocument } = await req.json();
  // Data Practice: User-generated content is transferred to OpenAI for inference.
  const response = await openai.chat.completions.create({
    model: 'gpt-4o',
    messages: [{ role: 'user', content: `${userMessage}\n\n${userDocument}` }],
  });
  return Response.json(response.choices[0].message);
}
```

### F. Payments, Subscriptions & E-Commerce
Verify billing integrations and PCI scope boundaries:

| Provider | Detection Signatures | Observed Practice Scope |
| :--- | :--- | :--- |
| **Stripe** | `@stripe/stripe-js`, `stripe` (Node SDK), webhook endpoint `/api/webhooks/stripe` | Tokenized card data (handled in iframe), billing address, customer email, subscription status. |
| **Paddle / LemonSqueezy** | `@lemonsqueezy/lemonsqueezy.js`, `@paddle/paddle-js` | Merchant of Record pattern; tax, invoice address, country, transaction history. |
| **Mobile IAP** | `react-native-iap`, `revenuecat` (`react-native-purchases`) | App Store / Play Store receipt validation, anonymous subscriber IDs. |

### G. Server Logging & Infrastructure Sinks
Inspect logging middlewares and persistent file stores:

| Component | Detection Signatures | Practice Notes |
| :--- | :--- | :--- |
| **Logger Middlewares** | `pino`, `winston`, `morgan('combined')`, `bunyan` | Check if log formats include raw request bodies (`req.body`) or IP addresses (`req.ip`). Look for redaction rules. |
| **Object / Media Storage** | `@aws-sdk/client-s3`, `@google-cloud/storage`, `cloudinary`, `uploadthing` | User uploaded images, PDF documents, avatar files. Check if buckets are public vs. presigned URL access. |
| **Transactional Email/SMS**| `resend`, `@sendgrid/mail`, `postmark`, `twilio` | Target recipient emails, phone numbers, message body content. |

---

## 3. Data Classification Matrix

When logging identified data items into the inventory, classify their sensitivity:

```text
Level 1: Public / Non-Personal Data
  - Aggregated anonymous metrics, static product catalogs, open public content.

Level 2: Standard Personal Data
  - Name, business email, username, general IP address, basic device specs, cookie identifiers.

Level 3: Account & Financial Data
  - Account passwords (hashed), purchase histories, billing addresses, tax identifiers, session tokens.

Level 4: High-Sensitivity / Special Category Data
  - Precise real-time GPS coordinates, biometric signatures, health/medical notes, government IDs,
    children's data, racial/ethnic data, political/religious beliefs, private unencrypted communications.
```

---

## 4. Identifying Undefined Data Practices

When inspecting code, actively flag "gaps" where the codebase initiates data collection but fails to define handling policies:

1. **Undefined Retention**:
   - Database tables that grow infinitely with no cleanup cron, TTL index (MongoDB), or pruning script.
   - Flag as: `⚠ Retention policy not defined in codebase`.
2. **Missing Deletion Cascade**:
   - Deleting a user in `users` table leaves orphaned records in `messages`, `activity_logs`, or S3 object stores.
   - Flag as: `⚠ Deletion does not cascade to auxiliary storage`.
3. **Unsanitized Telemetry**:
   - Error reporters (e.g. Sentry) logging full HTTP error objects containing raw credentials or auth tokens in query params or headers.
   - Flag as: `⚠ Telemetry sink lacks PII sanitization/redaction middleware`.
