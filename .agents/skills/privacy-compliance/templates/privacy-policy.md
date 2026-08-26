# Privacy Policy Template

> **Instructions for Agent**: Customize this template based strictly on the inspected codebase. Only include sections whose conditional tags (`<!-- IF ... -->`) match confirmed application features. Replace all bracketed tokens `[TOKEN]` or mark them with `[Developer action required]`.

---

# Privacy Policy

**Effective Date:** [EFFECTIVE_DATE, e.g. January 15, 2026]  
**Last Updated:** [LAST_UPDATED_DATE, e.g. January 15, 2026]

This Privacy Policy explains how **[APPLICATION_OR_COMPANY_NAME]** ("we", "us", or "our") collects, uses, processes, and protects your information when you use our website, application, and related services (collectively, the "Service").

---

## 1. Information We Collect

We collect information you provide directly to us, information collected automatically during your use of the Service, and information from third-party services where applicable.

### A. Information You Provide Directly
<!-- IF USER_ACCOUNTS -->
* **Account Information:** When you register for an account, we collect your [COLLECTED_ACCOUNT_FIELDS, e.g. email address, username, hashed password, profile name].
<!-- ENDIF -->
<!-- IF PAYMENTS -->
* **Billing Information:** When you purchase a subscription or product, our third-party payment processor ([PAYMENT_PROCESSOR_NAME, e.g. Stripe, LemonSqueezy]) collects your billing address, payment method details, and transaction history. We do not store full credit card numbers on our servers.
<!-- ENDIF -->
<!-- IF USER_UPLOADS -->
* **User-Generated Content & Files:** We collect files, images, documents, or comments you voluntarily upload or publish through the Service.
<!-- ENDIF -->
* **Communications:** When you contact customer support, we collect your email address, message contents, and any attachments you provide.

