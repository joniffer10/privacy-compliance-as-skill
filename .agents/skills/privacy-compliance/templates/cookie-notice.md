# Cookie Notice & Policy Template

> **Instructions for Agent**: Customize this template based on cookies and local storage mechanisms detected in the application. Categorize cookies strictly by observed behavior.

---

# Cookie Policy

**Effective Date:** [EFFECTIVE_DATE, e.g. January 15, 2026]  
**Last Updated:** [LAST_UPDATED_DATE, e.g. January 15, 2026]

This Cookie Policy explains how **[APPLICATION_OR_COMPANY_NAME]** ("we", "us", or "our") uses cookies, local storage, and similar web tracking technologies on our website and application.

---

## 1. What Are Cookies?

Cookies are small text files placed on your device by websites that you visit. They are widely used to make websites function efficiently, remember user preferences, and provide diagnostic and analytics data to website owners.

---

## 2. Categories of Cookies We Use

We classify cookies and storage technologies into the following categories based on their operational purpose:

### A. Strictly Necessary Cookies (Essential)
These cookies are required for the basic technical operation and security of the Service. They cannot be switched off in our systems.
* **Purpose:** User authentication, session management, CSRF security tokens, load balancing.

### B. Functional & Preference Cookies
These cookies enable enhanced functionality and personalization, such as remembering your chosen language, theme (dark/light mode), or saved UI configurations.
* **Purpose:** User interface customization and convenience.

<!-- IF ANALYTICS -->
### C. Analytics & Performance Cookies
These cookies help us understand how visitors interact with the Service by collecting and reporting aggregate information anonymously.
* **Purpose:** Page view counts, bounce rates, feature adoption metrics, performance diagnostics.
<!-- ENDIF -->

<!-- IF ADVERTISING -->
### D. Marketing & Advertising Cookies
These cookies may be set through our site by our advertising partners to build a profile of your interests and show you relevant advertisements on other websites.
* **Purpose:** Campaign tracking, conversion measurement, behavioral advertising.
<!-- ENDIF -->

---

## 3. Inventory of Stored Cookies & Identifiers

| Cookie / Key Name | Provider | Category | Purpose | Retention / Expiry |
| :--- | :--- | :--- | :--- | :--- |
| `auth_token` / `session_id` | [APPLICATION_NAME] | Strictly Necessary | Maintains user login session | Session / [EXPIRY, e.g. 7 days] |
| `csrf_token` | [APPLICATION_NAME] | Strictly Necessary | Prevents Cross-Site Request Forgery attacks | Session |
| `theme_preference` | [APPLICATION_NAME] | Functional | Remembers user dark/light UI mode choice | [EXPIRY, e.g. 1 year] |
<!-- IF ANALYTICS -->
| `ph_*` / `_ga` | [PostHog / Google] | Analytics | Generates aggregate usage metrics | [EXPIRY, e.g. 1 year] |
<!-- ENDIF -->

---

## 4. How to Manage and Disable Cookies

You have the right to decide whether to accept or reject non-essential cookies.
* **Browser Settings:** You can configure your browser to reject all cookies, accept only certain cookies, or notify you when a cookie is set. Consult your browser's Help menu (e.g., Chrome, Safari, Firefox, Edge) for specific instructions.
* **Consequences of Disabling:** If you choose to reject strictly necessary cookies, certain essential features of our Service (such as logging into your account) may become unavailable.

---

## 5. Contact Us

If you have questions regarding our use of cookies, please contact:
* **Email:** [Developer action required: Insert support/privacy email]

---

# Cookie Banner UI Copy Snippets (For Frontend Implementation)

### Option 1: Clean Minimalist Banner (Recommended)
```text
🍪 We use essential cookies to make our application work, and optional analytics to help us improve. 
[ Manage Preferences ]  [ Reject Non-Essential ]  [ Accept All ]
```

### Option 2: Strictly Essential Only Banner (Zero-Analytics Apps)
```text
🍪 We only use strictly necessary cookies to keep you securely logged in. No tracking or marketing cookies are used.
[ Dismiss ]
```
