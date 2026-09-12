+++
title = "Privacy Policy"
date = "2026-09-12"
+++

**Version 2 — last updated September 12, 2026.** This replaces the previous version of this policy. Kita Browser added an optional cross-device sync feature ("Kita Sync"), which is the main thing that changed — see the [changelog](#changelog) at the bottom for the full list.

## The short version

- Kita Browser works entirely on your device by default. Nothing leaves your browser unless you connect an integration or turn on Sync yourself.
- The AniList integration talks directly to AniList from your browser using your own AniList account — we never see that data.
- Kita Sync is **opt-in and off by default**. If you turn it on, your tracked videos/tags and your email address are stored in a database we run on Supabase so your data can follow you across devices.
- You can delete your synced data or your whole account at any time, and we automatically clear out inactive accounts too.
- We don't run analytics, ads, or tracking scripts anywhere in the extension.

---

## Data stored locally on your device

By default, everything Kita Browser tracks — your saved videos, tags, tag relationships, auto-tag rules, series mappings, and settings — is stored **only in your browser's local storage/IndexedDB**. It never leaves your device unless you explicitly enable one of the two things below.

We do not have access to this data, cannot see it, and cannot recover it for you if you clear your browser data — it's entirely under your control.

## AniList integration

Kita Browser can optionally connect to your [AniList](https://anilist.co) account to search titles and push your watch progress there.

- **Kitamersion is not affiliated with AniList.** This is an unofficial, best-effort integration.
- Sign-in uses AniList's own OAuth flow (via the browser's `identity` API) — you authorize directly with AniList, and AniList issues a token straight to your browser. We never see your AniList password.
- The resulting access token is stored locally in your browser and used only to talk to AniList's API (`graphql.anilist.co`) directly from your device — title searches and progress updates go straight from your browser to AniList, not through any server we operate.
- Disconnecting the integration (or uninstalling the extension) deletes this token from your device.

## Kita Sync (optional, off by default)

Kita Sync lets your videos, tags, tag relationships, and auto-tag rules follow you across devices/browsers. It is entirely opt-in — nothing about it activates unless you sign up for it from **Settings → Sync**.

### What we collect if you turn it on

- **Your email address and password**, to create an account. Authentication is handled by Supabase Auth — we never see or store your password ourselves; Supabase hashes and manages it.
- **The same data described above** (videos, tags, video-tag relationships, auto-tag rules) — synced to a database so it's available on your other devices.

We don't collect anything beyond that. No browsing history outside the sites Kita Browser already tracks, no device fingerprinting, no analytics tied to your account.

### Where it's stored

Synced data is stored in a Postgres database hosted by [Supabase](https://supabase.com), which acts as our infrastructure provider for this feature. Every table enforces row-level security keyed to your account, so — enforced by the database itself, not just app logic — you can only ever read or write your own rows.

### Storage limits and retention

This is a small hobby project running on Supabase's free tier, so there are real limits:

- Synced storage is currently capped at **2MB per account**.
- If an account is inactive for **90 days**, its synced data is automatically wiped (your login stays intact).
- If an account is inactive for **1 year**, the account itself is deleted entirely.
- When you delete something, it's kept as a "tombstone" for **30 days** (so the deletion can propagate to your other devices) before being permanently purged.

### Your controls

From **Settings → Sync** and **Settings → Danger Zone**, at any time and without contacting us, you can:

- Pause Sync (stop it running in the background without deleting anything).
- Manually clear expired tombstones.
- Delete all of your synced data while keeping your account.
- Delete your account entirely, which deletes your synced data and your login.

### The email confirmation page

When you sign up, Supabase emails you a confirmation link that points to `kitamersion.com/auth/confirm/`. That page is a static, client-side-only file with no backend — it reads the confirmation token straight out of the URL (which browsers never send to any server) and hands it back to the extension. We have no server-side log of this token or this step.

---

## Permissions this extension requests

| Permission | Why |
|---|---|
| `storage` / `unlimitedStorage` | Store your tracked videos, tags, and settings locally on your device. |
| `identity` | Run the AniList sign-in flow described above. |
| `alarms` | Schedule the periodic background check that runs Kita Sync when it's enabled. |
| Access to youtube.com / crunchyroll.com | Detect and track videos you watch on those sites — this is the extension's core function. |
| Access to `kitamersion.com/auth/confirm/*` | Relay the Sync email-confirmation token described above back into the extension. |

## What we don't do

We don't run analytics, advertising, or tracking scripts of any kind in the extension. We don't sell or share your data with any third party other than Supabase, which exists solely to host the optional Sync database described above.

## Your rights and choices

- Uninstalling the extension removes all locally stored data.
- If you've used Kita Sync, you can delete your synced data or account at any time from Danger Zone — no need to ask us.
- For anything else (data questions, requests, bugs), open an issue on the [GitHub repository](https://github.com/kitamersion).

## Children's privacy

Kita Browser is not directed at children under 13, and we don't knowingly collect information from them.

## Changes to this policy

We'll update the version number and date at the top of this page whenever something material changes, and summarize what changed in the changelog below.

## Changelog

- **Version 2** (September 12, 2026): Added the Kita Sync section (opt-in cross-device sync via Supabase, including what's collected, storage limits, retention, and account deletion). Clarified exactly what the AniList integration sends and where. Added an explicit list of extension permissions and why each is needed.
- **Version 1** (October 2, 2025): Initial policy — all data local-only, no collection, no tracking.
