# Security Audit — Lumina

This document summarizes the security review conducted on Lumina, a renewable energy marketplace built with Firebase. The audit was performed in two phases: an initial pass during the .EXECUTE Green Hackathon (Technex 2026), and a follow-up hardening pass afterward.

## Summary

| # | Finding | Severity | Status |
|---|---------|----------|--------|
| 1 | Cross-Site Scripting (XSS) via unsanitized user input | High | Fixed |
| 2 | Missing Content Security Policy (CSP) | Medium | Fixed |
| 3 | Insufficient Session Expiration via cached content (CWE-613) | Medium | Fixed |
| 4 | Firestore database open to public read/write | Critical | Fixed |

---

## 1. Cross-Site Scripting (XSS)

**Issue:** User-supplied fields (e.g. listing titles/descriptions, names) were rendered into the DOM without sanitization, allowing injected HTML/JS to execute in other users' browsers.

**Impact:** An attacker could plant a malicious listing or profile field that executes arbitrary JavaScript for any user viewing it — session/token theft, defacement, or redirect attacks.

**Fix:** User-generated content is now escaped/sanitized before being inserted into the DOM.

---

## 2. Missing Content Security Policy

**Issue:** No CSP header was set, leaving the app without a browser-enforced defense-in-depth layer against script injection.

**Impact:** Even with input sanitization, absence of CSP means any missed injection point has no secondary barrier.

**Fix:** A restrictive CSP was added, allowlisting only required script/style sources.

---

## 3. Insufficient Session Expiration via Cached Content (CWE-613)

**Issue:** The app is a single-page application that swaps views via DOM show/hide, without using `history.pushState`. Firebase Auth state is only checked on initial load via `onAuthStateChanged`. Browsers' back-forward cache (bfcache) restores a frozen DOM snapshot on Back/Forward navigation without re-running this check — resulting in stale UI (e.g. a logged-out user's browser still showing the authenticated dashboard, or a logged-in user briefly seeing the login page).

**Impact:** On a shared/public device, a user who logs out and walks away could have their session state (or a stale authenticated view) exposed to the next person via the Back button.

**Fix:** A `pageshow` event listener detects bfcache restoration (`event.persisted`) and forces a page reload, guaranteeing `onAuthStateChanged` re-evaluates the real session state before any UI is shown.

```javascript
window.addEventListener('pageshow', function (event) {
  if (event.persisted) {
    window.location.reload();
  }
});
```

---

## 4. Firestore Database Open to Public Read/Write

**Issue:** The Firestore security rules were left in default "test mode":

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if true;
    }
  }
}
```

This allowed **any unauthenticated client** to read or write **any document in any collection** — including `users`, `transactions`, `ratings`, `notifications`, `listings`, and `investorInterests`.

**Impact:** Complete database compromise — arbitrary data exfiltration (user emails, transaction history), data tampering, deletion, or privilege escalation (e.g. a user rewriting their own `role` field from `consumer` to `producer`).

**Fix:** Replaced with least-privilege rules scoped per collection:

- **`users`** — readable/writable only by the owning user; `role` field is immutable after creation to prevent privilege escalation.
- **`listings`** — readable by any authenticated user (required for marketplace browsing); writable only by the owning `producerId`.
- **`transactions`** — readable/writable only by the involved `consumerId`/`producerId`; immutable once created (no delete).
- **`ratings`** — readable by any authenticated user; creatable only by the reviewing `consumerId`; immutable after creation.
- **`notifications`** — readable/updatable only by the recipient (`toUserId`); creatable by any authenticated user (since one user notifies another).
- **`investorInterests`** — readable only by the involved `investorId`/`producerId`; creatable only by the investor; immutable after creation.

Full ruleset is maintained in the Firebase console and mirrors the logic above.

---

## Methodology

This audit combined manual code review of client-side logic (DOM rendering, auth flow, routing) with inspection of the live Firebase configuration (Firestore rules, Authentication settings). No automated scanning tools were used; all findings were identified through direct analysis of application behavior and backend configuration.

## Disclosure

This was a self-conducted audit on a personal/academic project (not a bug bounty submission). Findings and fixes are documented here for portfolio and learning purposes.
