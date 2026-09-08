# 07 — Incumbent-feature teardown: do the platforms already ship this?

Resolves `issues/07-incumbent-features.md`. Researched fresh except where marked *carried over*
from `podcasts-notes-validation`.

**Evidence tags:** VERIFIED = primary/official source with URL. SECONDARY = press, forums,
community boards, Wikipedia — acceptable for product history and user sentiment, labelled as such.
NO EVIDENCE FOUND = searched, nothing found; not a synonym for "no".

**Method caveat, stated up front because it bounds the confidence of everything below.** The
`WebSearch` tool failed with a hard API validation error on every invocation across this session
(both at the top level and in two of four research agents). DuckDuckGo (HTML and Lite), Brave,
Mojeek, Ecosia, Startpage and two SearXNG instances all returned CAPTCHAs or 403s. Findings were
therefore obtained by fetching **directly-addressed official pages** — `developer.spotify.com`,
`newsroom.spotify.com`, `support.spotify.com`, `community.spotify.com`, `developer.apple.com`,
`developers.google.com`, `developer.amazon.com` — plus Wikipedia for product history. This method
is *strong* on official documentation and contract clauses (the most decision-relevant material)
and *weak* on press coverage and Reddit sentiment. Reddit was inaccessible throughout: the absence
of Reddit quotes below is a tooling limitation, not evidence of absent sentiment.

---

## Summary table

| # | Feature | Ships AI voice commentary? | Ships music+talk interleaved? | Open to third-party devs? | Verdict vs the concept |
|---|---|---|---|---|---|
| 1 | **Spotify DJ** | **YES — free-of-extra-charge in Premium, 6 languages, 75+ markets, actively expanding through May 2026** | No (music only; can be *asked* for podcasts, does not blend them) | **No** — closed first-party surface | **Occupies step 5 outright** |
| 2a | Spotify daylist / Discover Weekly / Blend | No | No | Playlist reads blocked for new apps since Nov 2024 | Occupies step 2's personalisation, not the concept |
| 2b | **Spotify "Your Daily Drive"** | No (real journalists, not AI) | **YES — shipped 2019, news podcast segments between songs** | No | **Occupied steps 3+4 for ~7 years, then was quietly retired** |
| 2c | Spotify "Music + Talk" | No | Yes (creator-authored) | No | Status unconfirmed; both support URLs 404 |
| 3 | Apple Music Radio / stations / AutoMix | **No — AutoMix mixes audio, never speaks** | **No — MusicKit has no podcast type at all** | **Yes, unusually open** (MusicKit playback, library, recently-played, crossfade API) | Structurally uninterested in talk; the most permissive dev surface |
| 4 | YouTube Music mixes + **"Beyond the Beat"** | **YES — AI commentary between tracks (US Labs experiment)** | Hosts podcasts, but no evidence of auto-interleaving | **No** — policy-blocked and API-absent | Occupies step 5 in embryo; structurally closed as a host |
| 5 | Tidal My Mix | NO EVIDENCE FOUND (none found) | No evidence of podcasts | **Yes — the only documented full-track playback SDK** | Provisionally not a threat; see §5 |
| 6 | **Alexa Flash Briefing + music** | No | **YES, via Routines — closest shipped analogue to the whole concept** | Flash Briefing skills: yes. Music skills: partner-gated | **Occupies steps 3+4 as a voice-first experience** |
| 7 | Historic attempts | — | Several | — | See §7 — the pattern is the finding |

**Headline finding.** Step 5 (AI-generated intros between segments) is **occupied and free**:
Spotify DJ has shipped AI voice commentary over personalised music since 2023 and Spotify was
still expanding it in **May 2026** — four new languages, 75+ markets, 94M subscribers served. It is
not a stalled experiment. Separately, steps 3 and 4 (podcast/news interleaved with music) were
**already shipped by Spotify itself** as *Your Daily Drive* from 2019 — and then retired without
announcement. That combination is the most important result in this ticket: the concept's
differentiating step is occupied by an incumbent, and the concept's *core mechanic* was shipped by
that same incumbent and withdrawn. Both halves are bad news, for different reasons.

---

## 1. Spotify DJ — the concept's step 5, already shipped and free

### What it does

- **VERIFIED** Spotify's own support page describes DJ as "your personal music companion. It plays
  favorites you already love and introduces new genres, playlists, and artists based on your taste,"
  and says you can "type or talk to shape your listening."
  <https://support.spotify.com/us/article/dj/>
- **VERIFIED** Tier: the support page is explicitly labelled "**For: Premium**". So it is not free
  as in free-tier — but it is **free of any incremental charge** to the ~700M-user Premium base.
  For competitive purposes that is the relevant sense: a user already paying for Spotify pays $0
  more to get AI voice commentary over their own music. <https://support.spotify.com/us/article/dj/>
- **VERIFIED** Podcasts *are* addressable by voice request: "Tell Spotify what you want to listen
  to, whether it's music, podcasts, or audiobooks," including queries like "What other podcasts has
  this guest been on?" **But this is request-routing, not interleaving** — DJ does not programme a
  blended music+podcast session. <https://support.spotify.com/us/article/dj/>
- **VERIFIED** News: **nothing on the DJ support page mentions news content.**
  <https://support.spotify.com/us/article/dj/>
- **VERIFIED** Language control exists as a user setting (three-dot menu → "DJ language"), and DJ
  "is currently available in a limited number of languages."
  <https://support.spotify.com/us/article/dj/>

### Availability and trajectory — DJ is expanding, not decaying

This matters: a feature being sunset is a different competitive fact from a feature being invested in.

- **VERIFIED** May 2025: voice requests ("Talk to DJ"), 60+ markets.
  <https://newsroom.spotify.com/2025-05-13/dj-voice-requests/>
- **VERIFIED** Sept 2025: "Talk to DJ" reaches **Android Auto** — i.e. Spotify is deliberately
  targeting the *commuting* context, which is this concept's primary use case. (Noted as a Sept 23
  2025 update in the above article.)
- **VERIFIED** Oct 2025: Spanish-language requests, **text requests**, personalised prompts.
  <https://newsroom.spotify.com/2025-10-15/dj-spanish-text-requests-update/>
- **VERIFIED** May 2026: **four new languages** (French, German, Italian, Brazilian Portuguese),
  new markets, **"over 75 markets"**, 94M subscribers served.
  <https://newsroom.spotify.com/2026-05-07/dj-expansion-4-new-languages/>
- **VERIFIED** Spotify's own engagement claims: "DJ listener engagement has nearly doubled over the
  past year" (May 2025); after voice requests shipped, "DJ listening has increased" (Oct 2025).
  Treat as vendor-reported, directionally useful, not independently audited.
- **Counter-signal on maturity — VERIFIED:** DJ is *still branded beta* three years after launch
  (as of May 2026), and still has **no commentary on/off toggle**; the oldest open request for one
  (March 2023) remains an unresolved "Live Idea" on Spotify's own board. So the feature is real and
  expanding but not polished, and Spotify has left an obvious usability gap open for three years.
