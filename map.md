# Map: Personalised radio app — go/no-go

Label: `wayfinder:map`

## Destination

A **go/no-go decision** the developer will act on: build / build-narrower / don't build the
personalised-radio app, with the reasoning, the surviving concept variant (if any), and the
explicit MRR arithmetic behind it. If the verdict is build, the leanest MVP scope and the single
riskiest assumption to test next ride along as a conditional appendix — not as deliverables in
their own right. **The decision is the deliverable — not a spec, not code.**

## The concept under validation

An app (platform TBD, iOS first) that programmes a personalised radio station:

1. User logs in via a music streaming service.
2. App reads listening history, liked songs, followed/liked podcasts.
3. It programmes a continuous radio-style stream: podcast episode → pause at a suitable moment →
   one or more liked songs → back to the episode.
4. Optionally mixes in freely broadcastable news segments.
5. Inserts AI-generated intros/outros to feel like a real radio show.

## Notes

- **Domain**: consumer audio apps; music streaming developer platforms; podcast ecosystem; music
  licensing; indie subscription apps.
- **Skills every session should consult**: `/grilling`, `/domain-modeling`. Research tickets use
  the `/research` subagent.
- **Success bar**: **$1k/mo MRR is the floor** that makes it worth building; **$3k/mo is the
  target**. Note that the prior project's arithmetic concluded even $2k was roughly a 1-in-11
  outcome for a solo developer with no audience — that base rate must be re-run at this lower bar,
  not assumed away.
