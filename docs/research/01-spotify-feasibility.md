# 01 — Spotify feasibility: can a third-party app programme an interleaved radio stream?

Research for ticket `issues/01-spotify-feasibility.md`. **Not legal advice.** Every claim is tagged
VERIFIED (read in a primary source, URL given), INFERRED (my reasoning from those sources),
NO EVIDENCE FOUND, or SECONDARY (non-primary, used only for enforcement-in-practice).

Primary sources read in full for this pass (raw HTML fetched and text-extracted, not summarised by
a third party):

- Spotify Developer Policy, effective 15 May 2025 — https://developer.spotify.com/policy
- Spotify Developer Terms of Service — https://developer.spotify.com/terms
- Spotify Compliance Tips — https://developer.spotify.com/compliance-tips
- Quota modes — https://developer.spotify.com/documentation/web-api/concepts/quota-modes
- Rate limits — https://developer.spotify.com/documentation/web-api/concepts/rate-limits
- iOS SDK landing — https://developer.spotify.com/documentation/ios
- iOS SDK source + public headers — https://github.com/spotify/ios-sdk (cloned, v5.0.1)
- Web Playback SDK — https://developer.spotify.com/documentation/web-api/../web-playback-sdk
  (canonical: https://developer.spotify.com/documentation/web-playback-sdk)
- Blog, 27 Nov 2024 — https://developer.spotify.com/blog/2024-11-27-changes-to-the-web-api
- Blog, 6 Feb 2026 — https://developer.spotify.com/blog/2026-02-06-update-on-developer-access-and-platform-security
- Web API changelogs Feb/Mar/May/Jul 2026 —
  https://developer.spotify.com/documentation/web-api/references/changes/february-2026 (and siblings)

## Verdict up front

| # | Sub-question | Verdict |
|---|---|---|
| 1 | Integration clause — alternating Spotify music with third-party podcast/news audio | **BLOCKED.** Prohibited by the letter of the policy under *three independent clauses* (III.5, III.7, III.4), not one. No carve-out exists for a solo developer. |
| 2 | Reading user data (history, liked, top, saved podcasts) | **PARTIAL, and only at toy scale.** Endpoints exist and are documented. But extended quota mode is organizations-only with a **250k MAU** floor, so a solo developer is permanently capped at **5 authorised users**. Not a shippable product. |
| 3 | Playback control | **PARTIAL (confirmed remote-control only).** iOS SDK requires the Spotify app installed and exposes no audio. Pause / play third-party audio / resume-at-position is *technically* achievable (`seekToPosition:`, `playbackPosition`). But doing it as a programmed sequence is what clause III.7 forbids. |
| 4 | Crossfading / ducking / mixing | **BLOCKED.** No PCM surface exists (confirmed by reading the SDK headers). And even a **hard cut** is prohibited: III.7 bars "segue", and Compliance Tips names "DJ/Mixes" as a disallowed use case. |
| 5 | Offline / caching for the commute | **BLOCKED for a third-party app.** ToS IV.3.b permits caching only of metadata/cover art, or "Conditional Downloads" — and Conditional Downloads are a Spotify-app feature, not a third-party surface. |
| 6 | Attribution / UX requirements | **FEASIBLE but constraining.** Mandatory Spotify Marks attribution, mandatory link-backs, and no playback without cover art + metadata. Plus **monetisation of a Streaming SDA is banned outright** (Policy IV.2), which independently destroys the business model. |

### Overall verdict: **BLOCKED. Not partial — blocked.**

The Spotify-native variant of this concept is dead, and it dies four separate times over:

1. **The mechanic is prohibited.** Interleaving Spotify tracks with non-Spotify audio in one
   programmed session is prohibited by Developer Policy III.5, III.7 and (for a "station" framing)
   III.4. Spotify's own Compliance Tips page names the shape of this app — "DJ/Mixes" — in its
   list of "common app use cases that break our policy, and you should not build".
2. **The business model is prohibited.** Policy IV.2 bans commercialisation of Streaming SDAs.
   Compliance Tips gives a worked example that maps almost exactly onto this app's pricing plan:
   "Charging users $5/month for a home automation app that includes the ability to trigger music".
   A $1–3k/mo MRR bar cannot be met by an app that is contractually barred from charging money.
3. **Access is capped at 5 users.** Since 15 May 2025 extended quota mode is organizations-only
   with a 250k-MAU minimum. A solo developer with no audience cannot ever qualify. Development mode
   is capped at 5 authorised users and Spotify says in terms it "should not be relied on as a
   foundation for building or scaling a business on Spotify."
4. **The offline commute case fails.** No third-party caching of sound recordings.

Any one of these is fatal at the map's $1k/mo floor. (1) and (2) are contractual prohibitions with
no workaround; (3) is an eligibility wall the developer cannot climb; (4) is a product-quality
failure in the developer's own primary use case.

**Per the map's ranking, this does not end the map** — it eliminates the Spotify branch of the
platform-native variant and raises the prior probability that Apple Music / YouTube Music / Tidal
fail the same way, since all four license from the same rightsholders under similar constraints.