- **NO EVIDENCE FOUND** The full 75-market list — Spotify has never published it. Also
  NO EVIDENCE FOUND for CarPlay support or web-player support specifically. Desktop support is
  **SECONDARY and ambiguous** (a community thread title only).
- **VERIFIED absent** Offline DJ. The corresponding feature idea was closed on a technicality.

### Conflicting primary sources — flagged, not resolved

- **CONFLICTING PRIMARY SOURCES on DJ + podcasts/audiobooks.** The support page (above) says you can
  ask DJ for podcasts and audiobooks. The May 2025 newsroom post describing voice requests does not
  support that scope. I could not reconcile these and am not going to pick a winner. Practical read:
  even on the most generous reading, DJ *fetches* podcasts on request; on no reading does it
  *interleave* them with music automatically.

### Third-party developer access to DJ — VERIFIED: none

This is the decisive question for whether the concept can ride DJ rather than compete with it.

- **VERIFIED** No DJ, DJ-commentary, DJ-voice, or AI/prompted-playlist endpoint exists anywhere in
  the Spotify Web API reference. The documented surface is catalog / library / player / playlists /
  search / users. <https://developer.spotify.com/documentation/web-api>
- **VERIFIED** None of Spotify's other developer products — Web Playback SDK, Ads API, iOS SDK,
  Android SDK, Embeds, Commercial Hardware, Open Access, Spotify Soloist — is described as a DJ or
  AI interface. <https://developer.spotify.com/documentation/web-api>
- **VERIFIED, and materially worse than "no DJ API":** `Get Recommendations` is now flagged
  **Deprecated** — so are `available_markets`, `linked_from`, `popularity`, `preview_url`, and
  several playlist-item endpoints. The tunable-attribute approach (`target_energy`, `min_tempo`, …)
  depends on the same audio-features signals.
  <https://developer.spotify.com/documentation/web-api/reference/get-recommendations>
- **VERIFIED, blocking for the AI layer:** Spotify's policy states its content "may not be used to
  train machine learning or AI model," and preview clips "may not be offered as a standalone service
  or product."
- **VERIFIED** The closest externally-reachable DJ-adjacent thing is Spotify's **Claude integration**
  (Apr 23 2026): personalised music *and podcast* recommendations, save/preview/play, Spotify Connect
  device control, global, Free and Premium. But it exposes **no DJ commentary**, and there is no
  mention of MCP, an open protocol, or third-party developer access — it is a first-party partner
  deal. <https://newsroom.spotify.com/2026-04-23/claude-integration/>

**Conclusion on DJ: the differentiator is occupied.** AI voice commentary over personalised music is
shipped, actively invested in, available at no extra cost to Premium subscribers in 75+ markets and
6 languages, and available on Android Auto in the car. It is closed to third parties, so it cannot
be built on — only competed with. The concept's step 5 is not a differentiator against Spotify; it
is table stakes that Spotify already provides and the concept would have to *match* at its own cost.

### User sentiment — the honest picture

**Tooling limitation, stated plainly:** Reddit (r/spotify, r/truespotify) was inaccessible for the
entire session. All sentiment below is from `community.spotify.com`, Spotify's own board. That is a
biased sample — idea boards over-represent complainers and under-represent satisfied silent users.
**Per-idea vote counts: NO EVIDENCE FOUND** — do not cite vote numbers for these.

The most substantive sentiment finding is not about DJ directly but about what DJ *failed to
replace* — see §2b.

---

## 2. Spotify's other personalisation — and the retired feature that *was* this concept

### 2a. daylist, Discover Weekly, Blend, Made For You

These occupy the concept's **step 2** (personalised selection from listening history) but not steps
3–5. They are music-only. Their competitive significance is narrower than it first appears: they
prove Spotify is good at the personalisation the concept treats as its foundation, but they do not
blend talk.

- **VERIFIED, and a hard constraint for the concept:** since **27 Nov 2024** Spotify has withdrawn
  API access for **new** applications to algorithmic playlists *and* Spotify-editorial playlists,
  the Recommendations endpoint, and Audio Features. A new third-party app therefore **cannot read
  daylist, Discover Weekly, or Blend at all**. <https://developer.spotify.com/documentation/web-api>
  This is not merely a competitive fact — it removes the concept's most obvious shortcut to
  personalisation. Carried-over context: the platform tickets (01) cover the full clause set.

### 2b. "Your Daily Drive" — steps 3 and 4, shipped by Spotify in 2019, then killed

**This is the single most decision-relevant finding in the ticket, and it cuts against the concept
in a way that is easy to miss.** Spotify did not fail to think of interleaving news with music. It
built it, ran it for years, and withdrew it.

- **VERIFIED (what it was), SECONDARY source for the description:** launched **June 2019**; it
  "mirrors the 'drive time' style of conventional radio, mixing brief news podcast segments from
  **The Wall Street Journal, NPR, and PRI** with familiar favorites plus unfamiliar tracks," placed
  in "Made For You" and refreshing through the day. <https://en.wikipedia.org/wiki/Spotify>
  Note this is **almost exactly the concept's steps 2, 3 and 4** — personalised music, interleaved
  talk segments, news specifically, in a drive-time format, aimed at commuters.
- **VERIFIED by absence:** `https://support.spotify.com/us/article/daily-drive/` returns HTTP 200 but
  the page body contains **no mention of Daily Drive at all** — the support article has been gutted.
  There are also **no Daily Drive posts** on `newsroom.spotify.com` (`?s=Daily+Drive` returns no
  matching headlines), and a probed playlist ID 404'd.
- **VERIFIED** Spotify **never announced its retirement.** No sunset post exists. Wikipedia gives no
  discontinuation date for it (although it does give dates for Stations and Car Thing), so Wikipedia
  is stale on this point.
- **VERIFIED** A user idea directly linking the two features — "More Dynamic DJ: Bring Back Daily
  Drive" — was **closed by Spotify on 2026-04-22** on a procedural guideline ("One change request per
  Idea"), not on merit.
  <https://community.spotify.com/t5/Closed-Ideas/More-Dynamic-DJ-Bring-Back-Daily-Drive/idi-p/7414212>
- **Exact shutdown date: NO EVIDENCE FOUND.** Best available bracket: retired before **2026-04-22**.

**SECONDARY — user sentiment, multi-user, verbatim, from Spotify's own board.** These matter because
they are the closest thing available to demand evidence for *this exact concept*, and because they
independently establish that DJ did not absorb the job.
<https://community.spotify.com/t5/Live-Ideas/BRING-BACK-THE-DAILY-DRIVE/idi-p/7378260>

- "The Daily Drive podcast was my favorite part of my day… **It was my main source of news and
  entertainment in the morning.** Please, Spotify, bring it back."
- "**I can't believe it just got pulled!** There's not even anything comparable in the 'Made For You'
  section!... **DJ X isn't smart enough yet to do that for me.** Bring it back!"
