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
- [08 — Indie competitive landscape](issues/08-indie-competitive-landscape.md) — **Saturation
  verdict: (b) empty because everyone who tried failed**, with a strong secondary reading of
  **(c) already solved by an adequate free substitute**. Emphatically not (a). The concept *is*
  genuinely unoccupied — no living product interleaves licensed music with the user's own podcast
  episodes; every living AI-DJ app mixes *synthesised* talk with music, every living AI-talk app has
  no music. **The join is a licensing problem wearing a product-problem costume.** But it is a
  graveyard, not virgin ground: **Scout FM** ("radio-like stations for podcasts" from listening
  history, with CarPlay) was bought by Apple in 2020 and switched off — HN noticed with **1 upvote**;
  **Swell** (personalised talk radio) was bought by Apple in 2014 after $30M and discontinued;
  **AudioOne**, the only product ever found promising news+podcasts+music in one stream (Show HN, 59
  pts), is now **NXDOMAIN**. **The decisive find is an SEC filing, not an indie app** (VERIFIED,
  sec.gov CIK 0001554818, 10-K filed 2026-03-06; company and filing date independently confirmed by
  the orchestrator): **Auddia Inc. (NASDAQ: AUUD)** built *faidr*, which **solved the hardest
  sub-problem this concept needs** — real-time detection of boundaries between commercials, songs and
  DJ talk. Results: **revenue $0 in FY2024 and FY2025**; $7,584 in H1 2026 (B2B, not consumers); net
  loss **$7.69M** in FY2025; **687 App Store ratings after $907k of marketing in six months**; going
  concern into Q2 2027; and in 2026 it **abandoned direct-to-consumer entirely** while merging into
  its own CEO's unrelated holding company. Two things no indie datapoint could establish: consumer
  demand is absent **at any spend level a solo developer can reach**, and the legal theory is named
  and fragile — Auddia's audited risk factors rest its freedom to operate on the **"personal use
  exemption" for time-shifting**. Hand that to 01–05 as prior art. **Differentiator audit: 12
  candidates, 0 unambiguously safe** (worse than the prior project's 1-of-9); the two
  structurally-uncopied items are uncopied for reasons that *also stop this concept* — the
  interleaving itself is legally blocked, and suitable-moment pausing is what Auddia built and
  monetised at $0. **Volume across the living field is negligible**: SUB/WAVE (closest competitor,
  free/MIT, 1,307 GitHub stars, shipping daily) has **4 iOS ratings**; AITUNE 2; musen 0;
  doremiradio 0; largest relevant indie InstaPodz 156. Caveats: `WebSearch` broken all session, and
  Apple's review RSS returned empty for every app — **no user review text was obtained anywhere**,
  the largest gap.
- [04b — Wider platform sweep](issues/04b-wider-platform-sweep.md) — **The blocker is not price. It
  is that the AI DJ announcing tracks in advance is legally incompatible with every affordable route
  to familiar music.** **17 U.S.C. §114(d)(2)(C)(ii)** (VERIFIED, and independently re-verified by the
  orchestrator at Cornell) bars a statutorily-licensed webcaster from causing or facilitating
  publication, via "an advance program schedule or prior announcement", of "the titles of the specific
  sound recordings to be transmitted" or "the names of the featured recording artists". Flat
  prohibition, no de minimis — only vague untimed artist teasers survive. **That is concept step 5,
  and it binds every statutory route, Feed.fm Radio included.** The vendor-side equivalent is Feed.fm
  T&C §5.6/§5.5(a), barring insertion into or alteration of the stream. **One correction to the
  ticket's own premise**: personalisation is *not* interactive by definition — *Arista v. Launch
  Media* (2d Cir. 2009) held LAUNCHcast non-interactive on a control-vs-influence test. But a
  your-liked-songs pool is a materially harder case, because LAUNCHcast survived precisely by *not*
  being a library player. Lawyer required. **Verified costs**: SoundExchange minimum **$1,000/yr per
  channel** and **$0.0032/performance** (37 CFR §380.10) → **≈$17,500/yr at 1,000 subscribers**
  against a $12–36k/yr revenue target — the minimums are affordable, the per-performance rate is what
  kills it, and how SoundExchange counts "channels" for per-user personalised streams is the
  highest-leverage unknown in the cost model. **ListenBrainz commercial use is $100/mo** (MetaBrainz
  Bronze) — the only affordable, cleanly-priced, verified component in the whole sweep. **Feed.fm /
  7digital / Tuned Global pricing: NO EVIDENCE FOUND** (`WebSearch` down all session) → 2 TASK tickets
  filed. **Hypotheses from the unattributed pass, now resolved**: (a) Tuned Global is
  **technology-only, not a rights provider — CONFIRMED**, correcting the scout's table; (b) Feed.fm
  SDKs live (iOS v5.8.0, 2026-06-02) and it is the only full bundler — CONFIRMED; (c) 7digital makes
  you the royalty payer and the DMCA bars pre-announcing — CONFIRMED both limbs; (d) royalty-free/CC =
  masters only → PRO liability — **PARTIAL, overstated** (CC 4.0 expressly includes public
  performance; the real risk is *authority*, not scope); (e) Mubert terms unpublished and
  contradictory — CONFIRMED. **New blocks**: **Deezer is dead on its ToS, not its SDK** — Section IV is
  literally titled "NON-COMMERCIAL USE", use "limited to a strictly private use within a family
  scope"; the scout's "`/user/me/history` returns 200" was a 200 with an OAuthException body.
  **Jellyfin is not natively Subsonic-compatible** (only an unmaintained 2022 bridge) but has an
  official Swift SDK and the richest taste signal found. **iOS `MPMediaItem` verified against the
  local iOS 27.0 SDK header**: play-count properties all present and not deprecated; `assetURL`
  nullable with `isCloudItem`/`hasProtectedAsset` as filterable flags. **The answer for 05**: three
  routes play familiar music; **none is familiar + affordable + DJ-compatible**. The user's **own
  library** is the only route that is legal, free, familiar **and** lets the DJ announce freely
  (private performance sits outside §114 entirely) — it fails purely on market size: the whole
  incumbent iOS field sits at **82–425 App Store ratings each**, and $1k MRR needs ~400–500 concurrent
  subscribers cold-start against free mature competitors. **The escape hatch worth carrying to 05 and
  13**: announcing *podcast* episodes in advance is perfectly legal — the pre-announcement bar is a
  **sound-recording** rule. A spoken-audio-first product keeps the DJ premise and drops the one
  structurally unreachable component.
- [02 — Apple Music / MusicKit](issues/02-apple-music-feasibility.md) — **BLOCKED on contract, not on
  code.** The ticket's designated make-or-break question resolves *in the concept's favour* and it
  does not matter: Apple Music audio and third-party audio **can** coexist in one `AVAudioSession`
  (`prepareToPlay()` interrupts only "nonmixable" sessions; `.playback` + `.mixWithOthers` set once),
  and playback control is **better than Spotify's** — app-owned queue, `pause()`, settable
  `playbackTime`. The concept dies on the licence. **DPLA §3.3.6(D)** (Aug 18 2026 ed., p.32),
  VERIFIED verbatim, three independently fatal clauses: (i) **"MusicKit Content cannot be synchronized
  with any other content"** — names the interleaving mechanic exactly; (ii) purpose-limitation to
  "facilitating access to Your end users' Apple Music subscriptions", relevance at Apple's sole
  discretion; (iii) **"You agree not to require payment for or indirectly monetize access to the Apple
  Music service (e.g. in-app purchase, advertising, requesting user info)"**. **Clause (iii) is the
  hardest finding and was not in the map's framing** — it survives any amount of engineering: even a
  variant that satisfied the synchronization clause has **no legal revenue model** at the $1–3k MRR
  bar, and going free does not help, because the clause names advertising and free cannot clear
  $1k/mo. Independently re-verified by the orchestrator: **ASRG 4.10** names "Apple Music access"
  explicitly, and **4.5.2(i)** repeats the prohibition almost verbatim. **Crossfade is BLOCKED** —
  zero volume/gain/mute symbols in MusicKit, `PlayParameters` documented "opaque"; hard cut is the
  ceiling. **This corrects ticket 07's report** of a usable `crossfade(duration:)` on iOS 18+.
  **No listening history** (10 resources / 30 tracks cap) and **no podcast data on any Apple API** —
  the latter breaks concept step 2 independently and is **Apple-wide, not MusicKit-specific**, so any
  iOS-first variant needs RSS/OPML import regardless of music source. **Two corrections to likely
  priors**: no broadcast/radio-station prohibition exists in any Apple document — do not cite one; and
  **no standalone MusicKit ToS exists** — the DPLA is the operative contract. Apple's competitive
  threat is LOW; Spotify is the incumbent. Caveats: Apple Developer Forums return Cloudflare 403 to
  automated fetching, and no Apple engineer statement exists on which session `ApplicationMusicPlayer`
  uses internally — the process-locality reasoning is INFERRED from the API surface plus third-party
  reports.

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
