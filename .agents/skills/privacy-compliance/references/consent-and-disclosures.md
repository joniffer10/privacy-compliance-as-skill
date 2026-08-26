# Consent & Contextual Disclosures Guide

A UX and engineering guide for designing transparent permissions, cookie consent mechanisms, and just-in-time contextual notices.

---

## 1. The Two-Layer Permission Model (Priming vs. OS Prompts)

Operating system permission dialogs (iOS, Android, Chrome/Safari browser prompts) cannot be customized beyond a short text string, and once rejected, are difficult for users to recover.

**Best Practice: Pre-Permission Priming Modal**
Always trigger an in-app explanatory UI modal *before* requesting the system-level permission.

```text
[User clicks "Find Nearby Stores"]
              ↓
[In-App Explanatory Priming Sheet / Modal]
"To show stores nearest to you, we need access to your device location. 
We only check your location while using this map."
              ↓
[User clicks "Continue"] ──(triggers native API)──> [OS System Permission Dialog]
              ↓
[User clicks "Not Now"]  ──(graceful fallback)  ──> [Show manual Zip/Postal code input]
```

### UX Anti-Pattern vs. Recommended Pattern

| Vector | ❌ Anti-Pattern (Raw / Surprise Prompt) | ✓ Recommended (Primed Contextual Notice) |
| :--- | :--- | :--- |
| **Location** | Calling `getCurrentPosition()` immediately upon page load without prior user action. | Explaining "Used to calculate shipping distance" on checkout screen before triggering geolocation. |
| **Camera** | Triggering camera permission when entering a profile settings page. | Triggering camera permission only when user taps "Scan Document / Take Photo" button with a brief pre-dialogue. |
| **Microphone** | Requesting microphone access during initial app launch splash screen. | Requesting microphone access only when user activates "Voice Note" recording button. |
| **Push Notifications**| Modal popup on first visit: "example.com wants to send notifications". | Contextual banner after user creates a project: "Get notified when team members comment on your tasks." |

---

## 2. Cookie Consent & Tracking Banners

Cookie banners should be clean, non-coercive, and technically wired to actual storage mechanics.

### Cookie Category Classification

```text
1. Strictly Necessary / Essential (No prior consent required in most frameworks)
   - Session authentication tokens (`auth_token`)
   - CSRF tokens (`csrf_token`)
   - Cookie consent preferences (`cookie_consent`)
   - Shopping cart session IDs

2. Functional / Preference (Consent optional or configurable)
   - UI theme selection (`dark_mode`)
   - Selected language / locale (`user_locale`)

3. Analytics & Performance (Consent required in EU/EEA and opt-out in other regions)
   - PostHog, Google Analytics (`_ga`), Mixpanel

4. Advertising & Retargeting (Explicit affirmative opt-in required in strict jurisdictions)
   - Meta Pixel (`_fbp`), Google Ads remarketing cookies
```

### Technical Implementation Rule: Consent State Gates
Ensure that tracking scripts do *not* fire before consent is granted:

```typescript
// Nuxt / Vue Consent Gating Pattern
function initAnalytics() {
  const consent = useCookie('analytics_consent').value;
  if (consent === 'granted') {
    // Only load external tracking SDK if consent has been explicitly granted
    loadPostHogScript();
  }
}
```

### Avoiding Deceptive UI Patterns (Dark Patterns)
- **Equal Visual Prominence**: The "Reject Non-Essential" button must be just as visible, readable, and accessible as the "Accept All" button. Do not hide rejection behind multiple sub-menus or low-contrast gray text.
- **No Pre-Ticked Checkboxes**: Non-essential cookies or marketing checkboxes must be unchecked by default.
- **No False Walls**: Basic access to public content should not be unconditionally blocked simply for refusing analytics tracking (unless an account is required for the service).

---

## 3. Just-in-Time Disclosures for Sensitive Features

When an application handles sensitive user data, provide localized, immediate explanations right where the action occurs:

### A. AI / LLM Feature Input Disclosures
Place subtle, clear microcopy below AI prompt inputs:
```text
💬 [ Ask AI Assistant...                                       ] [Send]
ℹ AI responses are generated using OpenAI and may occasionally produce inaccurate info. Do not submit sensitive passwords or financial credentials.
```

### B. Account / Data Deletion Warnings
Provide clear, unhurried warnings before destructive operations:
```text
⚠ Permanent Account Deletion
This action is irreversible. All your projects, uploaded files, and personal data will be permanently deleted within 30 days. Active subscriptions will be immediately canceled.
[Cancel]   [Permanently Delete My Account]
```

### C. Public Profile & Sharing Defaults
Ensure users know when content they publish will be publicly visible:
```text
Visibility: 🌐 Public (Anyone with the link can view your uploaded portfolio)
[Change to Private]
```
