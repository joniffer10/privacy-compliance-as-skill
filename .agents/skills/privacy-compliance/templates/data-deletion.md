# Account & Data Deletion Guide Template

> **Instructions for Agent**: Customize this document to provide clear, public-facing instructions for how users can delete their accounts and personal data. This satisfies Apple App Store guideline 5.1.1(v), Google Play Data Safety requirements, and general privacy transparency.

---

# Account & Data Deletion Policy and Instructions

**Application Name:** [APPLICATION_NAME]  
**Last Updated:** [LAST_UPDATED_DATE, e.g. January 15, 2026]

At **[APPLICATION_OR_COMPANY_NAME]**, we respect your right to control your personal data. This document outlines the step-by-step procedure to delete your account, what data is erased, what data is retained for legal or operational reasons, and the timelines involved.

---

## 1. How to Delete Your Account

### Method 1: In-App Self-Service Deletion (Recommended)
You can permanently delete your account directly inside the application:
1. Log in to your account on [APPLICATION_NAME].
2. Navigate to **[SETTINGS_PATH, e.g. Settings > Account Security > Danger Zone]**.
3. Click or tap **"Delete Account"**.
4. Confirm your decision by entering your password or typing the requested confirmation phrase.
5. Your account is immediately deactivated and queued for complete permanent erasure.

### Method 2: Deletion Request via Email / Support
If you cannot access your account or prefer to submit a manual deletion request:
1. Send an email from the email address associated with your account to **[DELETION_EMAIL, e.g. privacy@example.com]**.
2. Include the subject line: `Account Deletion Request - [Your Registered Email]`.
3. Our support team will verify your identity and process the deletion within [RESPONSE_WINDOW, e.g. 14 to 30 days].

---

## 2. What Happens When You Delete Your Account

When an account deletion request is processed:

### Data That Is Permanently Erased
* **Profile & Identity Records:** Name, username, email address, password hash, avatar images.
* **User-Generated Content:** Uploaded files, private documents, project files, and saved settings.
* **Authentication Sessions:** All active login tokens, session cookies, and OAuth authorizations are revoked.

### Data That May Be Temporarily Retained or Anonymized
* **Financial & Transaction Records:** Invoices and purchase records may be retained for [TAX_RETENTION_PERIOD, e.g. 7 years] to comply with mandatory tax, accounting, and anti-fraud regulations. These records will be disassociated from your active user profile.
* **Encrypted Disaster Recovery Backups:** Data in routine disaster recovery backups is overwritten according to our automated backup lifecycle ([BACKUP_PURGE_WINDOW, e.g. within 30 days]).
* **Aggregated Metrics:** Fully anonymized, de-identified telemetry that cannot be linked back to you may be retained for macro product analytics.

---

## 3. Data Deletion Timelines

| Stage | Timeline |
| :--- | :--- |
| **Immediate Deactivation** | Instant (Account login blocked immediately) |
| **Production Database Purge** | Completed within [PURGE_WINDOW, e.g. 7 to 30 days] |
| **Backup Rotation Purge** | Overwritten automatically within [BACKUP_WINDOW, e.g. 30 days] |

---

## 4. Third-Party Sub-Processor Deletion

Upon account deletion, our automated systems signal deletion cascades to our third-party infrastructure:
* **Transactional Email ([EMAIL_PROVIDER]):** Suppressed or deleted from recipient databases.
* **Product Analytics ([ANALYTICS_PROVIDER]):** User profile identifiers are disassociated or scrubbed.
* **Payment Processor ([PAYMENT_PROVIDER]):** Active subscriptions are canceled immediately; payment records are archived in compliance with PCI and tax rules.

---

## 5. Contact Information

For any questions or difficulties regarding data deletion, contact:
* **Privacy Officer / Support Team:** [Developer action required: Insert support email]
