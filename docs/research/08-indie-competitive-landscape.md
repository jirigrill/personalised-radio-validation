# 08 — Wider competitive landscape: indie apps, living and dead

Ticket: `issues/08-indie-competitive-landscape.md` · Status: resolved · Date: 2026-09-08

**Carried over from `podcasts-notes-validation`:** nothing substantive. The prior project's
*method* (tag VERIFIED / SECONDARY / NO EVIDENCE FOUND; report volume honestly; discipline the
differentiator audit) is carried over. Every app, figure, and quote below is **new work**.

---

## Headline

The exact concept — **licensed catalogue music interleaved with the user's own followed podcast
episodes, programmed as a continuous stream** — is **built by nobody living**. Not by an indie,
not by an incumbent, not by an open-source project. That is a genuine void.

But it is **not an unexplored void**. It has been attempted at least a dozen times across twelve
years and the attempts have a consistent shape: the *talk-and-music* products died or were
absorbed, and the survivors all routed around the same obstacle by **dropping one of the two
ingredients**. Every living AI-DJ product mixes *synthesised talk* with music. Every living
AI-talk product has *no music*. The join is empty because the join is a licensing problem, not a
product problem.

The single most decisive datum in this ticket is not an indie app. It is a **Nasdaq-listed company
(Auddia, AUUD) that spent 14 years and at least $7.7M/yr building the closest adjacent product,
reported $0 revenue for FY2024 and FY2025, $7,584 for H1 2026, and in 2026 abandoned the
direct-to-consumer model entirely** — while naming, in its own risk factors, the precise legal
theory this concept would have to rely on. Primary sources below.

---

## Competitor table

Volume is reported as **n = rating count** (the honest measure of traction for a consumer app).
All App Store figures VERIFIED via `itunes.apple.com/lookup` on 2026-09-08.

### Tier 1 — Does it programme music + the user's podcasts as one stream? (the actual concept)

| Product | Status | Music source | Podcasts? | Price | Volume | Verdict |
|---|---|---|---|---|---|---|
| **— none found —** | | | | | | The concept is unoccupied |

No living product does this. The rest of this table explains what exists instead.

### Tier 2 — AI DJ over licensed/owned music (talk is *synthesised*, not podcasts)