- "I want a curated and personalized mix of news podcast episodes and songs to play throughout the
  day that updates regularly. **There is currently no alternative option to this, the AI DJ is not
  useful.**"
- "**I want short news podcasts mixed with music in one playlist. I'm not going to set that up
  manually — it needs to be a one-click, automatic experience.**"
- "I don't want to have to talk to DJ Beta to get some podcasts **I just want to press play on a
  playlist and have a mix.**"
- "I've been a Spotify subscriber for 10 years, Daily Drive (**In France = Mon Daily**) is pretty much
  the only reason I've stayed instead of switching to Deezer!... I want Daily Drive back, or it's
  goodbye, Spotify!!!"
- On voice-assistant addressability: "what should i tell google home to play to get a mix of podcasts
  and music?"
- **Counter-evidence against the concept's own quality ceiling**, from the same thread: users report
  Daily Drive itself suffered repetition — "the same twelve songs ALWAYS played."
- **SECONDARY** The OP of the closed idea: "**It seems there has been a drive towards the use of AI,
  like pushing for DJ X, but at the cost of removing other features like Daily Drive.**"

**How to read this evidence — and it points two ways at once.**

*For the concept:* real users, unprompted, describing this concept in their own words, explicitly
saying no alternative exists, explicitly saying the AI DJ does not substitute, and explicitly
demanding a **one-click automatic** experience rather than manual playlist assembly. One user names
it as their reason for retaining a 10-year subscription. This is genuine, specific, on-point demand
evidence — the strongest this ticket found.

*Against the concept, and this is the heavier side:* Spotify — with the licences, the catalogue, the
recommendation infrastructure, WSJ/NPR/PRI relationships, and no need to ask anyone's permission —
built precisely this, ran it for around seven years, and **killed it without a word.** Companies do
not silently retire features that retain subscribers. The most probable reading is that usage was too
low to justify the editorial and licensing overhead. A solo developer would be rebuilding a feature
that its own platform owner tested at enormous advantage and abandoned, while forbidden by that
platform's terms from rebuilding it (§8). The vocal minority on an idea board is *consistent with*
Spotify's decision, not evidence against it: a feature can be loved by hundreds and still be
uneconomic. **Do not treat the quotes above as validating market size.** They validate that the pain
is real for a small, identifiable group.

### 2c. Spotify "Music + Talk"

- Creator format (launched ~2020 via Anchor) allowing full licensed songs alongside spoken segments.
- **Status: unconfirmed.** Both relevant support URLs 404, it is absent from Wikipedia's Spotify
  article, and **no sunset notice was found**. **NO EVIDENCE FOUND** either way. Do not state it as
  live or as dead. Note the pattern, though: same silent-404 signature as Daily Drive.

### 2d. What replaced them — and what it isn't

- **VERIFIED** **Prompted Playlist for podcasts** (7 Apr 2026) is the nearest successor, but it
  generates **podcast playlists, not music+news interleaved** — the article contains no statement
  confirming mixed music-and-podcast playlists. 7 markets, English only, Premium, beta, with usage
  limits. <https://newsroom.spotify.com/2026-04-07/prompted-playlist-for-podcasts-launch/>
- **SECONDARY, lead only, not fetched:** a 7 May 2026 newsroom item about saving a "Personal Podcast"
  to Spotify surfaced in search. Flagged for follow-up; no claim made on it.

### 2e. Can a third party build the mixed queue via the API?

- **VERIFIED, and genuinely surprising:** the Add-to-Queue endpoint's `uri` parameter states: *"The
  uri of the item to add to the queue. Must be a track or an **episode** uri."*
  <https://developer.spotify.com/documentation/web-api/reference/add-to-queue>
  So **technically, mixed music+podcast queues are an explicitly supported API capability.** The
  concept's step 3 is one API call away — *technically*.
- **VERIFIED, and this is what closes it:** Spotify's Developer Policy prohibits exactly this.
  See §8 for the verbatim clauses. Technical capability and contractual permission diverge here, and
  the contract is the binding constraint. A workaround that violates a clause is not a workaround.

---

## 3. Apple — structurally uninterested in talk, but by far the most open developer surface

### 3a. Apple Music Radio, stations, personalised mixes

- **VERIFIED** MusicKit's framework abstract is "Integrate your app with **Apple Music**"; it wraps
  the **Apple Music API** only. <https://developer.apple.com/documentation/musickit>
- **VERIFIED** `RadioShow` exists as a music-item type — but it is Apple Music radio programming
  (Apple Music 1 / Hits / Country style shows), **not** a podcast feed. Distinguishing these matters:
  Apple has talk *radio shows* inside Apple Music and podcasts entirely outside it.
- **NO EVIDENCE FOUND** for Apple's named personalised mixes as documented types (Station for You,
  New Music Mix, Favorites Mix, Get Up! Mix, Chill Mix). Apple's support-guide article bodies are
  client-rendered and never served over WebFetch; only table-of-contents entries resolved. Their
  existence as consumer features is well known but is **not verified here** — do not cite specifics.

### 3b. Can Apple mix podcasts into music? — VERIFIED NO, by type-system absence

This is a stronger form of evidence than a missing feature announcement.

- **VERIFIED** MusicKit's complete music-item roster contains **no podcast or episode type**, and no
  podcast request or filter type appears in **any** topic section — Catalog Search, Resource Loading,
  Library, Recently Played, Recommendations, or Charts.
  <https://developer.apple.com/documentation/musickit>
- **VERIFIED** There is **no public framework for third-party playback of Apple Podcasts episodes**
  in anything retrieved. This *confirms and extends* the carried-over prior-project finding (Apple
  Podcasts has no playback SDK, `MPNowPlayingInfoCenter` is write-only). New here: it is not merely
  that Apple Podcasts lacks an SDK — Apple **Music's** SDK has no concept of podcast content either.
  The two are architecturally separate with no bridge.

### 3c. AutoMix and crossfade — Apple's AI is in the *transitions*, not the *talk*

Worth dwelling on, because it is the one place Apple beats the concept and the one place Apple
*helps* it.

- **VERIFIED** **AutoMix** (iOS 26, 2025) uses AI time-stretching and beat-matching to blend tracks
  like a DJ, extending to tvOS/HomePod in 2026. **It never speaks.** Apple built the DJ's *mixing
  desk*, not the DJ's *microphone* — the opposite half of the problem from Spotify DJ.
- **VERIFIED, and useful to the concept:** `MusicPlayer.Transition.crossfade(duration:)` is public
  API from **iOS 18+**. It is duration-only — *not* the AI AutoMix — but it means a third-party app
  using MusicKit **can crossfade**. This directly answers the "transition ceiling" concern flagged in
  ticket 05: on Apple, the ceiling is a real crossfade, not a hard cut. Carried-over concern
  partially relieved. <https://developer.apple.com/documentation/musickit>
- **VERIFIED** Apple has shipped **no AI voice host / AI DJ** in Apple Music in anything retrieved.
  NO EVIDENCE FOUND for any Apple Intelligence music-hosting feature.

