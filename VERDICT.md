# Verdict: personalised radio app — DON'T BUILD

Decision taken 2026-09-14 by the developer, worked live in
[ticket 14](https://github.com/jirigrill/personalised-radio-validation/issues/15) on the
[map](https://github.com/jirigrill/personalised-radio-validation/issues/18). Every claim below is
tagged **VERIFIED** (read verbatim on a cited primary source, and in the decisive cases
independently re-checked by the orchestrator), **INFERRED** (our reasoning, stated so it can be
attacked), or **NO EVIDENCE FOUND**. Full detail lives in `docs/research/`; this document is the
index and the decision.

## The verdict

**Don't build.** Not "not yet", not "build narrower": the product as specified is **not currently
possible** for a solo developer to ship legally and sell. The core mechanic — interleaving a user's
own streamed music with podcast audio and AI-generated talk, and charging for it — is prohibited **by
name** in the developer terms of every music platform that has the user's taste data
(Spotify, Apple Music, YouTube Music, Tidal, Deezer), and each prohibition is drafted by a different
legal team using the same verbs. The alternative routes to music that *are* legal (statutory
webcasting, Feed.fm, royalty-free catalogues, the user's own files) each drop one of the three
things that make it this product — familiar music, an affordable price, or a DJ that may name the
tracks — and the developer has ruled every one of them out as **not the product they want**. There
is no engineering answer to a contract clause, and no negotiation route open to a developer at a
$1–3k/mo revenue bar. The market evidence points the same way: the one company that solved the
hardest sub-problem spent millions and booked $0 revenue, and Spotify itself shipped and quietly
killed the closest thing to this concept.

This is not a demand failure discovered after a feasibility pass. Feasibility failed first, on five
independent contracts, and the demand evidence would have failed it second.

## Why it is not currently possible

The concept has five steps. Each is blocked on its own; the table shows which step dies where.

| Concept step | Status | Killed by |
|---|---|---|
| 1. Log in via a streaming service | Possible, but useless | Login works everywhere; nothing downstream is permitted |
| 2. Read history, liked songs, followed podcasts | **BLOCKED on 4 of 5** | Spotify: `recently-played` excludes podcast episodes, recommendations gone post-Nov-2024 · Apple: no listening history, no podcast API anywhere · YouTube: history API removed 2016/2020 · Tidal: no podcasts since Jul 2024 |
| 3. Interleave podcast → songs → podcast | **BLOCKED on 5 of 5** | Spotify Policy III.7 "segue, mix, re-mix, or overlap" · Apple DPLA §3.3.6(D) "cannot be synchronized with any other content" · Tidal Guidelines §III "segues, overlaps, mixes … combines … with any other audio content" · YouTube III.I.9 background players / III.I.1 substitute service · Deezer ToS §IV non-commercial only |
| 4. Freely broadcastable news | Legal on RSS, occupied and dead | Spotify built and retired *Your Daily Drive*; licensed talk needs per-market publisher deals |
| 5. AI DJ that names tracks | **BLOCKED on every statutory route** | 17 U.S.C. §114(d)(2)(C)(ii) bars pre-announcing titles/artists; Feed.fm T&C §5.5–5.6 bars stream alteration; Spotify DJ ships it free |
| **Charge for it** | **BLOCKED on both big platforms** | Spotify Policy IV.2 bans monetising Streaming SDAs · Apple DPLA §3.3.6(D) "not … require payment for or indirectly monetize access" |

### 1. The platform contracts prohibit the mechanic by name — VERIFIED, five times

- **Spotify** ([01](https://github.com/jirigrill/personalised-radio-validation/issues/1),
  [docs/research/01](docs/research/01-spotify-feasibility.md)). Dies four independent ways.
  [Developer Policy](https://developer.spotify.com/policy) III.7: *"Do not permit any device or
  system to segue, mix, re-mix, or overlap any Spotify Content with any other audio content
  (including other Spotify Content)"* — "segue" is the first verb, so a hard cut is barred and the
  parenthesis closes the Spotify-only retreat. III.5 bars integration with another service's
  content. IV.2 bans monetising a Streaming SDA, and the ToS define Streaming to include remote
  control of the background app "regardless of whether it also provides additional functionality".
  Spotify's own [Compliance Tips](https://developer.spotify.com/compliance-tips) lists **"DJ/Mixes"**
  as a disallowed use case and gives *"charging users $5/month for a home automation app that
  triggers music"* as its example of disallowed monetisation — structurally this app's pricing.
  Extended quota is organisations-only and requires 250k MAU on a launched service; development
  mode caps at 5 users. No third-party SDK exposes PCM, volume or download (headers grepped: zero
  matches), so music silently dies in a tunnel while podcast audio plays — the developer's own
  commute.
- **Apple Music** ([02](https://github.com/jirigrill/personalised-radio-validation/issues/2),
  [docs/research/02](docs/research/02-apple-music-feasibility.md)). Technically the *best* fit —
  app-owned queue, coexisting audio sessions — and blocked on contract.
  [DPLA](https://developer.apple.com/support/downloads/terms/apple-developer-program/Apple-Developer-Program-License-Agreement-English.pdf)
  §3.3.6(D), three fatal clauses: *"MusicKit Content cannot be synchronized with any other
  content"*; purpose-limited to facilitating the user's Apple Music subscription; and *"You agree
  not to require payment for or indirectly monetize access to the Apple Music service (e.g. in-app
  purchase, advertising, requesting user info)"*. The third survives any engineering: there is no
  legal revenue model, and free cannot clear $1k/mo. Repeated in
  [App Store Review Guidelines](https://developer.apple.com/app-store/review/guidelines/) 4.10 and
  4.5.2(i). No listening history, no podcast data on any Apple API. Crossfade not exposed.
- **YouTube Music** ([03](https://github.com/jirigrill/personalised-radio-validation/issues/3),
  [docs/research/03](docs/research/03-youtube-music-feasibility.md)).
  [Developer Policies](https://developers.google.com/youtube/terms/developer-policies) III.I.1
  (no substitute for YouTube's core experience — programmed music+podcast radio *is* YouTube
  Music's core UX), III.I.7 (no separating audio), III.I.9 (no background players), III.I.14 (no
  non-API routes). Policies re-verified current at 2026-06-24. History API deprecated 2016, removed
  from docs 2020 — step 2 dies independently of policy. No playback SDK exists; the IFrame
  [Required Minimum Functionality](https://developers.google.com/youtube/terms/required-minimum-functionality)
  demands a visible 200×200 player.
- **Tidal** ([04](https://github.com/jirigrill/personalised-radio-validation/issues/4),
  [docs/research/04](docs/research/04-tidal-feasibility.md)). The best playback SDK in the sweep
  and the worst terms. [Developer Guidelines v3.0](https://developer.tidal.com/documentation/guidelines/guidelines-developer-guidelines)
  §III prohibits *"functionality that segues, overlaps, mixes or re-mixes any TIDAL content or
  otherwise alters TIDAL content or combines any TIDAL content with any other audio content
  (including without limitation other TIDAL content)"*, plus integrating with another service, an
  AI/ML ban, and "generating news media" — five clauses tripped. Third parties get 30-second
  previews. Zero podcasts since 24 Jul 2024. Sub-1% market share; Block has written off ~89% of
  its purchase price.
- **Deezer** ([04b](https://github.com/jirigrill/personalised-radio-validation/issues/5),
  [docs/research/04b](docs/research/04b-wider-platform-sweep.md)).
  [API Terms](https://developers.deezer.com/termsofuse) Section IV is titled **"NON-COMMERCIAL
  USE"**; use is *"limited to a strictly private use within a family scope"*.

**INFERRED, strongly**: the "metadata and cover art only" caching formula and the "content from
another service" clause appear near-verbatim in Spotify's *and* Tidal's independently drafted terms.
That is the signature of a **label-licensing requirement flowing downstream**, not one platform's
policy choice. There is no friendlier platform to find; the constraint sits above the platforms.

### 2. Every alternative music route drops a defining property — VERIFIED, and rejected by the developer

[04b](https://github.com/jirigrill/personalised-radio-validation/issues/5) and
[05](https://github.com/jirigrill/personalised-radio-validation/issues/6) swept ~45 further routes.
The result is a trilemma: **familiar music / affordable / DJ may name tracks — pick two.**

| Route | Familiar | Affordable | DJ names tracks | What kills it |
|---|---|---|---|---|
| Statutory US webcasting (SoundExchange) | yes | no | **no** | [17 U.S.C. §114(d)(2)(C)(ii)](https://www.law.cornell.edu/uscode/text/17/114) flatly bars pre-announcing titles or artists — that is step 5. [37 CFR §380.10](https://www.law.cornell.edu/cfr/text/37/380.10): $1,000/yr/channel minimum plus $0.0032/performance ≈ **$17,500/yr at 1,000 subscribers** against $12–36k revenue. Whether each personalised stream is a "channel" is unresolved. US-only |
| Feed.fm / Adaptr | yes (major labels) | NO EVIDENCE FOUND | no | Shuffle-only genre stations; [T&C](https://www.feed.fm/terms-and-conditions) §5.5–5.6 bar insertion/alteration; **US/CA/AU only** (developer is in Czech Republic); sales-gated, quarterly prepaid, one-year auto-renew; zero indie customers or user reviews found |
| 7digital / Tuned Global | yes | no | no | Tuned Global is technology-only, not rights; 7digital makes you the royalty payer; DMCA pre-announcement bar still applies |
| Royalty-free / CC catalogues | **no** | yes | yes | Not the user's music — kills the personalisation premise |
| User's own local library (`MPMediaItem`) | yes | yes | yes | Legal, private performance sits outside §114. Fails on **market**: the whole incumbent iOS local-player field sits at 82–425 App Store ratings each |
| Spoken-audio-first (no music) | n/a | yes | yes (podcast pre-announcement is legal) | Legally cleanest variant found; not a music product |

The developer, in the 05 session, rejected the three surviving corners — spoken-audio-first,
Feed.fm/Adaptr, local-library — as **not the product they want**. That is recorded on the map as a
scoping decision, and this verdict honours it: a variant nobody wants to build is not a "build
narrower".

### 3. The market evidence points the same way — VERIFIED

- **Spotify already occupies step 5 for free.** Spotify DJ ships AI voice commentary over
  personalised music at no incremental charge, 75+ markets, 6 languages, on Android Auto — the
  developer's own commute context ([07](https://github.com/jirigrill/personalised-radio-validation/issues/8)).
- **Spotify shipped steps 3+4 and killed them.** *Your Daily Drive* (Jun 2019) interleaved
  WSJ/NPR/PRI news with personalised songs in a drive-time format and was retired without
  announcement. Cause of death NO EVIDENCE FOUND; the licensing lesson stands — licensed talk
  needed bilateral publisher deals per market.
- **The one company that solved the hard sub-problem made $0.** Auddia Inc. (NASDAQ: AUUD) built
  *faidr*, which detects boundaries between ads, songs and DJ talk in real time. Per its
  [10-K filed 2026-03-06](https://www.sec.gov/cgi-bin/browse-edgar?action=getcompany&CIK=0001554818)
  (VERIFIED, CIK 0001554818): revenue **$0 in FY2024 and FY2025**; net loss $7.69M FY2025; **687 App
  Store ratings after $907k of marketing in six months**; abandoned direct-to-consumer in 2026;
  going-concern warning. Its audited risk factors rest freedom to operate on the personal-use
  time-shifting exemption — a fragile theory a solo developer cannot afford to test
  ([08](https://github.com/jirigrill/personalised-radio-validation/issues/9)).
- **The indie field is a graveyard.** Scout FM (podcast radio stations from history, CarPlay) —
  acquired by Apple 2020, shut; Swell — acquired by Apple 2014 after $30M, shut; AudioOne — the
  only product ever promising news+podcasts+music in one stream — NXDOMAIN. Living closest
  competitor SUB/WAVE: free, MIT-licensed, 1,307 GitHub stars, **4 iOS ratings**. Differentiator
  audit: 12 candidates, **0 unambiguously safe**.
- **No user review text was obtained anywhere** — Apple's review RSS returned empty for every
  app and `WebSearch` was down across sessions. The largest evidence gap; it cuts against the
  concept, since it means no demand signal was found, not that it was found and discounted.

### 4. The MRR arithmetic, re-run at the $1k floor

Tickets 09–12 were closed as moot once no music source survived, so this is the base-rate re-run the
map required, not a full funnel model. Method carried over from
[`podcasts-notes-validation` ticket 07](https://github.com/jirigrill/podcasts-notes-validation);
numbers re-pulled fresh 2026-09-14 from the RevenueCat *State of Subscription Apps 2026*
[chart payload](https://www.revenuecat.com/page-data/state-of-subscription-apps/page-data.json)
(VERIFIED). Share of newly-launched apps reaching a monthly-revenue milestone within 2 years:

| Milestone | Media & Entertainment | Productivity (prior project) | All categories |
|---|---|---|---|
| **$1K/mo (the floor)** | **19.9%** | 15.3% | 17.3% |
| $2.5K/mo | 12.4% | 8.8% | 10.7% |
| $3K/mo (the target) | NO EVIDENCE FOUND — between 12.4% and 8.0% | — | — |
| $5K/mo | 8.0% | 5.7% | 7.2% |

Read honestly: about **1 in 5** Media & Entertainment apps reach the floor, **roughly 1 in 9 or
10** reach the target — better than the prior project's 1-in-11 at $2.5k, because the bar is lower
and the category richer. **INFERRED**: that denominator includes funded teams and apps with
audiences; a solo cold-start developer sits in its unfavourable tail. And the figure is conditional
on being *able to ship and charge*, which section 1 shows this concept cannot. The improved base
rate is therefore true and irrelevant: it describes a population this app cannot join.

Cost side (VERIFIED, 04b): the only cleanly-priced component in the entire sweep is ListenBrainz
commercial use at $100/mo ([MetaBrainz Bronze](https://metabrainz.org/supporters/account-type)) —
a taste layer with no audio attached.

## The steelman for building, at full strength

- **The developer is the user.** A real, recurring, daily commute use case, which the prior
  project's developer lacked. This is the strongest thing on the map and it is genuine.
- **The join is unoccupied.** No living product interleaves licensed music with the user's own
  podcast episodes. Step 3 is vacant.
- **Personalisation is not automatically "interactive"** under §114: *Arista v. Launch Media*
  (2d Cir. 2009) held LAUNCHcast non-interactive on a control-vs-influence test (VERIFIED). A
  liked-songs pool is a harder case, but not a settled loss.
- **Podcast pre-announcement is legal.** A DJ that talks about the next *episode* trips nothing.
- **The Media & Entertainment base rate is the best in the report** short of Gaming and
  Photo & Video.

Why it loses: every item above either describes a variant the developer has rejected, or a
property that the contracts in section 1 make unusable. A sample of one cannot outweigh five
independent legal kills, and "unoccupied" here means *vacated* — by Spotify, by Apple's
acquisitions, and by a public company that spent $7.7M a year to get to $0.

## The steelman against building, stated plainly

1. The core mechanic is prohibited by name on every platform that holds the user's taste data, with
   no written-approval escape hatch on the operative clauses (Spotify III.4/5/7 carry none; III.1
   and III.11 do).
2. Charging for it is separately prohibited on the two platforms that matter (Spotify IV.2, Apple
   DPLA §3.3.6(D)), so even a hypothetically compliant version has no revenue model.
3. Spotify's developer program gate — extended quota requires 250k MAU on a launched product and a
   review-team test of the app — precedes any enforcement question. You never reach the argument.
4. The legal music routes that remain make the DJ mute (§114), the music unfamiliar (royalty-free),
   or the audience 82–425 people (local library).
5. Spotify DJ already gives the AI-talk layer away free to the same commuter, on the same head unit.
6. The user already pays for a music subscription and, often, a podcast app; this asks for a third
   subscription to do what the first one is contractually forbidden to let it do.
7. The nearest well-funded attempt booked $0 revenue over two audited years.
8. Solo, no audience, cold start — the unfavourable tail of a base rate that is only ~1 in 5 at
   the floor even for apps that *can* ship.

## What would have to change to reopen this

Durable conditions, so the decision is not relitigated next month. **Any one** of the first three
reopens the platform-native variant; nothing short of them does.

- **A platform deletes or grants written exception to its interleaving clause** — Spotify Policy
  III.7 and IV.2, Apple DPLA §3.3.6(D), or Tidal Guidelines §III. Check the clause text, not the
  marketing. Also requires the platform to expose podcast history (Spotify) or podcasts at all
  (Apple, Tidal).
- **A self-serve, published-price, EU-available licensed catalogue with per-track programmatic
  control and a right to announce tracks** appears. Nothing like it exists today; Feed.fm is the
  closest and fails on region, interactivity and announcement.
- **Statutory webcasting changes** — §114(d)(2)(C)(ii) amended, or a ruling that AI-generated
  untimed commentary is not an "advance program schedule or prior announcement". Would still leave
  US-only geography and the per-performance rate.
- **The developer changes their mind on a rejected variant.** Spoken-audio-first (podcast + news +
  AI DJ, no music) is legally clean and was the only corner with a route to $1k. Local-library is
  legally clean and market-dead. Reopening either is a *new* effort, not a resumption of this one.
- **Personal, non-commercial tool for the developer alone** — not in scope of this map. Note that
  Spotify's and Tidal's clauses bar the mechanic regardless of monetisation, and Deezer's private-use
  carve-out is the only one that would plausibly cover it.

## Inheritance note

Carried over from [`podcasts-notes-validation`](https://github.com/jirigrill/podcasts-notes-validation)
and **verified rather than re-derived**:

| Finding | Prior status | Status here |
|---|---|---|
| Spotify iOS SDK is remote-control only | VERIFIED | VERIFIED, upgraded — headers grepped, zero PCM/volume/duck symbols |
| Spotify Policy bars "integrated with … another service" (III.5) | INFERRED-fatal | VERIFIED, and found to be the *weakest* of three applicable clauses; III.7 is decisive |
| Spotify ToS bars local caching beyond metadata/cover art | VERIFIED | VERIFIED; same formula found independently in Tidal's terms |
| YouTube Policies III.I.7 / III.I.9 / III.I.14 | VERIFIED | VERIFIED current at 2026-06-24; **III.I.1** added as a second independent kill |
| Apple Podcasts has no playback SDK; `MPNowPlayingInfoCenter` write-only | VERIFIED | Confirmed; extended — no podcast data on *any* Apple API |
| App Store Guideline 5.2.3 | VERIFIED | Not load-bearing here; ASRG 4.10 / 4.5.2(i) are |
| Open RSS has no ToS layer; copyright binds | VERIFIED | Carried unchanged (06 closed as moot) |
| Base-rate method (RevenueCat milestones, D35 conversion, LTV vs CPI) | VERIFIED method | Method reused; **figures re-pulled fresh** for Media & Entertainment |
| "Costs are irrelevant" conclusion | — | **Not carried.** SoundExchange per-performance rate would dominate at scale |

**Researched fresh in this project**: Spotify III.7, IV.2, Compliance Tips "DJ/Mixes", quota gate,
podcast-history gap, post-Nov-2024 endpoint removals (01); all of Apple Music / MusicKit / DPLA
§3.3.6(D) (02); YouTube III.I.1 and history-API removal (03); all of Tidal, including the SPA
discovery method (04); §114(d)(2)(C)(ii), 37 CFR §380.10, *Launch Media*, Deezer §IV, ListenBrainz
pricing, Feed.fm, 7digital, Tuned Global, Jellyfin, `MPMediaItem` (04b); Feed.fm market check and the
~40-candidate indie API sweep (05a/05b); Spotify DJ, *Your Daily Drive*, MusicKit crossfade
correction (07); Auddia 10-K, Scout FM, Swell, AudioOne, SUB/WAVE, differentiator audit (08).

## Known gaps

- `WebSearch` was unavailable in every session. Findings are strong on primary-source clause text
  and weak on press, sentiment and enforcement precedent. **No named app suspension** under Spotify
  III.5/III.7 was found; the "prohibited in practice" claim rests on the quota gate, not on
  enforcement history.
- No user review text was obtained for any competitor.
- Feed.fm, 7digital and Tuned Global pricing: NO EVIDENCE FOUND. Vendor enquiries (ticket 15) were
  closed as moot rather than sent.
- Whether SoundExchange counts a per-user personalised stream as one "channel" or many: unresolved
  and would require counsel.
- Tidal Guidelines v3.0 (6 May 2024) is the newest text in the served bundle; "currently effective"
  is unconfirmed because the version endpoint requires auth.

None of these gaps, if closed favourably, reverses a section-1 finding.

## Ticket index

| Ticket | Verdict |
|---|---|
| [01 Spotify](https://github.com/jirigrill/personalised-radio-validation/issues/1) | BLOCKED, four independent ways |
| [02 Apple Music](https://github.com/jirigrill/personalised-radio-validation/issues/2) | BLOCKED on contract; no legal revenue model |
| [03 YouTube Music](https://github.com/jirigrill/personalised-radio-validation/issues/3) | Eliminated: policy and absent API |
| [04 Tidal](https://github.com/jirigrill/personalised-radio-validation/issues/4) | BLOCKED; least permissive of all |
| [04b Wider sweep](https://github.com/jirigrill/personalised-radio-validation/issues/5) | AI DJ legally incompatible with every affordable route |
| [05 Platform sweep](https://github.com/jirigrill/personalised-radio-validation/issues/6) | None survive; remaining variants rejected by developer |
| [07 Incumbent teardown](https://github.com/jirigrill/personalised-radio-validation/issues/8) | Differentiator occupied; mechanic barred by name |
| [08 Indie landscape](https://github.com/jirigrill/personalised-radio-validation/issues/9) | Empty because everyone who tried failed |
| 06, 09–13, 15, 16 | Closed as moot — cannot change the verdict |
| [14 Verdict](https://github.com/jirigrill/personalised-radio-validation/issues/15) | **Don't build** — this document |