| Product | Status | Music source | Podcasts? | Price | Volume (n = ratings) | Notes |
|---|---|---|---|---|---|---|
| **Spotify AI DJ** | Alive | Spotify catalogue | No (music only; can *route to* podcasts) | Premium | n/a (feature) | The incumbent. HN's largest signal is a **complaint**: "The Appalling Stupidity of Spotify's AI DJ", **370 pts / 294 comments** |
| **SUB/WAVE** (`getsubwave.com`) | Alive, very active | **Your own library** (Navidrome/Subsonic) | **No** | Free, MIT, self-host | **1,307 GitHub stars**; iOS **n = 4** (5.0) | Closest living competitor — teardown below |
| **AITUNE – AI Radio + DJ** | Alive | **Apple Music** (MusicKit) | No | Free + premium TTS | **n = 2** (5.0), rel. 2026-06-17 | Proves the Apple Music path is *shippable*; proves nothing about demand |
| **musen AI Radio** | Alive | Not disclosed; "does not generate the songs it plays" | No | Free | **n = 0**, rel. 2026-07-10 | 14 months of dev, zero ratings |
| **doremiradio: AI Radio** | Alive | Not disclosed | No | Free | **n = 0**, rel. 2026-06-25 | |
| **Mureka: AI Radio & Music Maker** | Alive | **Generates** the songs | No | Free | **n = 10,865** (4.79) | The only AI-radio app with real volume — and it sidesteps licensing by generating the music |
| **Radio AI** (Play Store, M. Rybachenko) | Alive | Unverified | Unverified | Unverified | NO EVIDENCE FOUND (listing 404'd) | |
| **iHeartRadio AI DJ**, **Deezer Flow**, **Amazon Music AI**, **YT Music Ask Music** | UNVERIFIED | — | — | — | — | All official pages 404'd; do not cite |

### Tier 3 — Adjacent: AI-generated talk radio (no licensed music at all)

| Product | Status | Volume | Notes |
|---|---|---|---|
| **InstaPodz – AI Radio & Podcast** | Alive | **n = 156** (4.71), rel. 2025-07-10 | Largest genuinely-relevant indie. Generates the talk; **no music**. Its founder's post-mortem is the single most useful document in this ticket (below) |
| **Curia** (`curia.surge.sh`) | Alive, launched 2026-08-17 | HN Show HN: **2 pts, 0 comments** | Your saved links → a produced audio show. No music |
| **Noa: Listen to audio articles** | Alive | **n = 371** (4.55) | Narrated journalism queue. No music |
| **hermestransmissions.com** | Alive | HN: **6 pts, 0 comments** | Autonomous AI radio station |
| **NotebookLM Audio Overviews**, Wondercraft, Podcastle, ElevenLabs | UNVERIFIED here | — | Generation tools, not programmers of a stream |

### Tier 4 — The honest substitute, and the aggregators

| Product | Status | Volume | Programmes a stream? |
|---|---|---|---|
| **A playlist + a podcast app** (Spotify / Apple Music + Overcast / Pocket Casts) | **This is the real competitor** | Spotify iOS **n = 41,965,524**; Apple Podcasts **n = 2,073,804**; Overcast **n = 47,701**; Pocket Casts **n = 15,677** | **No** — the user does the programming, by switching apps. Cost: two taps at a traffic light |
| **Pandora: Music & Podcasts** | Alive | **n = 11,298,274** (4.80) | Music stations + podcasts, **separately**. Pandora is the last major platform shipping human voice-tracks between full music tracks (as Curated Programs, ex-"Stories") |
| **iHeart: Radio, Music, Podcasts** | Alive | **n = 640,730** | Broadcast radio *does* mix music and talk — professionally, licensed, and free |
| **SiriusXM** | Alive | **n = 3,075,988** | Same. Free-at-the-margin in a car |
| **faidr** (Auddia, NASDAQ: AUUD) | Alive but pivoting away | **n = 687** (4.65) | See the Auddia case below — the most instructive single datum in this ticket |
| **Plexamp** / **Marvis Pro** / **Longplay** / **Doppler** / **Sleeve** | Alive | 10,054 / 1,270 / 165 / 1,514 / 9 | Power-listener clients. **None programme a stream.** Marvis Pro at $9.99 / n=1,270 is the honest ceiling on "power listener who pays" |
| **Snipd** / **Podurama** / **Speechify** / **Live365 Radio – Music & Talk** | Alive | 1,142 / 397 / 522,429 / 5,988 | Queue and TTS apps; Live365 is a *tuner*, not a programmer |

---

## 1. Direct competitors

**Finding: zero living products match the concept.** Reporting the search volume honestly, because
the emptiness is the datum.

**App Store sweep** (VERIFIED — `itunes.apple.com/search`, 11 query terms, 2026-09-08):
"personal radio station", "AI radio DJ", "AI DJ", "personalised radio", "personalized radio",
"radio show generator", "my own radio station", "podcast and music player", "music and podcasts
mixed", "AI radio host", "radio station creator". Results were dominated by **FM/AM tuners**
(Simple Radio n=528,232; myTuner n=243,314) and **AI song generators** (Suno n=341,961; Mureka
n=10,865). A programmatic filter for apps whose description contains *podcast* AND *music* AND
*radio* returned **19 apps, all of which were either incumbents or tuners** — not one programmes a
mixed stream.

The only three apps in the entire sweep whose stated purpose is "AI radio built from your own
listening taste" are **AITUNE (n=2)**, **musen (n=0)** and **doremiradio (n=0)** — all released
June–July 2026, **all music-only, none touching podcasts**.

- AITUNE is worth one line of attention because it is the **existence proof for the Apple Music
  variant**: it advertises "100% Apple Music" native integration, "DRM-respecting, fully licensed",
  and pitches itself for "commuting" (VERIFIED — App Store description + `aitune-web.vercel.app`).
  It also confirms the ceiling: a working, shipped, licensed implementation of the *music half* of
  this concept has **2 ratings after three months**.
- musen's own description states: "musen uses AI to understand intent and orchestrate existing
  music. It does not generate the songs it plays." (VERIFIED — App Store). No podcasts.

**Hacker News sweep** (VERIFIED — `hn.algolia.com/api/v1`):
- Query `"personal radio"` → **9 stories total, all time**. The single closest match to this concept
  in HN's entire history is *"Show HN: I Created a personal radio platform that delivers news,
  podcasts, music"* — **59 points, 27 comments, 2022-08-06**, at `audioone.fyi`. **That domain now
  returns NXDOMAIN** (VERIFIED — `getaddrinfo ENOTFOUND www.audioone.fyi`; last Wayback capture
  2024-11-22, with 402s from Feb 2025). See the graveyard.
- Query `"AI radio"` (points > 2) → **11 stories, all time**, of which 3 are Show HNs, scoring
  **12, 6 and 3 points**.
- Query `"AI DJ"` → 28 stories. The **largest by an order of magnitude is a complaint**: *"The
  Appalling Stupidity of Spotify's AI DJ"*, **370 points / 294 comments**.
- Queries `radio station generator podcast`, `podcast music mix queue player`, and
  `Show HN radio station AI music podcast` (show_hn tag) → **0 hits each**.

**GitHub sweep** (VERIFIED — GitHub search API): the query `"personal radio" in:description` returns
**62 repos**; the top two have **14 stars each** (`bengfarrell/sharkattack`, last pushed 2015;
`line72/boldaric`). A query for AI-DJ radio streamers returned **16 repos, of which 14 have 0 stars
and 2 have 1 star**. `"podcast" "music" radio dj in:description` → **0 results**. A dozen people
built an AI radio station in 2026; not one attracted an audience. **That is a demand signal, not a
technology gap.**

The one genuine outlier is **SUB/WAVE at 1,307 stars** — see the teardown.

---

## 2. Adjacent categories, and the honest substitute

### The honest substitute is a playlist plus a podcast app

This must lead, because it is what the target user — including the developer — actually does today,
and it is **free, installed, reliable, offline-capable, and CarPlay-native**. The friction the
concept removes is: *reaching over at a red light to switch app*. Against that:

- Spotify iOS: **n = 41,965,524**. Apple Podcasts: **n = 2,073,804**. Overcast: **n = 47,701**.
- Spotify's own AI DJ page invites the user to "Tell Spotify what you want to listen to, whether
  it's music, podcasts, or audiobooks" (VERIFIED — `support.spotify.com/us/article/dj/`) — so the
  incumbent already occupies the *voice-request* surface, in one app, on Premium.
- **Counter-evidence on the interleaving premise itself**: in the 294-comment HN thread on Spotify's
  AI DJ, **nobody asked for podcasts mixed with music**. The traffic ran the other way. One
  commenter wanted "Spotify without videos and podcasts"; another complained Spotify has spent years
  pushing podcasts and audiobooks at them (SECONDARY — HN item 47385272). The parallel Reddit /
  Spotify-Ideas sweep found the same asymmetry, and much harder: ideas asking to **separate**
  podcasts from music score **500 / 463 / 443 votes**, while ideas asking to **interleave** them
  score **12 and 10**. Three separate Spotify ideas request the ability to *disable* AI DJ features;
  no user anywhere asked for AI DJ intros.

That is the most uncomfortable finding in this ticket and it is not a competitor finding — it is a
demand finding that arrived through the competitive sweep. It belongs in ticket 09's evidence base.

### Broadcast radio and satellite radio

The job "fill my commute with music and talk without touching the phone" is **already solved, for
free, by FM radio**, and for a fee by SiriusXM (**n = 3,075,988**). The only thing broadcast radio
lacks is personalisation. Note the direction of the trade: a listener gets professionally-produced
talk, licensed music, and zero setup, and gives up *their* liked songs. There is no evidence in this
ticket that they want to make the reverse trade.

The corroborating volume signal is stark: the highest-engagement community post found anywhere in
this research was **Non Stop Radio in r/AndroidAuto — 174 and 160 upvotes, 151 and 107 comments**
(SECONDARY, via a redlib mirror; counts not cross-checked against reddit.com). It is a *tuner* over
60,000 existing streams — free, ad-free, account-free. **The car-audio audience turns out in force
for reliable playback of things that already exist, not for programmed personalisation.**

### AI-generated talk products (no music)

InstaPodz (**n = 156**), Curia, Noa (**n = 371**), NotebookLM Audio Overviews. These have solved the
*talk* half and have not attempted music. This is the licensing wall visible from the other side.

### Aggregator / queue / power-listener clients

Plexamp (**n = 10,054**), Marvis Pro ($9.99, **n = 1,270**), Doppler (**n = 1,514**), Longplay
(**n = 165**), Sleeve (**n = 9**), Snipd (**n = 1,142**), Podurama (**n = 397**). **None programme a
stream.** Marvis Pro is the most useful comparable in this entire document: a well-regarded **paid
$9.99 Apple Music power-user client** has **1,270 ratings**. That is the realistic size of the
"sophisticated listener who pays for a better client" segment on iOS.

Two candidates could not be confirmed to exist as audio apps: **Curio** and **Recast** — the App
Store hits under those names are an unrelated antique identifier and a file compressor. **Do not
cite Curio or Recast as living competitors on the strength of this research.**

### CarPlay-focused apps — and a build risk found here

One incidental but load-bearing finding: **CarPlay forbids custom radio-style UI.** SECONDARY —
r/CarPlay, u/unndunn (3 upvotes): *"Apple has very strict UI guidelines for CarPlay apps, and that
sort of UI is neither allowed nor supported."* Low volume, but it is a platform-constraint claim that
should be verified against Apple's CarPlay App Programming Guide before the concept's headline use
case is costed. **The commute is the platform where the concept has the least design freedom.**

---

## 3. The graveyard

The prior project found **3 of 4 dedicated competitors dead and nobody noticed.** *The same pattern
holds, at larger scale.* Twelve-plus attempts across twelve years; the dominant cause of death is
**absorption or silence**, not a technology failure.

### The three closest analogues are all dead, and two were killed by Apple

| Product | What it was | Fate | Cause | Evidence |
|---|---|---|---|---|
| **Scout FM** | Apple's own words: an app that made "radio-like stations for podcasts", curating "based on your listening history", AI that "drops content you skip". iOS + **CarPlay** + Android + Alexa. | **Acquired by Apple 2020; Apple "shut it down on all platforms shortly after the deal was made."** | Acquisition → shutdown | VERIFIED (Apple acquisition list, Wikipedia); SECONDARY (Engadget, 2020-09-25). HN coverage: **1 point, 0 comments.** |
| **Swell** | Personalised talk radio — a continuous stream of news/talk shaped by listening behaviour | **Acquired by Apple 2014-06-29 for $30M.** Apple "generally discontinues the existing businesses of acquired companies." | Acquisition → shutdown | VERIFIED (Wikipedia list of Apple M&A). HN: **0 stories.** |
| **AudioOne** (`audioone.fyi`) | Self-described: "a personal radio platform that delivers news, podcasts, music" — **i.e. this exact concept** | **Dead. Domain is NXDOMAIN.** Last Wayback capture 2024-11-22; 402s from Feb 2025. | Silent demand failure | VERIFIED (DNS resolution failure; Wayback CDX). HN Show HN 2022: **59 pts, 27 comments** |

The Scout FM datum deserves emphasis: **the single closest prior product to this concept was bought
by Apple and switched off**, and Hacker News noticed it with **one upvote**. It had shipped the
CarPlay integration this concept depends on.

The AudioOne datum deserves equal emphasis. It is the **only product ever found that explicitly
promised news + podcasts + music in one personal stream**. It got a respectable Show HN (59 points),
and then died quietly. Reading its HN thread (SECONDARY — HN 32366642) is instructive for what is
*absent*: essentially no demand enthusiasm. The thread's energy went to a broken back button.
Representative: *"I'm not terribly interested in podcasts but I do love seeing what people create."*

### The wider graveyard

| Product | Years | Fate / cause |
|---|---|---|
| **Spotify Stations** (standalone personalised-radio app) | 2018–2022 | Shut down **16 May 2022**, folded into main app (VERIFIED — Wikipedia) |
| **Spotify "Your Daily Drive"** — music + WSJ/NPR/PRI news podcast segments interleaved; *the closest thing an incumbent ever shipped* | Launched **June 2019** | **Both known playlist IDs now return HTTP 404** (VERIFIED — 2026-09-08). No shutdown announcement exists; describe it as **unpromoted and undocumented since 2021, and now unreachable** — not as formally killed |
| **Spotify "Music + Talk" / Anchor** (creators mix *full* Spotify tracks with talk) | 2020–~2024 | Status **could not be verified** — every Spotify support URL tried returned 404 and search engines were unusable. **NO EVIDENCE FOUND either way.** Do not assert it is dead |
| **Otto Radio** — personalised news+podcast radio | 2016–2018 | Gone from the App Store (VERIFIED — iTunes lookup returns nothing). Cause **inferred, not documented**: no shutdown post exists. Funding total **unknowable** — Radio World issued a correction saying the amount is not public; **any specific figure you see is fabricated** |
| **Rivet News Radio** — personalised news radio | 2013–2016 | Consumer app gone; company pivoted to enterprise. Founder, mid-2015: consumer was *"much bigger, but it's going to take much longer"* while 80% of revenue came from enterprise hold-music. Never raised the Series A to fund "much longer" (SECONDARY) |
| **60dB** — personalised short-form audio | 2016–2017 | Acquired/absorbed (SECONDARY) |
| **Curio** (narrated journalism) | 2016–2025 | Shut down Jun 2025. Founders, in their own words: *"in an AI-driven future, narrated journalism alone could not be sustainable long term… the deep pockets of big tech have made it increasingly hard for an independent company to compete — especially if we are to continue paying fairly for the journalism we use."* (SECONDARY) |
| **Huxe** | 2024–2026 | Killed **one day after Spotify shipped the same thing** (SECONDARY) |
| **Radiant – "AI Radio Presenter"** | 2020–? | **Gone from the App Store** (VERIFIED — iTunes lookup id=1476163061 returns resultCount 0). HN Show HN: **12 pts, 3 comments** |
| **"Show HN: AI DJ for Apple Music"** (TestFlight, Feb 2025) | 2025 | **TestFlight beta closed** — "This beta isn't accepting any new testers right now." HN: **1 point, 0 comments** (VERIFIED) |
| **WFH.FM** | — | Best Product Hunt result in the category (**143 upvotes**) — **domain no longer resolves** (SECONDARY) |
| Songza, Rdio, Grooveshark, Beats Music, Google Play Music, Google Podcasts, Radio.com, Slacker, Moodagent, Entale, Umano | 2009–2024 | Acquired, consolidated, or silent. Grooveshark died of **content-rights economics** outright |
| **Pandora "Stories"** | 2019– | **NOT dead** — survives as **Curated Programs**, updated Feb 2026 (SECONDARY). Correct any assumption otherwise; overclaiming this would undermine the analysis |

### The Auddia case — the most instructive datum in this ticket

All figures **VERIFIED** from SEC filings (primary source; sec.gov, CIK 0001554818).

Auddia Inc. (NASDAQ: **AUUD**), Boulder CO, founded 2012 as Clip Interactive. Its product **faidr**
is the closest thing to a *funded, public-market* attempt at this concept: an app that streams AM/FM
radio, uses AI to detect the boundaries between commercials, songs and DJ talk, and **replaces ad
breaks in real time with songs** — plus podcasts with ad-skipping. Its own 10-K claims: *"the faidr
app represents the first-time consumers can combine the local content uniquely provided by AM/FM
radio with commercial-free and personalized listening… No other audio streaming app…"*

That is a company that solved the **audio-segmentation problem this concept needs** (the "pause at a
suitable moment" step — see map.md's *Not yet specified*), shipped it, and listed on Nasdaq.

The results:

- **Revenue, FY2025: $0. Revenue, FY2024: $0.** (10-K filed 2026-03-06: *"Total revenues for the
  years ended December 31, 2025 and 2024 were $0"*.)
- **Revenue, H1 2026: $7,584** — and that came from the *B2B* pilot, not consumers (10-Q filed
  2026-08-14).
- **Net loss FY2025: $7,693,197.** Net loss H1 2026: $5,256,819.
- **faidr App Store volume after years of paid marketing** ($907,411 of sales & marketing in H1 2026
  alone): **n = 687 ratings.**
- **Going concern:** cash "will only be sufficient to fund our current operating plans into the
  second quarter of 2027."
- **The pivot, in their own words:** *"Auddia is evolving its business model from
  direct-to-consumer to business-to-business, shifting its focus from individual radio-streaming
  subscribers to artists and labels as subscribers."*
- **And the company is being merged** into Thramann Holdings — a holding company controlling three
  unrelated AI startups founded by Auddia's own CEO (Agreement and Plan of Merger dated 2026-02-17).
  This is a company exiting the consumer-audio business.

**Two things this establishes that no indie datapoint can:**

1. **The consumer demand is not there at any spend level a solo developer can reach.** $907k of
   marketing in six months bought 687 lifetime ratings and $0 of consumer revenue. This corroborates
   the prior project's paid-acquisition arithmetic from a completely independent direction.
2. **The legal theory is named, and it is fragile.** Auddia's own risk factors: *"Our subscription
   revenue margins and our freedom to operate our faidr radio platform rely on continuity of the
   established music licensing framework… Our faidr platform will rely on the established 'personal
   use exemption' which allows individuals to record content for time-shifting purposes."* And:
   *"Changes in licensing costs and general rights to play music content could impact our direct
   costs for content or even prohibit access to content that is fundamental to the platform."*

A public company's audited disclosure that its entire freedom to operate rests on the **personal-use
time-shifting exemption** is the clearest available statement of where the wall is. It should be
handed to tickets 01–05 as prior art.

### What the graveyard means

- **Nobody died of a technology problem.** Causes, by frequency: **acquisition/absorption** (Swell,
  Scout FM, Songza, Beats, 60dB, Slacker); **consolidation into a parent app** (Stations, Anchor,
  Google Play Music, Google Podcasts); **silent demand failure** (AudioOne, Otto Radio, Rivet's
  consumer app, Radiant, Moodagent, Entale); **content-rights economics** (Grooveshark, Curio
  explicitly, Music+Talk implicitly); **killed by an incumbent feature** (Huxe, one day after).
- **The pattern from the prior project holds and intensifies.** Of the three closest analogues, all
  three are dead: two bought and switched off by Apple, one a dead domain. HN noticed the Apple
  acquisition of the closest one with **1 upvote**.
- **The surviving shape is synthetic, not curated.** The talk-between-songs problem got solved by
  *generating* the talk, which routes around the licensing and creator-supply failures that killed
  everything above.

---

## 4. Teardown: the closest living competitor — SUB/WAVE

`github.com/perminder-klair/subwave` · `getsubwave.com` · MIT licence · by Perminder Singh Klair

This is the closest living competitor **on product instinct**, not on market position. It is worth
studying and it is not worth fearing.

### What it does (VERIFIED — project site, README, App Store description)

One **Icecast stream** that all listeners hear simultaneously. An LLM DJ picks the next track, writes
and voices an intro over TTS, and reads station IDs, weather, news headlines and time checks between
songs. Its own copy: *"Remember radio? One signal, everyone hearing the same song at the same
moment."* And: *"One stream. No skip. It's radio, not a playlist."*

Architecture: Controller → DJ Brain (LLM) → Liquidsoap → Icecast. `docker compose up -d`. Up to 24 DJ
personas, scheduled shows, plain-language track requests, an MCP server, admin console. TTS via
Piper / Kokoro / Chatterbox / PocketTTS / OpenAI / ElevenLabs. Between-track segments are pluggable
"skills" (weather, news, traffic, your own) with editable briefs. **CarPlay is supported** and
background playback works.

### The three things that disqualify it as a threat

1. **It has no podcasts.** Its non-music audio is *synthesised* DJ talk and news headlines. It does
   not play the user's followed podcast episodes. The core mechanic of this concept — pause an
   episode at a sensible boundary, play liked songs, resume — is absent.
2. **It does not touch a streaming service.** Music comes from **the user's own library** via the
   Subsonic API (Navidrome, Airsonic, Funkwhale, Nextcloud Music). Spotify and Apple Music are not
   supported and not mentioned. It solves the licensing problem by **requiring the user to have
   already solved it.**
3. **It requires a Docker host.** Its market is self-hosters, which is a different and much smaller
   population than commuters.

### Cost — and yes, it gives the core feature away free

**Plainly: the entire core product is free and MIT-licensed.** The site states *"SUB/WAVE is not a
cloud service"*, *"No subscriptions, no round-trip to a data center"*, and *"No licensing fees"* on
music you already own. A local LLM needs *"no API key."*

A "DJ Brain from £5/month" hosted tier was reported by one reading of the GitHub page (SECONDARY,
single source) but **is absent from the project site, which carries no pricing at all**, and
`docs.getsubwave.com` does not resolve. **Treat SUB/WAVE Hosted pricing as NO EVIDENCE FOUND.**

### Volume and trend (VERIFIED)

| Metric | Value | Source |
|---|---|---|
| GitHub stars | **1,307** | GitHub API, 2026-09-08 |
| Forks / open issues | 117 / 39 | GitHub API |
| Repo created | **2026-05-11** | GitHub API |
| Last push | **2026-09-07** (yesterday) | GitHub API |
| Commits | 1,716 on `develop` | GitHub |
| **iOS App Store ratings** | **n = 4**, avg 5.0 | iTunes lookup id 6778786696 |
| iOS release / last update | 2026-06-13 / 2026-08-26 | iTunes lookup |
| Android installs | ~1,000 | SECONDARY |
| HN Show HN (2026-06-25) | **2 points, 0 comments** | HN Algolia |

**Trend:** development velocity is genuinely high — 1,716 commits in four months, pushed yesterday,
active Discord, mobile and desktop apps, published GHCR images. **Distribution is nil.** 1,307
GitHub stars converted to **4 iOS ratings** and ~1,000 Android installs. It is a self-hoster's toy
built by someone who ships fast.

### What its users praise, complain about, and ask for

**NO EVIDENCE FOUND — and this is a real gap in this ticket.** Apple's customer-review RSS returned
**empty feeds for every app checked** (SUB/WAVE, musen, AITUNE, doremiradio, both US and GB
storefronts, VERIFIED by direct query). Reddit and Product Hunt access was blocked or CAPTCHA-walled
throughout. **No user review text was obtained for any app in this document.** Every "users say…"
claim is therefore unavailable, including for SUB/WAVE. If review sentiment matters to the decision,
that work is outstanding.

### The one competitor document worth reading in full

Not SUB/WAVE — **InstaPodz** (n = 156, the largest genuinely-relevant indie). Its founder published
*"What we learned after 6 months of building an AI podcast app nobody used"* (SECONDARY,
self-reported). Two failure modes:

- *"people don't want to decide what to make every day"* — even a 30-second creation flow requires
  the user to arrive with a topic. Users *"didn't want to figure out the specifics every day"*; they
  arrived with a mood.
- The social/discovery layer was redundant because *"nobody wants to spend their commute on content
  they don't fully trust."*

They pivoted from creation to a configure-once **Daily Show** and report *"In the first week, usage
went up 3x. By week three, it was 6x."* — **multiples with no disclosed baseline over three weeks;
that is not evidence of retention.** The reusable lesson is the zero-decision, press-play,
configure-once shape — which is **exactly SUB/WAVE's no-skip thesis, arrived at independently**, and
exactly this concept's shape. Three independent teams converging on "remove the daily decision" is
the most encouraging signal in this ticket. It is a signal about *product form*, not about demand.