### 3d. MusicKit developer access — the open door

- **VERIFIED** MusicKit's documented flow: capabilities check → `MusicCatalogSearchRequest` /
  `MusicCatalogResourceRequest` → player → optionally `MusicSubscriptionOffer` for non-subscribers.
  Token management via `MusicTokenProvider` / `MusicDeveloperTokenProvider` /
  `MusicUserTokenProvider`. <https://developer.apple.com/documentation/musickit>
- **VERIFIED** The personalisation layer is **public**, in direct contrast to Spotify:
  `MusicPersonalRecommendationsRequest`, `MusicRecentlyPlayedRequest`, and `MusicLibraryRequest` are
  all available. Where Spotify **closed** algorithmic-playlist and recommendation access to new apps
  in Nov 2024, Apple leaves recommendations, recently-played, and library open.
- **NO EVIDENCE FOUND** for any Apple clause restricting the combination of Apple Music content with
  third-party audio, or restricting radio-style presentation. The Apple Music API reference page body
  did not resolve over WebFetch, and the Developer Program License Agreement PDF 404'd at the URL
  tried. **This is a genuine gap, and an important one** — absence of a found restriction is *not*
  evidence of permission. Ticket 02 must resolve it against the actual PDF and the MusicKit ToS;
  it is the single most valuable open question left by this ticket.

**Apple's position, stated plainly: structurally uninterested in the concept's talk layer, and the
most permissive host for it.** Apple ships no AI voice host and cannot represent a podcast at all in
MusicKit, so it does not occupy steps 3, 4, or 5 and shows no sign of moving there. It is **not one
API call away** — adding podcasts to MusicKit would require a new content type across the framework.
Meanwhile it offers what Spotify withdrew: open recommendations, open recently-played, open library,
and a real crossfade API. **On the incumbent-threat axis specifically, Apple Music is the strongest
platform for this concept and Spotify is the weakest** — the reverse of the map's stated ranking
preference. That ranking should be revisited in ticket 05.

---

## 4. YouTube Music — closed, and the closure is redundant

Kept proportionate, as the platform ticket (03) already found this redundantly dead. **Carried over
and re-verified against the current policy text**, which is new work: the clauses are still live and
still worded as the prior project recorded them.

- **VERIFIED** III.I.7 — you must not "separate, isolate, or modify the audio or video components of
  any YouTube audiovisual content" made available via the API Services.
- **VERIFIED** III.I.8 — you must not "promote separately the audio or video components of any
  YouTube audiovisual content."
- **VERIFIED** III.I.9 — you must not "create, include, or promote features that play content,
  including audio or video components, from a background player," a background player being one
  "that is not displayed in the page, tab, or screen that the user is viewing."
- **VERIFIED** III.I.14 — you must not "use any technology other than YouTube API Services to access
  or retrieve API Data," extending to any part of YouTube audiovisual content.
- **VERIFIED** III.E.1 — bars downloading, caching, or storing copies, and separately bars efforts to
  "make content available for offline playback."
- All of the above: <https://developers.google.com/youtube/terms/developer-policies>

The concept requires audio-only playback (III.I.7, III.I.8), background playback while driving
(III.I.9), and buffering ahead on a poor connection (III.E.1). **Three independent named
prohibitions, each individually fatal.**

### 4a. "Beyond the Beat" — the closest thing to step 5 outside Spotify

- **SECONDARY** YouTube Music ships **"Beyond the Beat"**, which inserts **AI voice commentary
  between tracks** over radio and mixes — trivia, stories, and context about the music. This is the
  closest analogue to the concept's step 5 found on any platform other than Spotify. Two important
  qualifications: the commentary is **music-about-music**, not news or podcast content; and it is a
  limited **US Labs experiment** that may be "disabled without notice."
- **NO EVIDENCE FOUND** for YouTube Music automatically interleaving **podcast episodes** with music.
  YouTube Music does host podcasts (since 2023), and hosting both verticals in one app is not the same
  as blending them in one stream. The question is **unresolved in both directions** — though moot,
  since the platform is closed to third parties regardless.
- **NO EVIDENCE FOUND** for YouTube Music mix mechanics (My Mix / Supermix / Discover Mix / New
  Release Mix) as primary-sourced facts — no support article was retrievable. Do not cite specifics.

**Verdict: structurally closed, not structurally uninterested.** Whether YouTube occupies the
differentiator is irrelevant — a third party cannot build here at all.

---

## 5. Tidal — not a feature threat, and the one real third-party playback path

- **VERIFIED by absence, weak form** No Tidal AI DJ or AI voice-host feature was found on a
  recently-updated official surface. Tagged weak because inferring absence from official silence is
  softer than a positive denial.
- **NO EVIDENCE FOUND** — whether Tidal carries podcasts at all; My Mix / Daily Discovery mechanics
  as primary-sourced facts; and current tier structure (`tidal.com/pricing` returned 403).
- **VERIFIED, and the most consequential correction in this ticket:** `@tidal-music/player`
  officially supports `AssetPresentation.FULL`, online streaming, and entitlement-checked playback.
  **Tidal is the only one of the four platforms with a documented third-party path to playing full
  tracks in your own app.** The prior assumption that no platform offers this is wrong.
- **The open question is licensing, not capability.** Tidal's developer terms could not be read (the
  portal is client-rendered and served no body to WebFetch), so whether the licence permits a
  *commercial* third-party app to stream full tracks — and at what listener tier — is unresolved.
  Ticket 04 owns this; it should be treated as that ticket's highest-priority item.

**Verdict on the incumbent-threat axis: Tidal is provisionally not a threat** — no AI voice
commentary, no evidence of music+talk blending. Ticket 04 also owns the more decisive commercial
point: a permissive-but-small platform may be technically feasible and commercially pointless.

---

## 6. Amazon Music / Alexa — the closest shipped analogue to the *whole* concept

The ticket flagged this as deserving serious treatment, and that judgement was correct: the
"news briefing then my music" experience is **shipped, mainstream, and voice-first**, which is the
concept's own primary context (hands-free while commuting).

### 6a. Flash Briefing — what it actually is

- **VERIFIED** A Flash Briefing skill can "provide Alexa customers with news headlines and other
  short content"; a Flash Briefing "provides a quick overview of news and other content such as
  comedy, interviews, and lists."
  <https://developer.amazon.com/en-US/docs/alexa/flashbriefing/understand-the-flash-briefing-skill-api.html>
- **VERIFIED** It is alive and documented as of this research. The developer documentation is live and
  current, which is meaningful given Amazon's Alexa+ LLM transition. **NO EVIDENCE FOUND** on whether
  Alexa+ changes or deprecates Flash Briefing — a real gap, and worth resolving before relying on it.
- **VERIFIED, requirements a developer must meet:** ownership or distribution rights to "original text
  or audio content that updates frequently," plus HTTPS RSS and/or JSON feeds. "You can make updates
  to your content as often as every 15 minutes, but must update it at least daily."

