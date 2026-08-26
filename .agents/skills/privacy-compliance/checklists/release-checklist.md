# Release & Deployment Privacy Checklist

A mandatory pre-deployment verification checklist for product teams, coding agents, and developers before shipping web, mobile, or SaaS applications to production.

---

## 1. Legal Documents & Public Disclosure Readiness

- [ ] **Public Routes & Footer Links**:
  - [ ] `/privacy` route is active, responsive, and contains the generated project-specific Privacy Policy.
  - [ ] `/terms` route is active and contains the code-aligned Terms of Use.
  - [ ] `/cookie-policy` or cookie banner preference manager is accessible.
  - [ ] Footer on all primary landing, signup, and login pages includes direct, working links to these policies.
- [ ] **Placeholder Elimination**:
  - [ ] Search all public legal pages for unpopulated tokens: `[Developer action required]`, `[COMPANY_NAME]`, `[EMAIL]`, `TODO`.
  - [ ] Verify that official company legal name, jurisdiction, and support email are accurately filled.

---

## 2. In-App Trust & Consent Mechanics

- [ ] **Cookie Consent Banner**:
  - [ ] Tested on a clean browser profile (incognito): banner displays properly without obscuring critical UI.
  - [ ] Clicking "Reject Non-Essential" successfully prevents analytics/advertising scripts from loading.
  - [ ] User choice is persisted in an essential cookie / localStorage for subsequent visits.
- [ ] **Permission Priming Dialogs**:
  - [ ] All native/browser sensor permissions (Location, Camera, Microphone, Notifications) are preceded by contextual in-app explanatory modals.
  - [ ] Rejection of permission is handled gracefully with manual input alternatives.
- [ ] **AI & Sensitive Feature Microcopy**:
  - [ ] AI prompt interfaces include subtle disclaimer text noting AI provider usage and limitations.
  - [ ] Public vs. Private visibility toggles on user-generated content are clearly indicated.

---

## 3. Account & Data Deletion Workflows

- [ ] **Self-Serve Account Deletion**:
  - [ ] Account deletion option is accessible in user settings (e.g. `Settings > Account > Delete Account`).
  - [ ] Deletion requires confirmation (e.g. entering password or confirmation phrase).
  - [ ] Deletion endpoint executes cleanly: session cookies revoked, auth tokens destroyed, DB record purged/soft-deleted.
- [ ] **Public Deletion Request Path**:
  - [ ] A dedicated public deletion guide or direct contact email (`privacy@domain.com`) is published for users unable to log in.

---

## 4. Mobile App Store Requirements (If Applicable)

- [ ] **Apple App Store (iOS)**:
  - [ ] **App Privacy Details**: Completed "Privacy Nutrition Labels" in App Store Connect matching code inspection findings.
  - [ ] **In-App Account Deletion (Guideline 5.1.1(v))**: Verified that users can delete their entire account directly inside the iOS app.
  - [ ] **Usage Descriptions**: Verified that all `NS...UsageDescription` strings in `Info.plist` provide clear, user-friendly rationales.
- [ ] **Google Play Store (Android)**:
  - [ ] **Data Safety Section**: Completed Data Safety declarations in Google Play Console.
  - [ ] **Delete Account URL**: Provided public URL in Play Console explaining account deletion procedures.

---

## 5. Security & Infrastructure Hardening

- [ ] **Encryption in Transit**:
  - [ ] Strict HTTPS enforced across all production domains (HSTS headers enabled).
- [ ] **Cookie Flags**:
  - [ ] All session and authentication cookies configured with `HttpOnly`, `Secure`, and `SameSite=Lax` or `SameSite=Strict`.
- [ ] **Sanitized Logging**:
  - [ ] Verified that production logs do not capture raw passwords, authentication bearer tokens, or full credit card numbers.
- [ ] **Backup Overwrite Schedule**:
  - [ ] Verified automated backup lifecycle retention policy is configured in cloud provider (e.g. AWS RDS 30-day snapshot rotation).
