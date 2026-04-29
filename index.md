# PawTask Privacy Policy

**Privacy Policy** · [Support](/pawtask-legal/support/)

**English** · [繁體中文](/pawtask-legal/zh/)

**Last updated:** April 28, 2026

## 1. Introduction

PawTask is a parent-and-child habit-formation app developed by **Chen Chao Yu** ("we", "us", "our"). The parent uses an iPhone to set their child's school location, home location, and meal schedule; the child wears an Apple Watch (typically configured via Apple Family Setup) and earns rewards on a virtual pet by checking in to school, eating meals on time, and maintaining good sleep and handwashing habits.

We built PawTask with a privacy-first architecture:

- Sensitive location data is **end-to-end encrypted** so even our servers cannot read it.
- Health data such as handwashing events and sleep records is **processed entirely on your child's wrist** and never leaves the device.

This policy explains exactly what we collect, what we deliberately do not collect, and how we protect what we do collect.

## 2. Data We Collect

### 2.1 Account Information

- **Email address** — When a parent signs in using *Sign in with Apple*, Apple shares the parent's email address (or a private relay address chosen by the parent) so we can create the parent account.
- **User ID** — We store the unique identifier Apple provides for *Sign in with Apple* so the same parent stays signed in across sessions and devices.

### 2.2 Child Device Information

- **Device UUID** — A random unique identifier generated on the child's watch the first time it is paired with the parent's account.
- **Cryptographic public key** — Each watch generates an X25519 keypair on first launch. The public key is stored on our server so the parent app can encrypt sensitive data to that specific watch (see Section 4). The private key never leaves the watch.

### 2.3 Schedule and Settings

- Meal times (breakfast, lunch, dinner)
- School arrive and leave times
- Sleep window threshold
- Handwashing duration target and meal duration target
- Per-child PvP feature toggle

These settings are stored as plaintext on our server because, on their own, they are not personally sensitive.

### 2.4 Location Data — End-to-End Encrypted

- **School and home coordinates** — When the parent picks an address on the map, the latitude and longitude are encrypted on the parent's iPhone using the child's public key (X25519 Sealed Box / NaCl) **before** being uploaded. Only the child's watch holds the matching private key and can decrypt these coordinates. **Our servers store only ciphertext and cannot read the actual coordinates.**
- **Check-in verification — performed on the watch** — School and home check-in verification happens entirely on the watch. The watch fetches the encrypted school/home coordinates from our server, decrypts them locally with its own private key, and compares them on-device to its current GPS reading. Only the resulting check-in event (timestamp and pass/fail) is sent to our server. **The raw GPS coordinates measured at check-in time never leave the watch, and our servers never decrypt location data.**

### 2.5 App Activity

- Check-in timestamps (school, home, meal start, meal end)
- Meal duration and a handwashing flag (true/false, derived on-device from HealthKit)
- Pet level, XP, stat values, and stat changes
- PvP match outcomes — which child played, when, the round-by-round moves, and the result

This data is associated with a child's device ID and powers the in-app pet progression and the parent's dashboard view.

### 2.6 Health Data — Stays on the Device

- **HealthKit handwashing events and sleep records** — These are read directly from Apple's HealthKit on the child's watch. They are processed locally to compute pet XP rewards. **The raw HealthKit values are never transmitted off the device.** Only the resulting XP changes (no longer health data on their own) are sent to our server.

This design follows Apple's HealthKit data-use policy, which forbids sending HealthKit-derived data to remote servers.

## 3. Data We Do Not Collect

- **Advertising identifiers (IDFA)** — We do not request *App Tracking Transparency* permission and we do not access cross-app identifiers.
- **Browsing or search history** — We do not track web behavior.
- **Contacts, photos, voice recordings, or messages** — We do not access these.
- **Sensitive categories** — We do not collect race, ethnicity, sexual orientation, pregnancy data, disability information, religious or philosophical beliefs, trade union membership, political opinions, genetic data, or biometric identifiers (fingerprints, face scans, voice prints).
- **Third-party analytics or advertising SDKs** — PawTask contains no advertising libraries, no third-party analytics SDKs (Mixpanel, Amplitude, Segment, etc.), no Facebook / TikTok / Google Ads pixels, and no data broker integrations.
- **Payment information** — PawTask is free and contains no in-app purchases.

## 4. How We Use Data

