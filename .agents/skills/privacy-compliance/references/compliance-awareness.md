# Compliance Awareness & High-Risk Vectors Reference

A practical guide for software developers and coding agents to understand data protection frameworks, identify high-risk technical features, and recognize when formal legal counsel is essential.

---

## 1. Important Notice: Non-Legal Advice Boundary

> [!IMPORTANT]
> This framework is an **engineering and product transparency tool**. It assists developers in auditing data flows, minimizing unnecessary data collection, and preparing technical drafts. It is **not** formal legal advice and does **not** grant official regulatory compliance certifications.

When interacting with developers or reviewing architectures:
- Never declare an application "100% GDPR-compliant" or "bulletproof against lawsuits".
- Recommend formal legal review whenever high-risk data practices or complex multi-jurisdictional commerce are involved.

---

## 2. High-Risk Technical Feature Matrix

When inspecting an application, immediately raise an elevated risk alert if any of the following technical features are detected:

| High-Risk Feature | Code Indicators & Signatures | Regulatory & Privacy Considerations | Action Required |
| :--- | :--- | :--- | :--- |
| **Children's Information** | Age fields $< 13/16$, child-oriented gaming UI, school/education apps. | US COPPA, UK Age Appropriate Design Code, GDPR-K. Strict parental consent mandates. | ⚠ Require explicit parental verification flow; prohibit behavioral tracking ad SDKs. |
| **Health, Fitness & Medical** | Health metrics, symptom logs, menstrual tracking, medical records, EHR APIs. | HIPAA (US healthcare entities), GDPR Special Category Data (Art. 9). | ⚠ Enforce end-to-end encryption at rest; audit BAA agreements with cloud providers. |
| **Biometrics & Facial Analysis** | Face detection, fingerprint matching, voiceprint authentication SDKs. | Illinois BIPA, GDPR Art. 9, Texas CBO. Massive statutory penalties for unconsented capture. | ⚠ Require standalone written biometric disclosure and explicit affirmative consent. |
| **Precise Background Geolocation** | Continuous location updates, background GPS tracking (`watchPosition` with high accuracy). | GDPR, CCPA/CPRA, Apple iOS Location guidelines. High scrutiny in App Store reviews. | ⚠ Audit if coarse (city-level) location suffices; implement explicit runtime consent priming. |
| **Financial & Credit Data** | Raw card numbers, bank account numbers, credit score inquiries. | PCI-DSS, GLBA, GDPR. High security audit liability. | ⚠ Ensure raw card data never touches application servers; use tokenized iframes (Stripe Elements). |
| **Automated Decision-Making / AI Profiling** | Credit scoring algorithms, automated employment candidate screening, automated denial of access. | EU AI Act, GDPR Art. 22 (Right to human intervention in automated decisions). | ⚠ Provide human-in-the-loop review mechanism and explainability disclosures. |

---

## 3. Major Regulatory Frameworks: Engineering Perspective

### A. European Union & UK (GDPR / UK-GDPR)
* **Core Principles**: Lawfulness, fairness, transparency, purpose limitation, data minimization, accuracy, storage limitation, integrity/confidentiality.
* **Key Technical Requirements**:
  - Lawful basis for processing (e.g. Contractual necessity for core app, Consent for marketing/analytics).
  - Ability to fulfill Subject Access Requests (SAR) and right to erasure (account deletion within 30 days).
  - Cookie consent prior to loading non-essential trackers.
  - Data Protection Impact Assessment (DPIA) required for high-risk automated processing.

### B. United States (State Privacy Laws: CCPA/CPRA, VCDPA, CPA, etc.)
* **Core Principles**: Consumer transparency, right to know, right to delete, right to opt-out of the "sale" or "sharing" of personal data for cross-context behavioral advertising.
* **Key Technical Requirements**:
  - Clear "Do Not Sell or Share My Personal Information" mechanism if third-party ad pixels or tracking brokers are used.
  - Recognition of Global Privacy Control (GPC) signals in HTTP headers or browser events.

### C. Mobile App Stores (Apple App Store & Google Play)
* **Apple App Privacy Labels ("Nutrition Labels")**:
  - Developers must declare every data type collected and whether it is linked to the user's identity or used for tracking across other apps.
* **Apple Account Deletion Rule (Guideline 5.1.1(v))**:
  - If the app supports account creation, it **must** offer account deletion directly within the mobile app, not just a link to a website or an email address.
* **Google Play Data Safety Section**:
  - Requires public disclosure of data collection, encryption in transit, and whether users can request data deletion.

---

## 4. International Data Transfers

When applications host infrastructure in regions different from their user base (e.g., EU users with database hosted in AWS US-East-1):
* **Technical Reality**: Disclose that data will be processed and stored in the specific hosting regions (e.g. United States).
* **Safeguards**: Rely on standard contractual clauses (SCCs) or the EU-US Data Privacy Framework where certified.

---

## 5. When to Recommend Formal Legal Review

Prompt the developer to seek professional legal counsel when:
1. The app processes data from minors under 16 years of age.
2. The business conducts cross-border sales across multiple continents with physical deliveries or localized tax responsibilities.
3. The platform processes sensitive categories: medical, biometric, financial credit, or legal records.
4. The product deploys autonomous AI agents making consequential real-world financial, legal, or health decisions.
5. The application engages in third-party data brokering, ad-tech retargeting, or data monetization.
