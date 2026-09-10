# Product

<!-- impeccable:product-schema 1 -->

## Platform

web

## Users

Primary (and effectively only) user: one person — the owner — who is actively **trying to conceive** and logs her daily fertility signals as part of a routine. Bloom is a private personal tool, not a multi-tenant product: there is no public audience, no account system, and no marketing surface. Design should optimize for one person's real daily ritual (often first thing in the morning, on a phone, one-handed) rather than acquisition, broad onboarding, or general-population edge cases.

## Product Purpose

Bloom is a personal fertility journal for trying to conceive. The user logs daily signals — basal body temperature (BBT), LH strip result, period flow, cervical mucus, symptoms, intimacy, and notes — and Bloom turns that history into a fertile-window forecast: ovulation prediction, a daily conception-probability view, and a cycle calendar. Success is knowing, on any given day, where she is in her cycle and which days are best to try, with enough confidence to act.

## Positioning

A private, self-owned alternative to commercial fertility apps. Two things a neighboring product could not truthfully copy:

1. **Data she owns.** Entries live in her own browser and sync only through her own JSONBin bin (her API key + passphrase). No accounts, no company servers, no analytics — deletion means gone.
2. **A model tuned to her body.** Cycle day re-anchors off her logged periods (not a fixed calendar), and LH-surge detection is judged **relative to her personal baseline** rather than fixed strip categories — so an elevated baseline is not mistaken for a surge, while a genuine peak is always honored.

## Operating Context

- **Daily morning use:** read/enter BBT, read the LH strip, log flow/symptoms/intimacy, glance at fertile-window status. Speed, legibility, and one-handed phone use matter most.
- **Cross-device:** the same journal is used across her phone(s)/browser(s) via manual push/pull sync (opt-in; her own JSONBin credentials).
- **One-time import:** supports a Natural Cycles `measurements.json` export (fills only days not already logged).
- **Deployment:** static site on GitHub Pages; installable to the home screen; usable offline.

## Capabilities and Constraints

- **Per-day entry fields:** BBT temp; LH strip (none/low/high/peak); period flow (none/spotting/light/medium/heavy); cervical mucus; symptoms (multi-select); intimacy (boolean); notes. New fields are additive and optional — older entries missing a field must still render correctly.
- **Cycle anchoring:** `getCurrentCycleStart()` derives the current cycle's anchor from logged period runs (first day of a contiguous light/medium/heavy run; spotting alone does not start a cycle), falling back to a stored "last period" date only when no period entries exist. Cycle day = today − anchor + 1 (no modulo).
- **Prediction:** ovulation is confirmed by BBT thermal shift or LH peak; the live current-cycle estimate uses baseline-relative LH-surge detection. A logged peak **always** counts as a confirmed surge and is never suppressed by the baseline logic.
- **Storage & sync:** browser `localStorage` (`bloom_entries`, `bloom_settings`); manual push/pull sync to JSONBin.io using the user's own credentials. The entire entries blob syncs, so new fields need no backend change.
- **Architecture (hard constraint of the current build):** a single HTML file (`index.html`) plus `sw.js`, `manifest.json`, and two PNG icons — no framework, no build step, no bundler; deployable as static files. The service-worker `CACHE` version must be bumped whenever an update ships.
- No third-party analytics, accounts, or tracking.

## Brand Commitments

- **Name:** Bloom — "Your personal fertility journal."
- **Privacy-first is binding.** Data stays in the user's control: local by default, opt-in sync only to her own bin. No accounts, servers she doesn't own, or analytics. *(user-confirmed)*
- **No medical claims.** Bloom is a companion journal, not a medical device: warm, non-clinical language; no diagnosis; and no storage or display of bloodwork or numeric hormone values — only the logged strip *categories* are interpreted. *(user-confirmed)*
- **Assets:** two peony PNG app icons (`icon-192.png`, `icon-512.png`).

## Evidence on Hand

- Working incumbent implementation: `index.html`, `sw.js`, `manifest.json`, `icon-192.png`, `icon-512.png`.
- A data backup file, `bloom-2026-03-30.json` — **do not modify.**
- No testimonials, customers, benchmarks, pricing, licensing, or press exist. Future work must not fabricate any — this is a personal, unmarketed tool.

## Product Principles

1. **Her body is the source of truth** — anchor cycle day and predictions on what she actually logged, not on fixed defaults.
2. **Honor real signals, never manufacture certainty** — a logged peak always confirms, but soft/ambiguous readings stay soft; degrade gracefully when data is sparse.
3. **Privacy is the product** — no accounts, no servers she doesn't own, no analytics.
4. **Additive and durable** — new capabilities extend the single-file app without breaking existing data or the sync flow.
5. **Fits the morning ritual** — fast, legible, one-handed, works offline.

## Accessibility & Inclusion

Used in low-light morning conditions on a phone; comfortable one-handed touch targets and strong legibility matter. No formal external standard was established as a requirement.