- **Standing preferences for this effort** (carried over verbatim from
  `podcasts-notes-validation`, at the developer's explicit request):
  - The developer asked for **rigour, not encouragement**. Counter-validate. Do not inherit the
    brief's framing where it asserts premises that haven't been established.
  - **Distinguish verified from inferred** in every answer. Where evidence is absent, say
    NO EVIDENCE FOUND rather than producing a plausible-sounding number.
  - Developer is **solo with no audience**: no newsletter, no social following, no existing app
    with users. All funnel arithmetic must assume cold start.
  - Legal compliance is a **constraint to discover, not to route around**. The question is what is
    permissible under actual developer terms, not what is technically achievable.
- **Developer is in the target segment.** They would use this themselves, primarily **while
  commuting**. That is a genuine advantage over the prior project — but it is a sample of one and
  cannot substitute for demand evidence.
- **Music source is negotiable but ranked.** Platform-native music (Spotify/Apple Music/YouTube
  Music/Tidal) is the preferred variant. If that path is blocked, **licensed/royalty-free
  catalogues** and **the user's own local library** are acceptable variants, not consolation
  prizes. A blocked-on-all-platforms finding therefore does **not** end the map.
- **Findings are committed**, unlike the prior project: research goes to
  `docs/research/<name>.md`, tickets to `issues/<name>.md`, and this map is tracked in git.
  Nothing wayfinding-related is gitignored.

## Inherited from `podcasts-notes-validation`

Prior work at `../podcasts-notes-validation` (GitHub: `jirigrill/podcasts-notes-validation`).
Carried over as **prior work to verify, not to re-derive**:

- **Spotify iOS SDK is remote-control only** (VERIFIED there) — it "offload[s] complexity to the
  main Spotify application such as: playback, authentication, networking, and offline caching" and
  requires the Spotify app installed. Relevant here because it constrains what playback control is
  possible at all.
- **Spotify Developer Policy bars any product "integrated with streams or content from another
  service"** — flagged INFERRED-fatal there for a different concept. **This clause is aimed almost
  exactly at this map's core mechanic** and is the single highest-priority thing to verify.
- **Spotify ToS IV.3.b bars local caching** of Spotify Content beyond metadata/cover art. Bears on
  the commuting/offline case.
- **YouTube Music has no third-party playback SDK**; Developer Policies III.I.7 bars separating
  audio from video, III.I.9 bars background players, III.I.14 bars non-API routes to content.
- **Apple Podcasts has no playback SDK** and no now-playing observation; `MPNowPlayingInfoCenter`
  is write-only. (Apple **Music**/MusicKit was *not* covered — that is new ground.)
- **App Store Guideline 5.2.3** — no saving/converting/downloading media from third-party sources
  without authorization. A review gate, not merely contract risk.
- **Open RSS podcast feeds have no ToS layer**; the binding constraint is copyright.
- **Base-rate methodology** for MRR arithmetic (RevenueCat benchmarks, install→paid conversion,
  LTV-per-install vs CPI). The *method* carries over; the *conclusion that costs are irrelevant*
  does **not** — see the unit-economics ticket.

Each ticket must state explicitly which of its findings were carried over and which are new.

## Decisions so far

<!-- one line per resolved ticket: gist + link -->

- [07 — Incumbent-feature teardown](issues/07-incumbent-features.md) — **The differentiator is
  occupied, and Spotify's policy bars the core mechanic by name.** Decisive new clause, VERIFIED
  independently by the orchestrator against <https://developer.spotify.com/policy>: **Policy III.7 —
  "Do not permit any device or system to segue, mix, re-mix, or overlap any Spotify Content with any
  other audio content"** (extending even to other Spotify material). That names the exact verbs for
  concept steps 3 and 5. It joins **III.5** ("integrated with streams or content from another
  service" — upgrading the map's carried-over INFERRED-fatal flag to **VERIFIED**) and **III.4**
  (bars non-interactive webcasting). The trap: Spotify's add-to-queue endpoint **explicitly accepts
  `episode` URIs**, so the mechanic is one API call away technically and contractually out of reach.
  **Step 5 is occupied**: Spotify DJ ships AI voice commentary over personalised music at no
  incremental charge to Premium — 75+ markets, 6 languages, still expanding May 2026, and on Android
  Auto (the developer's own commute context); YouTube Music's "Beyond the Beat" is a second entrant.
  **Steps 3+4 were shipped and killed by Spotify itself**: *Your Daily Drive* (June 2019) interleaved
  WSJ/NPR/PRI news with personalised songs in a drive-time format and was retired with no
  announcement before Apr 2026 — cause of death NO EVIDENCE FOUND. Graveyard's lesson: **talk you own
  scales; talk you license does not** (Daily Drive needed bilateral publisher deals per market) —
  load-bearing against step 4. **Apple inverts the map's ranking**: MusicKit has *no podcast type
  anywhere* (structurally uninterested, not one API call away), but is the most open surface and ships
  a real `crossfade(duration:)` from iOS 18+, relieving the hard-cut ceiling flagged in 05. **Tidal
  correction**: `@tidal-music/player` supports `AssetPresentation.FULL` — the only documented
  third-party full-track playback path found. **Unoccupied: one and a half of five steps**; only step
  3 has a real claim, and it is vacant because Spotify vacated it. Caveat: `WebSearch` failed all
  session, so findings are strong on clauses and weak on press/sentiment.

## Not yet specified

- **Whether a "talk you own" pivot is the surviving concept.** Ticket 07's graveyard analysis
  produced a structural lesson the map did not have: *talk you own scales; talk you license does
  not* — Spotify's own Daily Drive died needing bilateral publisher deals per market. Combined with
  Policy III.7 barring the segue mechanic on licensed music, this points at a variant where the
  **talk** is generated or owned rather than licensed. Too unsharp to ticket until 05 and 06 land,
  but it is now the most likely surviving shape and should not be lost.
- **The commuting / offline case as a second independent blocker.** Continuous radio while
  commuting implies CarPlay + Android Auto, hands-free control, poor connectivity, and buffering
  ahead. Prior work found **Spotify ToS IV.3.b bars local caching of Spotify Content beyond
  metadata and cover art** — so a stream that cannot buffer may be unusable in precisely the
  developer's own use case. Cannot be sharpened until the platform sweep says which platform (if
  any) survives; the question is different for a licensed-catalogue variant, where caching is the
  developer's own call. Note that Spotify DJ already ships **on Android Auto**, so the incumbent
  occupies the commute directly.
- **Which concept variant to price and spec**, once the platform sweep lands. Platform-native,
  licensed/royalty-free catalogue, and local-library variants likely differ enough in cost,
  audience, and legal posture that each may need its own arithmetic.
- **Where the "suitable moment" to interrupt a podcast comes from.** The concept assumes an
  episode can be paused at a sensible boundary. That is a transcript/segmentation problem with its
  own cost and accuracy profile — and the prior project found boundary accuracy was a real soft
  spot even for a funded incumbent. Only worth charting if a music path survives.
- **Distribution strategy**, if the verdict is build. Deliberately downstream of the go/no-go.
  Prior project's finding raises the stakes: paid acquisition was arithmetically ruled out there
  ($0.62 LTV/install vs $1.33 cheapest CPI), so organic is likely the only option.
- **What the developer's alternative use of the time is.** A "don't build" verdict is only
  meaningful against an alternative.

## Out of scope

- **Building any part of the app.** This map produces a decision; execution is a separate effort.
- **Non-audio personalisation** (video, visual radio, social/sharing features).
- **Becoming a licensed broadcaster / negotiating direct label deals.** Bespoke label licensing is
  out of reach for a solo developer at a $1–3k MRR bar; only self-serve or published-price
  licensing routes are in scope.