### 6b. The route-or-competitor question — answered: **mostly competitor**

This is the question the ticket asked, and the answer is unfavourable.

- **VERIFIED, and it kills the route:** Flash Briefing content "should not exceed 10 minutes in
  length" and must run at least ten seconds; audio "must contain a HTTPS URL to audio content" and
  "should be 256kbps mono or stereo MP3," normalised to −14 dB LUFS average, true peak ≤ −2 dBFS.
  <https://developer.amazon.com/en-US/docs/alexa/flashbriefing/flash-briefing-skill-api-feed-reference.html>
  A **10-minute cap on a pre-rendered MP3 feed** cannot express a continuous, dynamically-programmed,
  session-length radio stream. Flash Briefing is a short-content delivery format, not a radio engine.
- **VERIFIED** The generic `AudioPlayer` interface for custom skills does allow HTTPS streaming with
  playback-progress requests, one `Play` directive per request, optional captions and metadata.
  <https://developer.amazon.com/en-US/docs/alexa/custom-skills/audioplayer-interface-reference.html>
  So the *technical* means to stream a long session through a custom skill exists.
- **VERIFIED, and this is the binding constraint:** Amazon's skill policy rejects any skill that
  "Infringes the intellectual property rights (including copyright, trademark and publicity rights)
  of a third party," and requires you to "clearly indicate that you have permission to use these, and
  provide proof" at certification, with evidence such as a rights-holder licence.
  <https://developer.amazon.com/en-US/docs/alexa/custom-skills/policy-requirements-for-an-alexa-skill.html>
  A skill streaming commercial music therefore requires the developer to hold and *prove* music
  rights at certification. That is the licensing problem from ticket 11, relocated — not avoided.
- **VERIFIED, corroborating that Amazon treats music skills as a distinct privileged class:** the
  policy carves out "Streaming music, streaming radio, podcast, and flash briefing skills" for
  audio advertising, and Amazon documents a separate pre-built **"Music, Radio, and Podcast Skills"**
  category including a **Radio Skills Kit**.
  <https://developer.amazon.com/en-US/docs/alexa/ask-overviews/understanding-the-different-types-of-skills.html>
- **NO EVIDENCE FOUND** — whether the Music Skill API / Radio Skills Kit is open self-serve or
  restricted to invited partners. Every eligibility page tried returned 404 and Amazon's consumer help
  pages returned 503. **This is the most important unresolved question in this section.** The
  widespread understanding is that music skills are partner-gated, but that is **not verified here**
  and must not be asserted. Resolve before any Alexa-route decision.
- **NO EVIDENCE FOUND** — whether an Alexa Routine can chain a Flash Briefing and then music in one
  trigger. Amazon's consumer help pages returned 503 on every attempt. The "Alexa, good morning"
  pattern is widely reported but is **SECONDARY at best and unverified here.**

### 6c. Amazon Music / Alexa+ AI features

- **VERIFIED** **Alexa+ AI podcasts**: Alexa+ generates spoken news content from **200+ licensed news
  sources** on demand. This is the closest incumbent match to the concept's **step 4** in *content*
  terms — AI-narrated news from licensed sources — but it produces a **discrete episode**, not a
  music-interleaved stream.
- **NO EVIDENCE FOUND** for an Amazon Music AI DJ voice host, for Maestro's current status/tier, or
  for Amazon Music interleaving podcasts with music. Do not assert these either way.
- **SECONDARY, unverified at primary level** Alexa **Routines** are the only place a user can
  plausibly get news-then-music today, and that is **sequential chaining, not interleaving.**

**Verdict on Alexa: occupies steps 3 and 4 as a voice-first experience, and is a poor route.** The
"news then music" job is served on the most commute-adjacent surface there is. As a build route,
Flash Briefing's 10-minute pre-rendered-feed model cannot express the product, and the custom-skill
route inherits the full music-licensing burden with an explicit proof-of-rights gate at
certification. It is best read as **a competitor that also demonstrates the job is real** — and as
evidence that the concept's step 4 (news) is the most commoditised of the five.

---

## 7. The graveyard — dead products, and their causes of death

Cause of death is stated only where evidenced. Where Spotify or the operator published no reason, it
says so; no plausible-sounding motive has been supplied to fill a gap.

### Verified deaths with stated causes

| Product | Died | Stated cause |
|---|---|---|
| **Spotify Stations** (standalone app) | 16 May 2022 | Spotify described it as "sunsetting" an experiment, absorbed into main-app Spotify Radio (direct Spotify quote) |
| **Spotify Car Thing** | production halted 27 Jul 2022; devices **bricked 9 Dec 2024** | "supply chain issues and lack of consumer interest"; **$31.4M write-down** |
| **Curio** (narrated journalism) | ~2024 | Founders state narrated journalism "could not be sustainable long term" against AI and big tech; returned ~$10M to publishers |
| **Google Podcasts** | 2 Apr 2024 (US) | Consolidation into YouTube Music as a "centralized podcast destination" |
| **Google Play Music** | 3 Dec 2020 | Consolidation into YouTube Music |
| **TuneIn Pro recording** | 14 Sep 2020, worldwide | "legal reasons" |
| **MySXM** | removed in 2018 redesign | Promised replacement never evidenced shipping |

### Deaths where the cause is NOT established — do not fill these in

**Spotify Live/Greenroom** (30 Apr 2023), **RadioPublic** (Mar 2024), **Pandora Stories**,
**Otto Radio**, **Audioburst**, **Swell**, **Umano**, **Bumper**. For each: **cause of death: NO
EVIDENCE FOUND.**

### Alive and relevant

- **Spotify AI DJ** — expanding through 2026, **no news content** (§1).
- **iHeartRadio Artist Radio** — still shipping.
- **Huxe** (Sept 2025) — **news-only, no music**.
- **TuneIn** — acquired by Stingray for **$175M in Dec 2025**, down from a **$500M valuation in
  2017**. A 65% decline in enterprise value over eight years is the clearest available datum on the
  digital-radio business's trajectory.

### Spotify "Your Daily Drive" — the entry that carries the most weight