| Data | Purpose |
|---|---|
| Email and User ID | Parent account authentication; staying signed in across sessions |
| Device UUID and public key | Pair the child's watch to the parent account; route encrypted settings to the correct watch |
| Schedule and settings | Display the day's plan on the watch; trigger pet animations and reminders |
| Encrypted location data | Stored as ciphertext and relayed to the watch, which decrypts and verifies check-ins on-device |
| App activity | Drive pet progression, parent dashboard, PvP matchmaking |
| HealthKit (on-device only) | Compute pet XP rewards locally on the watch; only the XP delta is transmitted |

We do **not** use any of this data for advertising, marketing, profiling, or analytics shared with third parties.

## 5. How We Protect Data

- **End-to-end encryption** — School and home location data is encrypted using NaCl Sealed Box (X25519 + XSalsa20 + Poly1305) on the parent's iPhone before upload, and only the child's watch can decrypt it. Server-side ciphertext cannot be read by us, by our hosting providers, or by any unauthorized party. The raw GPS coordinates measured at check-in time stay on the watch and are never transmitted to our servers.
- **HealthKit isolation** — Per Apple's HealthKit policy, raw handwashing and sleep data is read only by the watch app and is never sent to any remote service.
- **Encryption in transit** — All requests between your devices and our servers use TLS 1.3.
- **Short-lived authentication** — Parent JWTs are short-lived and refreshed via a rotating refresh-token flow. Watch tokens are revoked immediately if the parent unpairs the watch.
- **Minimal storage** — We retain only what is needed for the app to function. We never log decrypted location values.

## 6. Service Providers

PawTask uses the following infrastructure providers. Each receives only what is necessary to operate the service.

- **Apple Inc.** — Sign in with Apple, push notifications, App Store distribution. Apple's privacy practices: <https://www.apple.com/legal/privacy/>
- **Google Cloud (Cloud Run)** — Hosts our backend API. Cloud Run never sees decrypted location data because all such data is encrypted client-side.
- **Neon (PostgreSQL)** — Provides our managed database. The database stores ciphertext for sensitive fields and plaintext for non-sensitive metadata (timestamps, schedule).

We do not sell, rent, or share your data with advertisers, data brokers, or analytics companies.

## 7. Children's Privacy

PawTask is designed to be used by parents and children together:

- The **parent** is the account holder. By installing the parent iPhone app and signing in with their own Apple ID, the parent grants consent for the child's watch to use the app under the family account.
- The **child** typically wears an Apple Watch configured via Apple Family Setup, where Apple's parental controls (screen time limits, communication limits, App Store approval) govern what the watch can do.
- We collect only what is necessary to deliver the app's functionality. We do not knowingly collect personal data about a child beyond what the parent has configured.
- We comply with Apple's App Store Kids guidelines, the U.S. Children's Online Privacy Protection Act (COPPA) where applicable, and EU GDPR Article 8 for children under 16.

A parent may, at any time, request that we delete all data tied to their account or to a specific child (see Section 9).

## 8. Data Retention

| Data | Retention |
|---|---|
| Active account and child data | Retained while the account is in use |
| Account and all associated child data | Deleted within 30 days of a parent's deletion request |
| Backend access logs | Retained up to 30 days, then automatically rotated |
| Crash diagnostics (Apple-collected, user opt-in) | Per Apple's policies; we do not maintain a separate copy |

## 9. Your Rights

You may, at any time, contact us at the email below to:

- Request a copy of the data we hold for your account or any of your children
- Correct inaccurate data
- Delete your account and all associated child data
- Withdraw a previously granted consent (for example, revoke a paired watch)

For users in the European Union, United Kingdom, California, Virginia, Colorado, or other jurisdictions with similar privacy laws, the rights above are also rights under GDPR, UK GDPR, CCPA / CPRA, and equivalent statutes. We will honor them within the timeframes those laws require.

## 10. International Data Transfers

Our servers may be located in the United States, the European Union, or other regions depending on infrastructure availability. By using PawTask, you consent to your data being processed in the region where our servers operate. We use industry-standard encryption (TLS in transit; Sealed Box for sensitive fields) regardless of region.

## 11. Changes to This Policy

When we make material changes, we will update the "Last updated" date at the top of this policy and, where required by law, notify users via the app or by email. Continued use of PawTask after changes take effect constitutes acceptance of the revised policy.

## 12. Contact

For questions, requests, or complaints about this privacy policy or your data, contact us:

**Email:** **dayou99.dev@gmail.com**

---

*This policy is provided in English and Traditional Chinese. In the event of any conflict between the two versions, the English version controls.*