### B. Information Collected Automatically
<!-- IF SERVER_LOGS -->
* **Log & Device Data:** Our servers automatically record standard technical log data when you access the Service, including your IP address, browser type and version, operating system, referring URLs, access timestamps, and diagnostic error codes.
<!-- ENDIF -->
<!-- IF COOKIES -->
* **Cookies & Tracking Technologies:** We use cookies and similar local storage technologies to maintain user sessions, remember preferences, and analyze service performance. For details, see our [Cookie Policy](#cookies-and-similar-technologies).
<!-- ENDIF -->
<!-- IF LOCATION_SERVICES -->
* **Location Information:** With your explicit device permission, we collect [PRECISE_OR_COARSE] location data to [LOCATION_PURPOSE, e.g. display localized search results and nearby service providers].
<!-- ENDIF -->

<!-- IF OAUTH_LOGIN -->
### C. Information from Third Parties
* **Third-Party Authentication:** If you choose to log in using third-party identity providers ([OAUTH_PROVIDERS, e.g. Google, GitHub, Apple]), we receive your public profile identifier, name, and verified email address as permitted by your profile settings on those services.
<!-- ENDIF -->

---

## 2. How We Use Your Information

We use the collected information for specific, limited operational purposes:
* **To Provide & Maintain the Service:** Authenticating users, hosting data, processing transactions, and delivering requested features.
* **To Communicate with You:** Sending transactional notifications, security alerts, password resets, and customer support responses.
<!-- IF ANALYTICS -->
* **To Improve & Optimize the Service:** Analyzing aggregate usage trends and troubleshooting technical defects.
<!-- ENDIF -->
* **To Ensure Security & Compliance:** Protecting against unauthorized access, malicious activity, and enforcement of our Terms of Use.

---

## 3. Third-Party Service Providers (Sub-processors)

We share personal information with third-party service providers solely to the extent necessary to deliver the Service. Our current sub-processors include:

| Sub-Processor | Service Category | Purpose & Data Shared | Location / Transfer Basis |
| :--- | :--- | :--- | :--- |
| **[HOSTING_PROVIDER, e.g. AWS / Vercel]** | Cloud Infrastructure | Server hosting and encrypted database storage | [SERVER_REGION, e.g. United States] |
<!-- IF PAYMENTS -->
| **[PAYMENT_PROVIDER, e.g. Stripe]** | Payment Processing | Card processing and recurring subscription management | [REGION, e.g. United States] |
<!-- ENDIF -->
<!-- IF EMAIL_PROVIDER -->
| **[EMAIL_PROVIDER, e.g. Postmark / Resend]** | Transactional Email | Delivery of account confirmation and alert emails | [REGION, e.g. United States] |
<!-- ENDIF -->
<!-- IF ANALYTICS -->
| **[ANALYTICS_PROVIDER, e.g. PostHog / Plausible]** | Product Analytics | Aggregate interaction metrics and page view tracking | [REGION, e.g. EU / US] |
<!-- ENDIF -->
<!-- IF AI_PROCESSING -->
| **[AI_PROVIDER, e.g. OpenAI / Anthropic]** | AI Model Processing | Processing user prompts to generate text completions | [REGION, e.g. United States] |
<!-- ENDIF -->

We do not sell, rent, or trade your personal information to third-party data brokers or advertisers.

---

<!-- IF AI_PROCESSING -->
## 4. Artificial Intelligence & Automated Processing

Certain features of the Service utilize artificial intelligence (AI) models provided by [AI_PROVIDER_NAME].
* **Data Transmission:** When you interact with AI features, your input text or uploaded content is transmitted securely to the AI provider for real-time model inference.
* **Model Training Disclosures:** [Developer action required: State whether customer API data is excluded from model training, e.g. "We utilize enterprise API endpoints where inputs are not used to train future public models."]
* **Accuracy Notice:** AI-generated outputs may occasionally be inaccurate, incomplete, or biased. You should independently verify any critical information generated by AI features.
<!-- ENDIF -->

---

<!-- IF COOKIES -->
## 5. Cookies and Similar Technologies

We use cookies, local storage, and session storage to operate and optimize the Service:
* **Essential Cookies:** Required for secure authentication and core application functionality.
* **Functional Cookies:** Remember your UI preferences (such as dark mode or language choice).
<!-- IF ANALYTICS -->
* **Analytics Cookies:** Measure how users interact with our features to help us improve system performance.
<!-- ENDIF -->

You can manage or disable cookies through your browser settings, though doing so may prevent certain interactive features from functioning properly.
<!-- ENDIF -->

---

## 6. Data Retention

We retain personal information only for as long as necessary to fulfill the operational purposes described in this policy:
<!-- IF USER_ACCOUNTS -->
* **Account Information:** Retained for as long as your account remains active. If you delete your account, your profile data is purged from our production database within [PURGE_TIMELINE, e.g. 30 days].
<!-- ENDIF -->
<!-- IF SERVER_LOGS -->
* **System Logs & Telemetry:** Retained for diagnostic purposes for a rolling period of [LOG_RETENTION_PERIOD, e.g. 30 to 90 days], after which they are automatically deleted or aggregated into anonymous summaries.
<!-- ENDIF -->
* **Encrypted Backups:** Routine disaster recovery database backups are permanently rotated and overwritten every [BACKUP_RETENTION, e.g. 30 days].

---

## 7. Your Rights and Choices

Depending on your jurisdiction, you may have the following rights regarding your personal information:
* **Access & Portability:** Request a copy of the personal information we hold about you in a structured, machine-readable format.
* **Correction:** Request that we correct inaccurate or incomplete profile records.
<!-- IF USER_ACCOUNTS -->
* **Deletion:** Request the deletion of your account and personal data. You can initiate account deletion directly in your application settings under **[SETTINGS_PATH, e.g. Settings > Account > Delete Account]**, or by contacting us.
<!-- ENDIF -->
* **Opt-Out of Analytics:** [Developer action required: Detail analytics opt-out method if applicable].

---

## 8. Data Security

We implement appropriate technical and administrative safeguards designed to protect your personal information against unauthorized access, loss, alteration, or disclosure. These measures include:
* Transport Layer Security (TLS 1.3/HTTPS) encryption for all data in transit.
* Strong cryptographic hashing (e.g. bcrypt/argon2) for stored account passwords.
* Access controls and least-privilege access policies for production databases.

Please note that no method of transmission over the Internet or electronic storage is 100% secure.

---

## 9. Children's Privacy

The Service is not directed to children under the age of [MIN_AGE, e.g. 13 (or 16 in the EEA)], and we do not knowingly collect personal data from children. If you become aware that a child has provided us with personal information without parental consent, please contact us immediately so we can delete such records.

---

## 10. Changes to this Privacy Policy

We may update this Privacy Policy from time to time to reflect changes in our legal obligations or application features. We will notify you of any material changes by updating the "Last Updated" date at the top of this policy and, where appropriate, displaying a prominent notice within the application.

---

## 11. Contact Us

If you have questions, concerns, or requests regarding this Privacy Policy or our data practices, please contact us at:

* **Entity Name:** [Developer action required: Legal entity name]
* **Email:** [Developer action required: Primary privacy/support contact email, e.g. privacy@example.com]
* **Postal Address:** [Developer action required: Official registered mailing address]