- **What it did:** personalised music with brief WSJ/NPR/PRI news podcast segments interleaved, in a
  drive-time format, refreshing through the day. Launched **June 2019**. (§2b; SECONDARY for the
  description: <https://en.wikipedia.org/wiki/Spotify>)
- **Status:** retired. VERIFIED by the support article being gutted, the newsroom carrying no posts,
  and a probed playlist ID 404ing. Bracketed to **before 2026-04-22** (§2b). Spotify has said nothing
  publicly about it since **October 2021**.
- **Cause of death: NO EVIDENCE FOUND.** Spotify never announced it and gave no reason. The most
  probable explanation — low usage relative to editorial and licensing overhead — is **INFERRED, not
  verified**, and labelled as such deliberately. What *is* verified is the silence: no sunset post,
  and a related user idea closed on a procedural technicality rather than on merit.
- **Why it matters most:** it is not an adjacent product that happened to fail. It is **this concept's
  steps 2, 3 and 4, built and operated by the platform whose API the concept wants to use, with every
  structural advantage, then withdrawn in silence.** It also **required bilateral publisher deals per
  market** — the WSJ/NPR/PRI relationships were negotiated, not licensed off a shelf.

### Spotify "Music + Talk"

Creator format, ~2020, full licensed songs alongside spoken segments. **Status: NO EVIDENCE FOUND
either way** — support URLs 404, absent from Wikipedia's Spotify article, no sunset notice located.
Same silent-404 signature as Daily Drive, which is **suggestive, not conclusive.**
**Cause of death: NO EVIDENCE FOUND** (death itself unconfirmed).

### The pattern — and it is the actual finding

**Your Daily Drive is the only serious music+news blend ever shipped at scale. It required bilateral
publisher deals per market. Meanwhile the music+talk format that survived and expanded is AI
commentary *about the music itself* — which needs no third-party licensing.**

That asymmetry is what the whole graveyard points at: **talk you own scales; talk you license does
not.** Curio's own shutdown note is the clearest articulation of why the licensed side does not close.
Spotify DJ is the same lesson expressed as a survivor — Spotify kept the commentary format that
required no negotiation and dropped the one that did.

This is directly load-bearing for the concept's **step 4**. The concept proposes "freely
broadcastable news segments," i.e. the licensed side of that asymmetry — the side with a graveyard.

### Remaining gaps in this section

**NO EVIDENCE FOUND / not researched to a conclusion:** Pandora Podcast Genome's current status;
whether TuneIn actually lost MLB/NFL rights (the evidence found points the *opposite* way — TuneIn
was *acquiring* those rights through Mar 2022); Audacy's Chapter 11 detail; iHeartRadio's financials.
Ticket **08** independently owns the indie graveyard and is the right home for the startup entries.

---

## 8. The clause that decides it: Spotify's Developer Policy bars the core mechanic

This is not an incumbent-*feature* finding, but it emerged while answering the "is it one API call
away?" question and it is too decisive to leave in a footnote. **VERIFIED**, all from
<https://developer.spotify.com/policy>, prohibited-applications section III:

- **III.5 — integration with other services:** "Do not create any product or service which is
  integrated with streams or content from another service."
- **III.7 — mixing with other audio:** "Do not permit any device or system to segue, mix, re-mix, or
  overlap any Spotify Content with any other audio content" — and the text notes this restriction
  extends even to combining with *other Spotify* material.
- **III.4 — non-interactive webcasting:** "Do not create any product or service which includes any
  non-interactive internet webcasting service," illustrated as "you can't create an application which
  plays content from a single source to several simultaneous listeners."
- II.5 requires cover art and metadata displayed during playback.

This **upgrades the map's carried-over INFERRED-fatal flag on III.5 to VERIFIED**, and adds III.7,
which the map did not have and which is arguably worse. III.7 names *segue, mix, re-mix, overlap* —
the exact verbs for what the concept's steps 3 and 5 do. A podcast segment seguing into a liked song,
or an AI intro overlapping a track, is prohibited by name.

The juxtaposition with §2e is the whole story in two lines: **Spotify's API explicitly accepts episode
URIs in the playback queue, and Spotify's policy explicitly forbids using it that way.** The concept's
core mechanic is one API call away technically and contractually out of reach. Per the map's standing
preference, a workaround that violates a clause is not a workaround. Ticket 01 owns the full Spotify
verdict; this ticket hands it a verified, quotable clause set.

**NO EVIDENCE FOUND** for any equivalent Apple clause (§3d) — a genuine gap, not a clean bill of
health, and the highest-value open question this ticket leaves behind.

---

## 9. Per-feature assessment: occupied, one API call away, or structurally uninterested?

| Feature | Assessment |
|---|---|
| **Spotify DJ** | **OCCUPIES step 5.** Shipped, no incremental cost to Premium, 75+ markets, 6 languages, on Android Auto, expanding May 2026. Closed to third parties — cannot be built on, only competed with. |
| **Spotify daylist / Discover Weekly / Blend** | **OCCUPIES step 2** — and, worse, *withdrew* API access to it for new apps in Nov 2024. Occupied and unavailable. |
| **Spotify Your Daily Drive** | **OCCUPIED steps 3+4, then vacated them.** Not a live threat. A dead one, which is more informative. |
| **Spotify Prompted Playlist / DJ requests** | Occupies the "talk to your radio" adjacency; podcast playlists, not blended streams. Closed to third parties. |
| **Apple Music Radio / stations / mixes** | **OCCUPIES step 2. Structurally uninterested** in steps 3–5 — MusicKit has no podcast type anywhere in its framework. **NOT one API call away**: adding podcasts would need a new content type across the whole framework. |
| **Apple AutoMix / crossfade** | Occupies AI *transitions*. Never speaks. And `crossfade(duration:)` is **public API from iOS 18** — Apple hands the concept a real crossfade rather than a hard cut. |
| **YouTube Music mixes / Beyond the Beat** | **OCCUPIES step 5 in embryo** (AI commentary between tracks, US Labs). Irrelevant as a threat *and* as a host: three independently fatal policy clauses close the platform entirely. |
| **Tidal My Mix / radio** | **Provisionally not a threat** — no AI voice host, no evidence of blending. Also the **only** platform with a documented full-track third-party playback SDK. Licensing unresolved. |
| **Alexa Flash Briefing + Routines** | **OCCUPIES steps 3+4 sequentially**, on the most commute-adjacent surface there is. **Poor route**: 10-min pre-rendered feed cap; custom-skill route inherits the full music-licensing burden with a proof-of-rights gate at certification. |
| **Alexa+ AI podcasts** | **OCCUPIES step 4's content job** — AI news narration from 200+ licensed sources — as a discrete episode, not a stream. |

### Which of the concept's five steps remain genuinely unoccupied?

Honest answer: **one and a half of five, and the surviving one is the least valuable.**

- **Step 1 (log in via a streaming service)** — not a differentiator; table stakes.
- **Step 2 (read history / liked songs / followed podcasts)** — **occupied by every platform**, and on
  Spotify the API access needed to replicate it was withdrawn for new apps in Nov 2024. Apple leaves
  it open. Not a differentiator anywhere.
- **Step 3 (continuous podcast↔music interleaving)** — **the only step with a real claim to being
  unoccupied today.** No incumbent currently ships automatic interleaving of spoken content with music
  in one continuous stream. But this is emphatically *not* virgin territory: Spotify shipped it
  (Daily Drive), ran it for years, and withdrew it in silence. Unoccupied because it was **vacated**,
  not because nobody thought of it.
- **Step 4 (news segments)** — **effectively occupied.** Alexa Routines chain news→music today;
  Alexa+ generates AI news audio from 200+ licensed sources. And §7's asymmetry says this is the
  licensed side of the line, the side with the graveyard on it.
- **Step 5 (AI intros/outros)** — **occupied and free.** Spotify DJ, 75+ markets, six languages,
  Android Auto, no incremental cost, actively invested in. YouTube Music's Beyond the Beat is a second
  entrant. This is the ticket's plainest finding: **the concept's stated differentiator is gone.**

**So: step 3 survives, step 4 is contested-to-lost, and steps 1, 2 and 5 are occupied.** The ticket
asked for honesty if the answer was "few"; the answer is few.

### The uncomfortable synthesis

The one unoccupied step is unoccupied for a documented reason, and the reason is not oversight.
Spotify built exactly it, with music rights, a podcast catalogue, WSJ/NPR/PRI relationships, and no
need for anyone's permission — and dropped it without explanation. The format that survived at
Spotify is the one requiring no third-party licensing. That is a coherent, repeated pattern across
the graveyard rather than a single data point.

Set against that, the demand evidence found here is **genuine but small and specific**: real users on
Spotify's own board asking for this concept in their own words, saying no alternative exists, saying
the AI DJ does not substitute, and demanding a one-click automatic experience. One names it as their
reason for keeping a decade-old subscription. That is on-point evidence — and it is consistent with
Spotify's decision rather than a refutation of it. A feature can be loved by hundreds and still be
uneconomic. **These quotes establish that the pain is real for an identifiable group; they do not
establish market size, and must not be used as if they did.** Ticket 09 owns that question.

### Recommendations to downstream tickets

1. **Ticket 05 should revisit the map's platform ranking.** On the incumbent-threat and
   developer-openness axes together, **Apple Music is the strongest host and Spotify the weakest** —
   the reverse of the map's stated preference. Spotify occupies step 5, withdrew step 2's API access,
   and forbids the core mechanic by name (§8). Apple ships no AI host, cannot represent a podcast at
   all, leaves recommendations/recently-played/library open, and provides a public crossfade API.
2. **Ticket 02's highest-value task is the Apple clause gap** — whether the Developer Program License
   Agreement or MusicKit ToS contains a Spotify-III.7-equivalent restriction on mixing Apple Music
   with third-party audio. If it does, the platform-native variant is dead everywhere and the map
   turns on the licensed-catalogue and local-library variants. If it does not, Apple is the answer.
3. **Ticket 10 should weigh cutting step 5.** It is occupied by a free incumbent feature, and §7's
   evidence is that AI-commentary-about-music survives precisely because it is cheap and
   unencumbered — which means it is also cheap for incumbents to keep expanding.
4. **Ticket 06 should treat step 4 as the weakest step**, not merely a legally awkward one. The
   graveyard's asymmetry, plus Alexa+ already generating licensed AI news audio, points to cutting
   news from an MVP.
5. **Ticket 04 should resolve Tidal's developer licence first** — it is the only documented full-track
   third-party playback path found, which makes it the only platform where the *technical* question is
   already answered and only the *contractual* one remains.

---

## Inheritance note

**Carried over from `podcasts-notes-validation` and re-verified here:** the Spotify Developer Policy
integration clause (previously INFERRED-fatal, **now VERIFIED** and joined by III.7 and III.4, §8);
YouTube Developer Policies III.I.7 / III.I.9 / III.I.14 / III.E.1 (**re-verified as current**, §4);
Apple Podcasts having no playback SDK (**extended** — Apple *Music's* SDK has no podcast type either,
§3b).

**New in this ticket:** everything on Spotify DJ's scope, tier, market count and trajectory; Your
Daily Drive's existence, description and retirement; Spotify's Nov 2024 withdrawal of
algorithmic-playlist and recommendation API access; the deprecation of `Get Recommendations`;
Spotify's add-to-queue endpoint accepting episode URIs; Policy III.7 and III.4; MusicKit's
`crossfade(duration:)` and open personalisation surface; AutoMix; YouTube Music's Beyond the Beat;
Tidal's `@tidal-music/player` full-asset playback; the Flash Briefing 10-minute cap and audio
requirements; Amazon's proof-of-rights certification gate; Alexa+ AI podcasts; and the graveyard
table in §7.

---

## 8. The clause that decides it — Spotify's Developer Policy

Strictly this belongs to ticket 01, but it surfaced here while testing whether incumbents' features
are reachable by API, and it changes how every finding above should be read. **New work; it confirms
and sharpens the carried-over INFERRED-fatal flag from `podcasts-notes-validation`.**

All from <https://developer.spotify.com/policy>, section III (prohibited applications):

- **III.5 — integration with other services:** "Do not create any product or service which is
  integrated with streams or content from another service."
- **III.4 — non-interactive webcasting:** "Do not create any product or service which includes any
  non-interactive internet webcasting service." Illustrated as: "you can't create an application which
  plays content from a single source to several simultaneous listeners."
- **III.7 — mixing:** "Do not permit any device or system to segue, mix, re-mix, or overlap any
  Spotify Content with any other audio content" — and the text notes this extends even to combining
  with **other Spotify material**.
- Also: III.6 bars pairing recordings with visual media; II.5 requires cover art and metadata during
  playback.

**Read against the concept's five steps, on Spotify:**

| Step | Clause |
|---|---|
| 3 — podcast → songs → podcast in one session | **III.5** (integrating third-party podcast/news streams) and **III.7** (segueing Spotify Content with other audio) |
| 4 — mixing in news segments | **III.5**, **III.7** |
| 5 — AI intros between songs and segments | **III.7** — an AI intro *overlapping or segueing* Spotify Content is named conduct |
| the "radio station" framing | **III.4** is aimed at simultaneous multi-listener webcasting, so a per-user personalised stream is arguably outside it — but the framing invites scrutiny |

**III.7 is the sharpest finding in this ticket.** It prohibits the concept's transition mechanic
*by name* — segue, mix, overlap — and it does so even between two Spotify tracks. This is why the
Add-to-Queue endpoint accepting `episode` URIs (§2e) is a trap rather than an opening: the API will
let you build the mixed queue, and the policy forbids the product built on it. The prior project's
INFERRED-fatal flag on III.5 is upgraded: **VERIFIED as prohibited by the letter of the policy**, and
III.7 is an additional, independent, more specific prohibition that was not previously identified.

Separately note the interaction with §1: Spotify **ships** AI commentary over music itself (DJ) while
its policy **forbids** third parties from segueing anything with Spotify Content. The incumbent
occupies the differentiator and the contract bars the challenger from matching it on the same
catalogue. That asymmetry is the core competitive fact of this ticket.

**Not legal advice.** These are quoted clauses and a reading of them; enforcement in practice is a
separate question from the letter of the policy, and ticket 01 owns the distinction.

---

## 9. Per-feature assessment: occupied / one API call away / structurally uninterested

| Feature | Assessment | Reasoning |
|---|---|---|
| **Spotify DJ** | **OCCUPIES the differentiator** | AI voice commentary over personalised music, no incremental charge to Premium, 75+ markets, 6 languages, Android Auto, actively expanding May 2026. Closed to third parties, so it can only be competed with — and III.7 bars competing on Spotify's own catalogue. |
| **Spotify daylist / Discover Weekly / Blend** | Occupies **step 2** only; structurally uninterested in talk | Music-only. Worse: API access to these was withdrawn for new apps in Nov 2024, so the concept cannot even read them. |
| **Spotify Your Daily Drive** | **OCCUPIED steps 3+4 for ~7 years — now vacated** | The only true precedent for the concept's core mechanic. Retired silently before Apr 2026. Vacancy created by an incumbent's withdrawal is a warning, not an opening. |
| **Spotify Music + Talk** | Unresolved | NO EVIDENCE FOUND either way. |
| **Spotify Prompted Playlist for podcasts** | **One API call away — but only for Spotify** | Apr 2026, podcast-only playlists today. Spotify already has AI playlist generation *and* podcast catalogue; blending them is a product decision, not an engineering problem. Spotify is the incumbent most likely to re-occupy steps 3+4, and it has done it before. |
| **Apple Music Radio / stations / mixes** | **Structurally uninterested** in steps 3–5 | MusicKit has **no podcast type anywhere in its type system**. Adding one is not one API call — it is a new content model across the framework. |
| **Apple AutoMix / crossfade** | Occupies the *transition-quality* bar; **helps** the concept | AutoMix is AI beat-matching that never speaks — the complement of Spotify DJ. `MusicPlayer.Transition.crossfade(duration:)` (iOS 18+) gives a third-party app a real crossfade, relieving the hard-cut ceiling from ticket 05. |
| **YouTube Music** | Structurally **closed** (not merely uninterested) | III.I.7, III.I.9, III.E.1 each independently fatal. Occupation is moot. |
| **Tidal** | **Provisionally not a threat — under-researched** | No evidence of AI voice host or music+talk. Weak claim from absence of evidence; ticket 04 owns it. |
| **Alexa Flash Briefing + music** | **OCCUPIES steps 3+4** as a voice-first experience; poor route | Closest shipped analogue to the whole concept, on the most commute-adjacent surface. As a route: 10-min pre-rendered feed cap kills Flash Briefing; custom skills inherit full music licensing with proof-of-rights at certification; Music Skill API eligibility unverified. |
| **Amazon Music AI** | Unresolved | NO EVIDENCE FOUND for an AI DJ, Maestro status, or podcast interleaving. |

---

## 10. Which of the concept's five steps remain genuinely unoccupied?

Honest answer: **one and a half of five, and the one that survives is the least valuable.**

| Step | Status |
|---|---|
| 1 — log in via a streaming service | **Not a differentiator at all.** Plumbing. Available to anyone. |
| 2 — read history, liked songs, followed podcasts | **Occupied, and partly withheld.** Every platform does personalisation better. On Spotify a new app *cannot* read algorithmic playlists, recommendations, or audio features (Nov 2024). Apple leaves the equivalents open — the one place the concept has room. |
| 3 — podcast → songs → podcast in one session | **Occupied and vacated by Spotify (Daily Drive), occupied by Alexa Routines. Prohibited on Spotify by III.5 + III.7. Impossible on Apple (no podcast type). Prohibited on YouTube.** The concept's core mechanic is the step with the most obstacles. |
| 4 — mix in news segments | **Most commoditised of the five.** Shipped by Spotify (retired) and by Alexa Flash Briefing, which is mainstream and voice-first. |
| 5 — AI-generated intros/outros | **Occupied by Spotify DJ**, at no incremental charge, 75+ markets, 6 languages, in the car, still expanding. Named as prohibited conduct on Spotify by III.7. |

**Genuinely unoccupied, as far as this ticket can establish:**

1. **The specific combination on Apple Music.** No incumbent ships AI voice commentary *and*
   music+talk interleaving *and* real crossfade on Apple's catalogue, and Apple's dev surface is
   uniquely open (recommendations, recently-played, library, crossfade all public). This is a real gap
   — but it is a gap of *combination*, not of capability, and it depends on the unresolved Apple terms
   question (§3d). **It is the only structurally interesting finding for the concept in this ticket.**
2. **A commentary on/off toggle** — VERIFIED absent from DJ for three years. Genuinely unoccupied, and
   too small to be a product.
3. **Offline/downloaded operation of a blended stream** — VERIFIED absent from DJ. But this collides
   with Spotify ToS IV.3.b (carried over) and YouTube III.E.1; on Apple it is untested.

**The uncomfortable synthesis, stated without softening.** The concept's differentiating step (5) is
occupied by a free incumbent feature that is actively expanding into the exact context the developer
cares about. The concept's core mechanic (3) plus its news layer (4) were **built by Spotify itself
and silently retired** — meaning the strongest available evidence about this mechanic's commercial
viability is an incumbent's decision to stop doing it. And the platform whose catalogue the concept
most wants (Spotify) prohibits segue/mix/overlap of its content with other audio **by name**.

The one genuinely live thread is **Apple Music**: structurally uninterested in the talk layer, unable
to represent podcasts at all, and simultaneously the most permissive developer surface with an actual
crossfade API and open personalisation endpoints. That inverts the map's stated platform preference
ranking and should be raised at ticket 05.

Against that, the demand evidence found here (§2b) is real, specific, unprompted — and small, from a
biased sample, and fully consistent with Spotify having killed the feature for sound commercial
reasons. It should be carried to ticket 09 as a genuine signal with an explicit warning attached, not
as validation.

---

## 11. Open questions this ticket could not close

Listed because they are the ones that would change the conclusions, and because leaving them implicit
would overstate what was established.

1. **Apple's terms** on combining Apple Music content with third-party audio and on radio-style
   presentation. The API reference body did not render and the Program License Agreement PDF 404'd.
   **This is the highest-value open question in the map right now** — it decides whether the one
   surviving structural opening is real. Owner: ticket 02.
2. **Whether Alexa's Music Skill API / Radio Skills Kit is self-serve or partner-gated.** Every
   eligibility page 404'd. Decides whether the Alexa route exists at all.
3. **Whether Flash Briefing survives the Alexa+ transition.**
4. **Exact Daily Drive shutdown date and Spotify's reason.** The reason is the single most valuable
   missing datum in the whole ticket.
5. **Whether Spotify Music + Talk is live.**
6. **The historic graveyard** (Google, Pandora Stories, Audacy, iHeartRadio, TuneIn, SiriusXM, Otto
   Radio, Audioburst, Swell, RadioPublic) — not researched to a conclusion in this pass.
7. **Tidal's developer platform and podcast/AI status.** Owner: ticket 04.
8. **Reddit and press sentiment on Spotify DJ.** Inaccessible all session. All sentiment here is from
   Spotify's own board, which is a complainer-weighted sample.
9. **Amazon Music AI DJ / Maestro / podcast interleaving.**

Items 1–3 need working search or authenticated fetches; items 4–5 may be unanswerable, since Spotify
appears never to have published anything.
