# Privacy policy & App Store labels — disclosure gap

Written 2026-08-24. **Read this before selling anything on `trade.anglerbook.fun`, and
arguably before the next App Store submission.**

## Why this exists

The Market Insight product sells aggregate statistics derived from the app's anonymous
telemetry. Its entire commercial claim is *"the only validly collected dataset in fly
fishing"*. That claim rests on the app's privacy disclosures being accurate.

Checking them turned up something larger: **the live policy is already inaccurate about
what the shipped app does, independent of Market Insight.**

## What the shipped app actually contacts

Audited from `AnglerBook-iOS/Sources/` on 2026-08-24 (v1.1.0 R130, live on the App Store):

| Endpoint | What it is |
|---|---|
| `anglerbook-telemetry.glysk.workers.dev` | anonymous session observations → Cloudflare D1 |
| `anglerbook-buddies.glysk.workers.dev` | Fishing Buddies → Cloudflare D1, **not anonymous** |
| `anglerbook-marketplace.glysk.workers.dev` | Marketplace web layer |
| `catalog.anglerbook.fun/catalog/*.json` | gear / species / beats catalogue |
| `maps.apple.com` | Apple Maps |

## What the live policy says

`privacy.html`, currently published at `anglerbook.fun/privacy` and **linked from the
in-app paywall under App Review 3.1.2**:

| Statement | Where | Problem |
|---|---|---|
| "GLYSK OÜ does not run servers that collect or store your fishing data" | intro | GLYSK runs the telemetry and buddies Workers. Both collect and store data derived from fishing. |
| "GLYSK does not receive a copy" | §02 | A coarsened copy of session-derived data is sent to the telemetry Worker. |
| "The only parties involved are … Apple … GitHub" | §07 | **Cloudflare is not named.** It hosts the telemetry and buddies Workers and their D1 databases. |
| §08 "No tracking" | §08 | Accurate as written — it disclaims *third-party* SDKs — but sits beside the claims above and reinforces a "nothing leaves the device" reading. |

Not mentioned anywhere: **telemetry**, **Fishing Buddies**, **Cloudflare**, **Marketplace**.

Fishing Buddies matters most of the four: it is the one **non-anonymous** backend. The
angler's chosen nickname leaves the device and is stored on a GLYSK server. That is a
disclosable data collection and it appears nowhere in the policy.

## Two distinct problems — keep them apart

**1. Accuracy (exists today, unrelated to Market Insight).** The policy describes an app
that keeps everything on-device. The shipped app has three first-party backends. This is
the one to fix first, and it needs fixing whether or not anything is ever sold.

**2. Purpose (created by Market Insight).** Telemetry was disclosed — where it is
disclosed at all — as improving the app. Publishing aggregate output as a commercial
market-intelligence product is a different purpose. Anonymous data still has to be
*described accurately*.

Note the asymmetry: **re-identification is not the issue.** The observations carry no
identity, no IP and no coordinates — a named beat rather than a position, a week rather
than a moment — and the product applies an `n < 5` suppression floor. Coarse aggregate
spec statistics cannot identify anyone. The problem is disclosure, not exposure.

## What needs doing

### privacy.html — add and correct

- New section covering **anonymous telemetry**: what is sent (country, named beat, coarse
  weather and temperature band, catch count, method set, gear strings, duration band, week
  and month, platform, OS, app version, subscription tier), what is *not* (identity, IP,
  coordinates, timestamps), where it is stored (EU-pinned Cloudflare D1), and that
  aggregate output may be published as industry statistics.
- New section covering **Fishing Buddies**: opt-in, the nickname is the only named string
  that leaves the device, what the buddy code is, and how to leave.
- Correct the intro and §02 so they no longer claim GLYSK runs no servers and receives no
  copy.
- Add **Cloudflare** to §07 as a processor, with EU data residency stated.
- Consider naming **Marketplace** if it processes anything.

Do not weaken §08 — "no third-party advertising or analytics SDKs" is true and worth
keeping. Make it precise rather than removing it.

### App Store Connect — privacy labels

Apple requires declaring collected data **even when it is not linked to identity** —
there is a "Data Not Linked to You" category for exactly this. Likely additions:

- **Diagnostics / Usage Data** — not linked to you (telemetry)
- **User Content** or **Identifiers** — as appropriate for the buddies nickname and code
- Confirm the **Coarse Location** treatment for the named-beat assignment, which is
  resolved on-device but is derived from location

The labels and the policy must agree. A mismatch is a common rejection reason and an easy
one to avoid.

### Google Play — Data Safety form

Android is not released yet, so this is cheapest to get right before first submission
rather than after. Same disclosures, Play's own form.

## Sequence

1. Rewrite `privacy.html` — accuracy first, then the commercial-use sentence.
2. Update App Store Connect privacy labels to match.
3. Fill Play Data Safety before Android's first submission.
4. Only then sell a Market Insight subscription.

The versioned B2B documents at `trade.anglerbook.fun/legal/privacy` are **separate** and
already cover the trade-account side. Do not merge the two — different controller
relationship, different lawful basis, different data.

## Related

- Vault `01.01.01.15 - Market Insight — Trade Portal` — the disclosure question stated precisely
- Vault `01.01.01.02 - Backend — Telemetry Architecture` — exactly what the wire carries
- Vault `01.01.01.01 - Backend — Fishing Buddies` — the six privacy rules
- `AnglerBook-Industry_Insight/src/legal.js` — the B2B documents, for tone and structure
