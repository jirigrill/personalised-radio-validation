# 04 — Tidal feasibility (research)

Resolves `issues/04-tidal-feasibility.md`. **Entirely new ground** — the prior
`podcasts-notes-validation` project did not examine Tidal. Nothing in this document is carried
over; every finding below is new and sourced.

Evidence tags: **VERIFIED** = primary source, URL given. **SECONDARY** = credible third-party
estimate, range given. **INFERRED** = my reasoning from verified facts, labelled as such.
**NO EVIDENCE FOUND** = absent, not guessed.

---

## Verdict

> ### BLOCKED — and blocked more comprehensively than any other platform reviewed.
>
> Tidal is **not** the most permissive of the four. It is the most restrictive. The premise in the
> map — "Tidal relaunched a public developer platform, so this may be the most permissive of the
> four" — is **false**, and it is falsified by Tidal's own published Developer Guidelines, not by
> inference.
>
> Tidal's Developer Guidelines v3.0 §III enumerate, as *separately listed* strictly prohibited
> offering types, **four independent clauses** each of which alone kills this concept:
> integrating Tidal content with streams from another service; combining Tidal content with any
> other audio content; using Tidal content with AI technologies; and generating news media. The
> concept in `map.md` does all four.
>
> On top of that, third-party playback is **30-second previews only** — so even a
> Tidal-music-only app cannot play a full song.

### Verdict table

| # | Ticket question | Finding | Verdict |
|---|---|---|---|
| 1 | Developer programme | Real, public, self-serve signup. API (JSON:API, 256 paths) + first-party SDKs for **iOS, Android, Web**, all Apache-licensed on GitHub. But access is **two-stage**: self-serve "in development" with quotas, then **manual source-code review** to reach Production. | **Open to sign up, gated to ship** |
| 2 | Playback | Genuine playback SDK — real audio decode via AVPlayer/ExoPlayer, not remote control. Own queue, `pause()`, `seek(position)`, `setNext()` all present. **But third parties get PREVIEWS ONLY** (~30s). Full-length requires `Device Login`/`Authorization Code` flows Tidal has not opened. | **PARTIAL → effectively BLOCKED** |
| 3 | **Interleaving** | **Explicitly prohibited, twice, in enumerated lists.** "Integrating TIDAL content with content or streams from another service" and "functionality that segues, overlaps, mixes … or combines any TIDAL content with any other audio content". Crossfade API exists (0–15s) but is Tidal-track→Tidal-track only. | **HARD BLOCKER** |
| 4 | User data | Favourites/collection **yes** (`/userCollectionTracks`, `collection.read`). Algorithmic mixes **yes**. **Listening history: NO ENDPOINT** — and scraping it is expressly prohibited. **Podcasts: zero** — no podcast resource in the API, and Tidal discontinued all podcasts 24 Jul 2024. | **PARTIAL; podcasts BLOCKED** |
| 5 | Offline/caching | SDK has a full Offline Engine, **but the Developer Terms limit third parties to "temporary caching of metadata and cover art"** and "Do not store TIDAL Content indefinitely." | **BLOCKED for the commuting case** |
| 6 | Commercial reality | Tidal has **never disclosed subscriber counts**; Block does not disclose them. Last verifiable figure is **2.1M (2020, court documents)**. Market share **under 1%**, not tracked as a named service by MIDiA/Statista. Block has written off **~$266M of the $297M** purchase price and said it is "scaling back our investment in Tidal". | **Commercially pointless even if permitted** |

**Net:** Tidal fails on legal permissibility, on playback capability, on offline caching, on podcast
content, and on addressable audience — five independent failures. There is no narrower variant of
the concept that survives, because the prohibition is on the *mechanic* (combining audio), not on
any particular scope.

---

## Note on sourcing method (matters for auditability)

`developer.tidal.com` is a client-rendered React SPA. `curl` of any documentation URL returns an
empty `<div id="root">` shell, and WebFetch of those URLs therefore returns nothing — which is why
a casual check of the site appears to show no terms at all. **The legal text is real and it is
served from the site**, inside lazily-loaded JS chunks referenced by the main bundle:

- Bundle: `https://developer.tidal.com/assets/index-BU00rsRS.js`
- Developer Terms v3.0: `https://developer.tidal.com/assets/developer-terms-3_0-BhtucITZ.js`
- Developer Guidelines v3.0: `https://developer.tidal.com/assets/developer-guidelines-3_0-DN4FTLfL.js`
- Rendered routes: `https://developer.tidal.com/documentation/guidelines/guidelines-developer-terms`
  and `.../guidelines-developer-guidelines`

Every quote below was extracted verbatim from those chunks. Both documents self-date as
**"Version 3.0 as of May 6, 2024"** — v1.0 (12 Sep 2023) and v2.0 (15 Jan 2024) are also still
shipped in the bundle, and the app fetches the current version dynamically from
`GET /v2/terms?filter[termsType]=DEVELOPER&filter[isLatestVersion]=true`. v3.0 is the newest
present in the live bundle. **VERIFIED.**

The API surface was read from Tidal's own committed OpenAPI 3.0.1 spec, `TIDAL API 1.10.104`,
1.5 MB, 256 paths, server `https://openapi.tidal.com/v2`:
`https://github.com/tidal-music/tidal-sdk-web/blob/main/packages/api/bin/tidal-api-oas.json`
This is a first-party artefact, not a community reverse-engineering. **VERIFIED.**

---

## 1. The developer programme

### What exists — more than expected

**VERIFIED.** Tidal runs a genuine, current, first-party developer platform. This is not a
vestigial or abandoned surface:

- **TIDAL API** — public REST, conforms to the JSON:API spec, 256 paths.
  Reference: <https://tidal-music.github.io/tidal-api-reference/>
  Docs: <https://developer.tidal.com/documentation/api/api-overview>
- **TIDAL SDK** — first-party, open-source, actively maintained on all three target platforms.
  Each has an `auth`, `common`, `eventproducer` and **`player`** module:
  - Web: <https://github.com/tidal-music/tidal-sdk-web> (npm `@tidal-music/player`)
  - iOS: <https://github.com/tidal-music/tidal-sdk-ios> (SPM)
  - Android: <https://github.com/tidal-music/tidal-sdk-android> (`com.tidal.sdk:player`)
  - Platform-agnostic design docs: <https://github.com/tidal-music/tidal-sdk>
- **TIDAL Embeds** — an HTML embed player for websites.
- **Tidal Connect** — cast-to-device.

Liveness check: `tidal-sdk-web` and `tidal-sdk-ios` both had commits within days of this research
(Sep 2026). This platform is maintained. **VERIFIED.**

Notably, Tidal's SDKs are the *same* SDKs Tidal's own apps use — which is why the Player module is
a real audio engine rather than a remote-control shim. Tidal says so directly:

> "The TIDAL SDK is designed and built for internal use at TIDAL, meaning that our own apps are
> relying on the TIDAL SDK for much of the heavy lifting."

— <https://developer.tidal.com/documentation/api-sdk/api-sdk-overview> (**VERIFIED**)

**This is the strongest technical developer offering of the four platforms in this sweep.** That
makes the legal position in §3 all the more decisive: the block is not a capability gap Tidal
might close, it is a deliberate licensing choice.

### Access model: self-serve to start, source-code review to ship

**VERIFIED.** Signup is self-serve — create a Tidal account, log in to
`https://developer.tidal.com/dashboard`, register an app, receive a Client ID and Client Secret
(<https://developer.tidal.com/documentation/api-sdk/api-sdk-quick-start>). There is no application
form to *begin*.

But that only gets you a restricted sandbox. Developer Terms v3.0 §IV:

> "Upon gaining access to the Developer Tools, Your Offering will remain in "in development," with
> applicable quota limits and use restrictions. To obtain TIDAL's approval for a quota extension,
> i.e., approval to move Your Offering into ["Production Mode"], you must submit Your Offering for
> formal approval by TIDAL. **Upon submission for approval, we will review Your Offering,
> including Your Offering's source code**, to ensure it is in compliance with these Developer
> Terms, as determined in our sole discretion. Any approval of Your Offering is contingent on the
> specific use case presented in your submission. We may revoke approval and reinstate limits and
> restrictions if Your Offering is used for a different or unapproved use case. Any material
> changes to Your Offering must be submitted to TIDAL for approval in advance."

Note the strength of this relative to Spotify or Apple: Tidal reserves the right to **read your
source code**, ties approval to a **specific declared use case**, requires pre-approval of
**material changes**, and can **revoke** afterwards. For this concept that is fatal in a
practical sense too — you cannot ship the mechanic and hope it goes unnoticed, because a human at
Tidal reads the code against the very guidelines the mechanic violates.

Also §IV:

> "TIDAL may limit the number of service calls that Your Offering may make, the selection or
> volume of TIDAL Content that may be accessed, or anything else about the TIDAL Platform as TIDAL
> deems appropriate, in its sole discretion, without notice."

**Numeric rate limits / quota values: NO EVIDENCE FOUND.** Not published; the terms deliberately
leave them at Tidal's discretion.

The API spec confirms a two-tier access model in machine-readable form. Every OAuth scope carries
an `x-scopes-required-access-tier` value of either `THIRD_PARTY` or `INTERNAL`. `r_usr`
("Read access to all end user data") and `w_usr` are **`INTERNAL`** — Tidal's own apps only.
**VERIFIED** (`tidal-api-oas.json`, `components.securitySchemes.Authorization_Code_PKCE`).

### Commercial use / paid apps

**A paid or revenue-generating app is outside the self-serve grant.** Two independent findings:

1. Developer Guidelines v3.0 §III prohibits, absent express written approval:
   > "Offering products or services targeted for use by businesses (it being understood that **the
   > TIDAL Service is for private use only, and use in commercial or public settings is not
   > permitted**)"
2. §III also prohibits "Generating news media or **offers for commercial products**".

**NO EVIDENCE FOUND** for a published commercial-agreement process, a revenue-share programme, or
a named paid tier for developers. There is no self-serve route to a monetised Tidal integration;
it would require negotiated written approval — which the map explicitly places out of scope
("Becoming a licensed broadcaster / negotiating direct label deals… out of reach for a solo
developer at a \$1–3k MRR bar").

---

## 2. Playback

### It is a real audio engine — the strongest of the four

**VERIFIED.** Unlike the Spotify iOS SDK (which the prior project verified is remote-control only
and requires the Spotify app installed), Tidal's Player module decodes and renders audio inside
*your* process, with no Tidal app required:

- **iOS** — "The implementation uses Apple's `AVPlayer` as the media player."
  <https://github.com/tidal-music/tidal-sdk-ios/blob/main/Sources/Player/README.md>
- **Android** — "The implementation depends on AndroidX Media (`androidx.media3`) and uses
  `ExoPlayer` as the media player."
  <https://github.com/tidal-music/tidal-sdk-android/blob/main/player/README.md>
- **Web** — `@tidal-music/player`, backed by Shaka Player / native `HTMLMediaElement`.

Both mobile READMEs list the same features: "Streaming and playing TIDAL catalog content", "Core
playback functionality", "Media streaming, caching and error handling".

The API even exposes the raw asset. `GET /trackFiles/{id}` returns an attribute `url` —
"File URL" — and formats `FLAC`, `FLAC_HIRES`, `AACLC`, `HEAACV1`, `EAC3_JOC`.
`GET /trackManifests/{id}` returns a manifest `uri` plus `drmData`. Both require the `playback`
scope, which **is** `THIRD_PARTY`-tier. **VERIFIED** (`tidal-api-oas.json`).

### Own queue, pause mid-track, resume at position — all yes

**VERIFIED**, from Tidal's own platform-agnostic design doc
<https://github.com/tidal-music/tidal-sdk/blob/main/Player.md>:

| Need | API | Verbatim |
|---|---|---|
| Build own queue | `next(MediaProduct)` | "Tell PE to make an implicit transition to the selected media product once the currently active finishes playing. **MUST be called EVERY TIME next up in playqueue changes.**" |
| Pause mid-track | `pause()` | "Pause playback of active media product." |
| Resume at position | `seek(Float position)` | "Change asset position in active media product." |
| Read position | `assetPosition` | "Read only property of the current asset position, in seconds, in the media product being played" |
| Skip | `skipToNext()`, `load()`, `reset()` | — |

Crucially, the app supplies the next item one at a time — there is no server-side playlist Tidal
controls. The design doc's own framing of transitions:

> "_Implicit transition_ - Occurs when one media product has finished playing and the next in queue
> is automatically played. **Allows the Player to make seamless, gapless, transitions.**"

**INFERRED (high confidence):** mechanically, `pause()` + `assetPosition` + `next()` + `seek()` is
exactly the primitive set the concept's "podcast → pause → song → back to episode" sequence needs.
The playback engine is not the obstacle. §3 is.

### The killer: third parties get previews only

This is the single most important technical finding, and it is stated flatly in Tidal's own docs.

> **NOTE**
> "The Player module in the SDK constitutes **the only allowed way** for third-party applications
> to incorporate playback of TIDAL content. By using an official, unmodified version of the Player
> module, third-party applications can include **playback of TIDAL previews**."

— <https://developer.tidal.com/documentation/api-sdk/api-sdk-overview> (**VERIFIED**)

Corroborated independently by both mobile SDK READMEs, in identical language:

> "Note that currently only the `Client Credentials` flow is publicly supported. This enables the
> test app to play **30-second tracks previews**. **Full length playback is only enabled when the
> client is authenticated through `Device Login` or `Authorization Code Flow`.**"

— <https://github.com/tidal-music/tidal-sdk-ios/blob/main/Sources/Player/README.md> and
  <https://github.com/tidal-music/tidal-sdk-android/blob/main/player/README.md> (**VERIFIED**)

And the Auth module README confirms `Device Login` is closed:

> "**Device Login** (Only available for TIDAL internally developed applications for now)"

— <https://github.com/tidal-music/tidal-sdk-web/blob/main/packages/auth/README.md> (**VERIFIED**)

The Developer Terms define the licence grant to match. §I:

> "TIDAL Content may include, among other things, album art, artist images, catalog metadata,
> artist biographies, playlist information, and **limited access to song previews** or, solely with
> respect to TIDAL Embeds … limited access to full-length songs through TIDAL Embeds"

So the *only* route to full-length third-party playback in the terms is **TIDAL Embeds** — an HTML
embed for websites, not an iOS app, and the terms confirm "Full-length Playbacks are only available
through the TIDAL Embed Player, and only TIDAL subscribers may access such full-length Playbacks.
Non-TIDAL subscribers may access only up to 30-second Playbacks." **VERIFIED.**

**Assessment.** `Authorization Code Flow` *is* documented for third parties and the `playback`
scope *is* `THIRD_PARTY`-tier, which suggests full-length playback may be reachable after
Production-Mode approval. But the developer-facing overview says third parties "can include
playback of TIDAL previews" without qualification, and there is **NO EVIDENCE FOUND** of any
published route by which an external developer obtains full-length playback in a native app. This
is a **PARTIAL** on paper and a **BLOCKED** in practice — and it is moot regardless, because §3
prohibits the offering type outright, so approval would be refused at the review gate.

### Subscription tier required

**VERIFIED.** Full-length playback requires the end user to be a Tidal subscriber
("only TIDAL subscribers may access such full-length Playbacks"). The API models this precisely:
`TrackManifests.previewReason` enumerates `FULL_REQUIRES_SUBSCRIPTION`, `FULL_REQUIRES_PURCHASE`,
`FULL_REQUIRES_HIGHER_ACCESS_TIER`, and 403 responses include `CLIENT_NOT_ENTITLED` and
`PURCHASE_REQUIRED`. There is an `entitlements.read` scope: "Read access to what functionality a
user is entitled to access on TIDAL, such as whether they can stream music, use DJ add-ons and
similar."

Tidal has **no free tier** — so every user of such an app would need a paid Tidal subscription
*plus* the app's own subscription. **INFERRED consequence:** this compounds §6 — the addressable
market is not "Tidal users" but "paying Tidal subscribers willing to pay a second subscription."

---

## 3. The interleaving question — the decisive section

This is the question the ticket flagged for special care. The answer is unambiguous and it is
worse than Spotify's.

### The binding document, and why the Guidelines count

The prohibitions live in the **Developer Guidelines**, not the Developer Terms. They are
nonetheless contractually binding, by their own terms and by the Terms' incorporation clause:

> "These Developer Guidelines are **subject to, and hereby incorporated into, the Developer
> Terms**."

— TIDAL Developer Guidelines v3.0, preamble,
  <https://developer.tidal.com/documentation/guidelines/guidelines-developer-guidelines>
  (**VERIFIED**)

And the Terms §I:

> "By using the Developer Tools and related materials made available to you, you agree to be bound
> by these Developer Terms **and any documentation and guidelines accompanying the Developer
> Tools**, the TIDAL Platform Terms and Conditions of Use applicable to you…"

### Guidelines v3.0 §III — Prohibited Offerings

The section opens:

> "**III - Prohibited Offerings**
> The use of the Developer Tools to create any of the following types of applications is **strictly
> prohibited unless you have TIDAL's express written approval**:"

Four separate enumerated items in that list each independently kill this concept. All four are
verbatim:

**(a) The interleaving clause — direct hit.**
> "**Integrating TIDAL content with content or streams from another service;**"

The concept's mechanic is precisely this: Tidal music streams interleaved with podcast episodes
from Apple Podcasts / an RSS feed / any other source. The clause names the mechanic.

**(b) The mixing clause — a second, broader direct hit.**
> "Creation or inclusion of functionality that **segues, overlaps, mixes or re-mixes any TIDAL
> content or otherwise alters TIDAL content or combines any TIDAL content with any other audio
> content (including without limitation other TIDAL content)**;"

Read the parenthetical carefully. It prohibits segueing/overlapping/mixing **even Tidal content
with other Tidal content**. This is what makes the finding unworkaroundable: it is not a
same-service carve-out. A radio-style programme that segues one Tidal song into another Tidal song
is prohibited, *before* any podcast enters the picture. And "combines … with any other audio
content" catches the podcast, the news segment, and the AI-generated intro alike.

**(c) The AI clause — kills the AI intros/outros, and arguably the whole app.**
> "Creating an offering that utilizes TIDAL content for any purpose in connection with any
> artificial intelligence or machine intelligence technologies;"

This one is also duplicated in the *Terms* themselves (§II - Restrictions and Prohibitions), where
it appears as an absolute prohibition with **no** "unless approved" escape hatch:
> "create an offering that utilizes TIDAL content for any purpose in connection with any artificial
> intelligence or machine intelligence technologies;"
> "use TIDAL content with artificial intelligence services or tools;"

The map's step 5 is "Inserts AI-generated intros/outros". **INFERRED (high confidence):** an
algorithmically personalised radio programmer is itself plausibly "machine intelligence
technologies … in connection with" Tidal content, so this clause may reach further than just the
TTS voice.

**(d) The news clause — kills step 4.**
> "**Generating news media** or offers for commercial products;"

The map's step 4 is "Optionally mixes in freely broadcastable news segments."

**Two more from the same list, for completeness:**
> "Providing access and/or distributing any music content from the TIDAL Service to non-subscribers
> of the TIDAL Service or via channels external to the TIDAL Service."
> "Creation or inclusion of a non-interactive webcasting service (e.g., an application that plays
> content from one source to multiple listeners simultaneously);"

A "personalised radio station" is close enough to the second that it would face scrutiny on its own.

### Terms §II adds an independent bar on podcasts specifically

Developer Terms v3.0 §II, "You will not … to:"

> "**Use TIDAL Content in podcasts**, messaging applications, games, or exercise programs;"

Also §II:
> "synchronize or otherwise use any TIDAL Content with any visual media output (including any
> advertisement, film, television program, slideshow, video or game);"

**VERIFIED.** Note that unlike Guidelines §III, Terms §II has no "unless expressly approved"
qualifier — these are flat prohibitions.

### Crossfade, ducking, per-stream volume — what actually exists

Here Tidal is technically *more* capable than expected, and it makes no difference.

**Crossfade: YES, but Tidal→Tidal only.** **VERIFIED** in shipped source on two platforms:

- Web — `setTransitionMode(crossfadeInMs)`, exported from the public API surface:
  > "Set the track transition mode via a single numeric value: - Zero: gapless (default) -
  > Positive: crossfade overlap in ms (e.g. 5000 = 5s crossfade). Clamped to the range 0..15000."

  <https://github.com/tidal-music/tidal-sdk-web/blob/main/packages/player/src/internal/handlers/set-transition-mode.ts>
- iOS — `public var crossfadeDuration: Int = 0`, documented "Duration of the crossfade between
  tracks, in seconds. When `0`, crossfade is disabled."
  <https://github.com/tidal-music/tidal-sdk-ios/blob/main/Sources/Player/Configuration.swift>
  (implementation: `CrossfadingPlayerWrapper.swift`, which also has an internal
  `pause(fadeDuration:)`)
- Android — **NO EVIDENCE FOUND** of a public crossfade setting. The `playbackengine/volume`
  package contains only `LoudnessNormalizer` and `VolumeHelper`.

The scope is the point: crossfade operates between the *active* and the *next* `MediaProduct`, and
`MediaProduct.productType` is an enum of exactly `TRACK` and `VIDEO` — both Tidal catalogue items.
There is **no API that accepts an external audio source**. So the crossfade surface cannot be
pointed at a podcast even if the guidelines permitted it. **VERIFIED** (`Player.md`, `ProductType`).

**Ducking: NO EVIDENCE FOUND.** No ducking API in the design doc or any of the three
implementations.

**Per-stream volume: partial, and not for this purpose.** The web SDK has
`setVolumeLevel(desiredVolumeLevel)`, range 0–100 — but its own docstring scopes it globally:
> "Set the desired volume level. Range 0-100. **This level will be used for all players**, but when
> loudness normalization is active it will be adjusted inside the player."

<https://github.com/tidal-music/tidal-sdk-web/blob/main/packages/player/src/internal/handlers/set-volume-level.ts>
(**VERIFIED**). It is one Tidal-wide volume, not a per-stream mixer, and there is no equivalent
public setter on iOS/Android — only `loudnessNormalizationMode` and
`loudnessNormalizationPreAmp`, whose documented purpose is replay-gain normalisation.

**So: is a hard cut the ceiling?** For Tidal→non-Tidal, **yes — a hard cut is the ceiling
technically, and even the hard cut is prohibited contractually.** The mixing clause bars
"segues, overlaps, mixes" — and a hard cut *is* a segue.

### Can Tidal playback coexist with another audio player in one session?

**Technically: partly, with an OS-level fight. Contractually: no.**

- **Contractually: NO.** Guidelines §III (a) and (b) above prohibit exactly this arrangement. A
  workaround that violates a clause is not a workaround.
- **Technically, iOS: contested.** The Player module uses `AVPlayer` and installs its own
  `AVAudioSession` observers (`AudioSessionInterruptionMonitor`,
  `AudioSessionRouteChangeMonitor`, `AudioSessionMediaServicesWereResetMonitor`). `AVAudioSession`
  is a **process-wide singleton**, so a second `AVAudioPlayer` for podcast audio shares one session
  the Tidal SDK is also configuring and reacting to. **INFERRED:** two independent players in one
  session is achievable in principle but the SDK is not designed to yield, and it exposes no
  category/mixing configuration to let you arbitrate.
- **Technically, server-side: actively blocked.** The API's 403 error enumeration for
  `/trackFiles/{id}` and `/trackManifests/{id}` includes **`CONCURRENT_PLAYBACK`**, and the Player
  event bus fires a `StreamingPrivilegesRevokedMessage`. **VERIFIED** (`tidal-api-oas.json`;
  `Player.md`). Tidal enforces one-stream-at-a-time at the account level and will revoke your
  stream — so "Tidal plus something else" is monitored, not merely disfavoured.

### Comparison to the Spotify finding carried in `map.md`

The map flagged Spotify's "integrated with streams or content from another service" clause as
**INFERRED-fatal** and the highest-priority thing to verify. Tidal's equivalent is **VERIFIED-fatal
and stronger**: Spotify has one clause; Tidal has that clause *plus* a separate clause banning
combining its audio with any other audio (including its own), *plus* a clause banning use in
podcasts, *plus* an AI ban, *plus* a source-code review gate that would catch it. Tidal is the
**least** permissive platform in this sweep, not the most.

---

## 4. User data

Read from the first-party OpenAPI spec (256 paths). **VERIFIED** unless noted.

### What exists

| Need | Endpoint(s) | Scope |
|---|---|---|
| Favourites / liked tracks | `/userCollectionTracks/{id}`, `/userCollections/{id}/relationships/tracks`, also albums / artists / playlists / videos | `collection.read` |
| Playlists | `/playlists`, `/playlists/{id}/relationships/items` | `playlists.read` |
| Algorithmic mixes | `/userDailyMixes/{id}`, `/userDiscoveryMixes/{id}`, `/userNewReleaseMixes/{id}`, `/userOfflineMixes/{id}` (each with `/relationships/items`) | `recommendations.read` |
| Recommendation blocks | `/userRecommendationBlocks/{id}/relationships/{artists,tracks,videos}` | `recommendations.read` |
| Play queue (server-side) | `/playQueues`, `/playQueues/{id}/relationships/{current,past,future}` | — |
| Track radio / similarity | `/tracks/{id}/relationships/radio`, `/similarTracks`, `/suggestedTracks`; `/artists/{id}/relationships/radio` | — |
| Account info | `/users/{id}` | `user.read` |
| Entitlements | — | `entitlements.read` |
| Search history | `/searchHistoryEntries/{id}`, `/searchSuggestions/{id}/relationships/history` | `search.read` |

The scope list is favourable on its face — `collection.read` and `recommendations.read` are both
`THIRD_PARTY` tier, so favourites and mixes are genuinely reachable. `/playQueues/{id}/relationships/past`
is interesting as the nearest thing to history, but it is queue state, not a listening log.

### Listening history: NO ENDPOINT, and scraping it is banned

**VERIFIED.** A full-text scan of the 1.5 MB spec for `listenHistory`, `listeningHistory`,
`playbackHistory`, `recentlyPlayed`, `topTracks`, `topArtists` returns **zero occurrences**. There
is no listening-history, top-items, or recently-played resource. This is a material gap versus
Spotify (`/me/player/recently-played`, `/me/top/{type}`).

Worse, the Terms pre-empt the obvious workaround. §II:

> "conduct, facilitate, authorize or permit any text or data mining or scraping in relation to the
> TIDAL Platform… For the avoidance of doubt, this prohibition expressly includes Personal Data …
> such as (but not limited to), **TIDAL users' playlists, listening history and generated
> activity**."

And Guidelines §III lists "**Applications that scrape or store end user data**" as a prohibited
offering type. So Tidal has (i) not exposed listening history, (ii) explicitly named it as
off-limits to mining, and (iii) reserved read access to all end-user data to the `INTERNAL`-only
`r_usr` scope. **This is a deliberate closure, not an oversight.**

**INFERRED:** the concept could substitute favourites + daily mixes for history — that is a
plausible personalisation input. The gap is real but is not the binding constraint; §3 is.

### Podcasts: zero, on two independent grounds

**VERIFIED — the strongest negative finding after §3.**

1. **No podcast resource in the API.** Scanning the spec for `podcast`, `Podcast`, `episode`,
   `Episode` returns **0 occurrences** across all 256 paths and every schema. The content model is
   explicitly music-only: `ProductType` is an enum of exactly `{TRACK, VIDEO}`, and the design doc
   defines "_Media product_ - **A TIDAL product of type track or video.**"
2. **Tidal discontinued all podcasts on 24 July 2024.** A Tidal spokesperson, quoted verbatim:
   > "we decided to **discontinue hosting MQA, 360 Reality Audio, and all podcasts** on TIDAL."

   <https://www.ecoustics.com/news/tidal-drops-mqa-360ra-podcasts/>. Confirmed by Tidal's own
   *Audio Format Updates* help article
   (<https://support.tidal.com/hc/en-us/articles/25876825185425-Audio-Format-Updates>). A search of
   Tidal's help centre returns **0 results** for `audiobook`, `spoken`, and `talk`, and the single
   `podcast` hit is stale boilerplate in a legacy *My Video Mix* article. Tidal *did* carry podcasts
   from June 2017 (Variety, 20 Jun 2017), so this is a deliberate exit from spoken word, not an
   absence of ambition.

3. And even if a podcast existed anywhere, Terms §II bars "**Use TIDAL Content in podcasts**".

**Consequence, stated plainly.** Tidal cannot supply podcast content at all. Any version of this
concept must source podcasts elsewhere — which is *precisely* the "Integrating TIDAL content with
content or streams from another service" that Guidelines §III prohibits. **The podcast finding and
the interleaving finding are not two separate problems; on Tidal they are the same problem, and it
has no exit.** Contrast: Spotify, Apple, and Amazon all carry substantial podcast catalogues.

---

## 5. Offline / caching — the commuting case

**BLOCKED.** The SDK can do it; the licence does not let you.

**The capability exists and is extensive.** `Player.md` documents a full **Offline Engine (OE)**
alongside the Playback Engine, with `offline(mediaProduct)`, `deleteOffline`, `deleteAllOfflines`,
`getOfflineState`, `offlineAudioQuality`, `offlineOverCellular`, plus `isOfflineMode` on the Player.
It is described as downloading "playbackinfo, manifest, DRM license and content … to permanent
(offline) storage on the device", explicitly so users "can play media products even when there is
no internet connection available." The API has matching resources: `/offlineTasks`, `/downloads`,
`/installations/{id}/relationships/offlineInventory`. **VERIFIED.**

**The Terms forbid a third party from using it.** Developer Terms v3.0 §II:

> "**locally host any TIDAL Content, except as strictly necessary to enhance the performance of Your
> Offering and its functionality, and limited to the temporary caching of metadata and cover
> art**;"

> "store, aggregate or create compilations or databases of TIDAL Content, other than as strictly
> necessary to operate Your Offering, and, in any event, you may not create stored databases or
> other compilations of data. You must use reasonable efforts to ensure that any data you display
> to end users is the most up to date data available through the TIDAL Platform, and to delete older
> data. **Do not store TIDAL Content indefinitely.**"

> "facilitate "stream ripping" or other functionalities that make it easier for users to capture or
> otherwise make permanent copies of TIDAL Content;"

**VERIFIED.** This is materially identical to the Spotify ToS IV.3.b finding the map carries over
("bars local caching of Spotify Content beyond metadata/cover art") — the same "metadata and cover
art only" formula. The Offline Engine exists for Tidal's own apps; the developer licence carves
third parties out of it.

**Consequence for the developer's own use case.** The map identifies commuting as the developer's
primary use, and flags poor connectivity and buffering-ahead as a potential second independent
blocker. On Tidal it is confirmed as a blocker: no permissible pre-download, so a
poor-connectivity commute cannot be served. Combined with `CONCURRENT_PLAYBACK` enforcement and
the 30-second preview limit, the developer's own stated use case is the *worst*-served case on
this platform. Also relevant: **App Store Guideline 5.2.3** (carried over from the prior project)
would independently bar downloading media from third-party sources without authorisation.

---

## 6. The commercial reality check

The ticket asked for this to be assessed plainly. It resolves plainly: **even if Tidal were fully
permissive, it would not be worth building for.**

### There is no current subscriber number, and that is the finding

**Tidal has never disclosed subscriber counts, and Block, Inc. does not disclose them.** Block's
SEC filings name Tidal as part of its ecosystem but report no Tidal subscriber metric.
**VERIFIED by absence** — checked against Block's 10-K filings.

- **Last verifiable figure: ~2.1 million (2020), from court documents.** **VERIFIED.** This is the
  most recent figure with a primary-source paper trail.
- **There is no credible Tidal subscriber estimate for any year after 2020.**
  **NO EVIDENCE FOUND.** Not "I could not find one" — none exists in the public record.
- **Historical Tidal-sourced claims are not trustworthy.** Tidal claimed ~3 million subscribers in
  2016; a 2017 investigation by the Norwegian newspaper *Dagens Næringsliv* alleged Tidal had
  materially inflated its subscriber claims. Any Tidal-sourced number should therefore be treated
  as **SECONDARY at best**, and no number at all is available for the present day.

**The defensible statement, and the only one:** "Tidal has never disclosed subscriber counts; Block
does not disclose them; the last verifiable figure is 2.1M from 2020 court documents." Do not put a
current number in the revenue model. There isn't one.

### Market share: below 1%, and not even tracked as a named service

**SECONDARY.** Tidal's share of global music-streaming subscribers is **under 1%**, and it is
**not broken out as a named service** in MIDiA Research's or Statista's market-share reporting —
it falls into "other". For scale, Spotify is ~30%+ and reports 250M+ subscribers.

**INFERRED (high confidence):** an app whose addressable market is "paying Tidal subscribers" is
addressing a sub-1% slice of music streaming, of unknown and undisclosed absolute size, most
recently measured at 2.1M six years ago.

### Block has written off the acquisition and says it is scaling back

**VERIFIED from primary filings and direct quotes:**

- Block acquired a majority stake in Tidal for **$297M** (2021).
- **~$265.8M of that $297M has been written off** — roughly **89%** of the purchase price.
  Impairment language in Block's filings ties writedowns to "terminations of certain revenue
  arrangements."
- Block's own words (Nov 2024): it is **"scaling back our investment in Tidal."**
- Repeated layoffs: >10% of Tidal staff cut Dec 2023; a further round reported to reach up to a
  quarter of staff; Block-wide cuts of ~4,000 roles Feb 2026.
- A Delaware judge dismissed a shareholder suit over the acquisition in May 2023 while explicitly
  calling it **"a terrible business decision"** — the holding was that Block is *permitted* to make
  terrible decisions, not that this wasn't one.
  <https://www.reuters.com/legal/judge-dismisses-lawsuit-over-blocks-terrible-purchase-jay-zs-tidal-2023-05-09/>
- Fortune reported (7 Nov 2024) that Block employees were ordered not to discuss board member
  Jay-Z amid the layoffs, prompting widespread "is this the end of Tidal?" press speculation.
  <https://fortune.com/2024/11/07/block-employees-jay-z-tidal-layoffs/>

**Countervailing evidence, in fairness.** **NO EVIDENCE FOUND** of a confirmed sale process or an
announced shutdown. Tidal is still named in Block's FY2025 10-K, is still shipping features
(AI-music tagging policy Jun 2026, Direct-to-Fan Sales Jun 2026), and raised prices Aug 2026.
**Assessment: Tidal is being run lean, not shut down.** But "run lean by an owner that has written
off 89% of its cost and publicly said it is disinvesting" is a poor platform to build a dependency
on, and the developer platform is exactly such a dependency — one Tidal reserves the right to
"remove access to any or all Developer Tools at TIDAL's sole discretion" (Terms §I).

**A pattern worth noting for platform risk.** Tidal's two flagship differentiators — Direct Artist
Payouts and fan-centred royalties, launched Nov 2021 — were **both discontinued by Feb/Mar 2023**,
within 16 months, after enrolling 70K artists and distributing only $500K against target. Tidal
also removed MQA and 360 Reality Audio in Jul 2024. This is a platform that retires features
quickly.

### Pricing and tiers

**VERIFIED** from Tidal's own help centre, effective **3 August 2026**
(<https://support.tidal.com/hc/en-us/articles/115003662825-Subscription-Types>):

| Tier | USD/month |
|---|---|
| Individual | $11.99 |
| Family (up to 6) | $19.99 |
| Student | $6.99 |
| DJ Individual | $19.99 |
| DJ Student | $14.49 |

**There is no free tier.** The US ad-supported free tier launched Nov 2021 was discontinued; only a
30-day trial remains. (Absence of a free tier **VERIFIED**; precise discontinuation date
**NO EVIDENCE FOUND**.) Single quality tier since 10 Apr 2024, when HiFi/HiFi Plus merged and
HiRes FLAC + Dolby Atmos moved into the base tier at $10.99; increased to $11.99 in Aug 2026.

### Podcast-listener overlap: unquantifiable, and structurally near-zero

The ticket asked what fraction of Tidal subscribers plausibly overlaps with podcast listeners.

**NO EVIDENCE FOUND** for any published Tidal-subscriber × podcast-listener overlap figure. But the
question is largely moot, and for a sharper reason than data scarcity: **Tidal removed podcasts
entirely in July 2024** (§4). Tidal has actively de-selected the spoken-word audience. **INFERRED
(high confidence):** a listener who wants podcasts interleaved with music has no reason to be a
Tidal subscriber in the first place, and Tidal's own product decisions have selected *against*
exactly the user this app targets. The overlap is not merely small; the platform has been
deliberately shaped to exclude it.

### Feeding this into the revenue model

Against the map's bar of **$1k/mo floor, $3k/mo target**, and a solo developer with **no audience**
(cold start, organic only, since the prior project ruled out paid acquisition at $0.62 LTV/install
vs $1.33 cheapest CPI):

- **Numerator unknown and unknowable.** No current subscriber figure exists, so no funnel can be
  built. Refuse to model it rather than invent a number.
- **Multiplicative filters, each severe.** Reachable market = (undisclosed Tidal subscriber base,
  last measured 2.1M in 2020, sub-1% share) × (paying subscribers only — no free tier) ×
  (podcast listeners — a group Tidal removed content for) × (willing to pay a *second* subscription
  on top of Tidal) × (cold-start organic discovery with no audience).
- **Assessment: Tidal cannot plausibly reach $1k/mo MRR as a sole platform**, on any assumption
  that isn't invented. It is not a marginal call.

**Plainly, as the ticket asked:** Tidal would be commercially pointless as a target platform even
if it were technically and legally permissive. As it happens it is neither — so this section is
confirmation, not the deciding factor. **Do not build the Tidal variant, and do not treat Tidal as
a fallback if another platform fails.**

---

## Hard blockers vs workarounds

### Hard blockers (no permissible workaround)

| # | Blocker | Source | Why there is no way round it |
|---|---|---|---|
| **HB1** | "Integrating TIDAL content with content or streams from another service" is a strictly prohibited offering type. | Guidelines v3.0 §III | Names the concept's core mechanic. Escape hatch is "TIDAL's express written approval" — a negotiated deal the map puts out of scope. |
| **HB2** | Combining Tidal audio with **any** other audio — including other Tidal audio — is prohibited: "segues, overlaps, mixes or re-mixes … or combines any TIDAL content with any other audio content (including without limitation other TIDAL content)". | Guidelines v3.0 §III | The parenthetical removes the same-service carve-out. Even a Tidal-only radio programme that segues song→song is prohibited. This is the clause with no narrower variant. |
| **HB3** | "Use TIDAL Content in podcasts". | Terms v3.0 §II | Flat prohibition, no approval carve-out. |
| **HB4** | AI/ML use of Tidal content prohibited — twice, once with no approval carve-out. | Terms v3.0 §II; Guidelines v3.0 §III | Kills AI intros/outros (map step 5), and plausibly the personalisation engine itself. |
| **HB5** | "Generating news media". | Guidelines v3.0 §III | Kills news segments (map step 4). |
| **HB6** | Third-party playback is previews only; full-length is Embeds-only or internal-only auth flows. | api-sdk-overview; both mobile SDK READMEs; Auth README; Terms §I | Cannot play a full song in a native app. No published route to change this. |
| **HB7** | Local caching limited to "temporary caching of metadata and cover art"; "Do not store TIDAL Content indefinitely." | Terms v3.0 §II | Kills the commuting/offline case — the developer's own primary use. |
| **HB8** | No podcast content on Tidal at all; discontinued 24 Jul 2024. | OpenAPI spec (0 hits); Tidal spokesperson; Tidal help centre | Forces an external podcast source, which triggers HB1. Closed loop. |
| **HB9** | Source-code review gate for Production Mode, tied to a declared use case, revocable. | Terms v3.0 §IV | Removes "ship it and hope". A human reads the code against HB1–HB5. |
| **HB10** | Addressable audience undisclosed, sub-1% share, owner has written off 89% and is disinvesting. | Block filings; MIDiA/Statista; Block statements | Not a legal blocker — an economic one. Fails the $1k/mo bar on any honest assumption. |

### Things that look like workarounds but are not

- **"Use the crossfade API to blend podcast into music."** The crossfade surface only accepts a
  `MediaProduct` (`TRACK`/`VIDEO`), i.e. Tidal catalogue items. There is no API accepting an
  external source. Technically impossible *and* barred by HB2.
- **"Run two independent audio players and hard-cut between them."** A hard cut *is* a segue
  (HB2), and the arrangement *is* integrating with another service (HB1). Technically it also
  fights a process-wide `AVAudioSession` and risks server-side `CONCURRENT_PLAYBACK` revocation.
- **"Only interleave Tidal music with Tidal music, drop podcasts."** Still barred — HB2's
  parenthetical explicitly covers "other TIDAL content". And it is no longer the concept.
- **"Ship in 'in development' mode without seeking approval."** Quota-limited and preview-only by
  design; not a shippable product, and Terms §IV reserves Tidal's right to monitor and block.
- **"Use the raw `url` from `/trackFiles/{id}` with your own mixer."** Requires the `playback`
  scope, and Guidelines §II states Playbacks "will only be available through our SDKs, namely, an
  official, unmodified version of the TIDAL Player module, and **TIDAL will reject any quota
  extension requests for any Offering that attempts to circumvent this**." Explicitly anticipated
  and refused.
- **"Ask Tidal for written approval."** Legitimate in principle — Guidelines §III's prohibitions
  are conditional on "unless you have TIDAL's express written approval". But this is a negotiated
  licence with a disinvesting rightsholder, for a use case that violates five separate clauses, on
  behalf of a solo developer at a $1–3k MRR bar. The map places this out of scope, and HB3/HB4
  (Terms §II) have **no approval carve-out** at all.

### What this means for the map

- **Tidal is eliminated as a platform-native music source.** It should not be treated as a
  fallback if Spotify/Apple/YouTube Music fail; it is strictly worse than all three.
- **The map's premise about Tidal was wrong and should be corrected**, not softened: Tidal is the
  *least* permissive of the four, and the finding is VERIFIED rather than inferred.
- **This strengthens the inherited Spotify finding.** The map flagged Spotify's "another service"
  clause as INFERRED-fatal and the top verification priority. Finding a near-identical clause,
  independently drafted, in a second platform's terms is evidence that this is an industry-standard
  label-licensing requirement rather than one platform's idiosyncrasy. **INFERRED (moderate-to-high
  confidence):** the platform-native variant is likely blocked everywhere, and the map's
  licensed-catalogue and local-library variants are where the surviving concept lives.
- **Two constraints generalise beyond Tidal** and are worth carrying into the remaining tickets:
  (i) the "metadata and cover art only" caching formula now appears verbatim in both Spotify's and
  Tidal's terms — treat the offline/commuting case as blocked on any platform-native variant until
  proven otherwise; (ii) explicit AI/ML prohibitions on catalogue content are appearing in
  developer terms, which bears on the AI-intro feature on any platform.

---

## Evidence limits and open items

Stated so a reader can judge confidence rather than infer it.

- **Terms version currency.** All quotes are from Developer Terms and Developer Guidelines
  **v3.0, "as of May 6, 2024"**, the newest present in the live SPA bundle. The app resolves the
  current version at runtime via `GET /v2/terms?filter[termsType]=DEVELOPER&filter[isLatestVersion]=true`,
  which requires authentication (returns `UNAUTHORIZED` unauthenticated). **I could not confirm
  v3.0 is the effective version** rather than a newer one served dynamically. Low risk to the
  verdict: the interleaving and mixing prohibitions are present in v3.0 and there is no indication
  of relaxation.
- **Full-length playback for approved third parties.** `Authorization Code Flow` is documented for
  third parties and the `playback` scope is `THIRD_PARTY`-tier, so full-length playback *may* be
  reachable post-approval. Tidal's overview page says only "previews" without qualification.
  I have marked this PARTIAL-on-paper / BLOCKED-in-practice rather than claiming certainty. Moot
  given §3.
- **NO EVIDENCE FOUND** (each checked, not assumed): numeric rate limits or quota values; a named
  commercial/revenue-share agreement for developers; any ducking API on any platform; any public
  crossfade setting on Android; any listening-history, top-items or recently-played endpoint; any
  Tidal subscriber estimate after 2020; any Tidal-subscriber × podcast-listener overlap figure; the
  precise date the US free tier was discontinued; any confirmed Tidal sale process or shutdown.
- **Method note.** WebSearch was unavailable throughout this research (HTTP 400) and DuckDuckGo's
  HTML endpoint now serves a CAPTCHA. Primary sources were therefore retrieved directly by `curl`
  against `developer.tidal.com` asset chunks, `raw.githubusercontent.com`, the GitHub API, and
  Tidal's Zendesk help-centre API; discovery used Google News RSS. `tidal.com` itself returns 403
  to non-JS clients, which is why pricing is cited from `support.tidal.com` rather than
  `tidal.com/pricing`.
- **Not independently re-verified by me:** the Block financial figures, market-share positioning,
  layoff history, and the podcast-discontinuation quote were established by a delegated research
  pass against SEC filings, Reuters, Fortune, ecoustics and Tidal's help centre, with URLs given
  inline above. The §1–§5 findings — every ToS clause, every API and SDK claim — I verified
  directly from primary artefacts myself.

## Sources

**Primary — Tidal legal (verbatim quotes above):**
- Developer Terms v3.0 — <https://developer.tidal.com/documentation/guidelines/guidelines-developer-terms>
  (text: `https://developer.tidal.com/assets/developer-terms-3_0-BhtucITZ.js`)
- Developer Guidelines v3.0 — <https://developer.tidal.com/documentation/guidelines/guidelines-developer-guidelines>
  (text: `https://developer.tidal.com/assets/developer-guidelines-3_0-DN4FTLfL.js`)

**Primary — Tidal technical:**
- API & SDK overview (previews-only note) — <https://developer.tidal.com/documentation/api-sdk/api-sdk-overview>
- Quick start — <https://developer.tidal.com/documentation/api-sdk/api-sdk-quick-start>
- API reference — <https://tidal-music.github.io/tidal-api-reference/>
- OpenAPI spec `TIDAL API 1.10.104` — <https://github.com/tidal-music/tidal-sdk-web/blob/main/packages/api/bin/tidal-api-oas.json>
- Player design doc — <https://github.com/tidal-music/tidal-sdk/blob/main/Player.md>
- iOS Player README / `Configuration.swift` — <https://github.com/tidal-music/tidal-sdk-ios>
- Android Player README — <https://github.com/tidal-music/tidal-sdk-android>
- Web `set-transition-mode.ts`, `set-volume-level.ts`, `auth/README.md` — <https://github.com/tidal-music/tidal-sdk-web>

**Primary — Tidal consumer:**
- Subscription types & pricing — <https://support.tidal.com/hc/en-us/articles/115003662825-Subscription-Types>
- Audio Format Updates (podcast/MQA/360RA removal) — <https://support.tidal.com/hc/en-us/articles/25876825185425-Audio-Format-Updates>

**Secondary:**
- Podcast discontinuation quote — <https://www.ecoustics.com/news/tidal-drops-mqa-360ra-podcasts/>
- "Terrible business decision" ruling — <https://www.reuters.com/legal/judge-dismisses-lawsuit-over-blocks-terrible-purchase-jay-zs-tidal-2023-05-09/>
- Block layoffs / Jay-Z reporting — <https://fortune.com/2024/11/07/block-employees-jay-z-tidal-layoffs/>
- Block scaling back Tidal — <https://techcrunch.com/2024/11/07/block-scales-back-tidal-investment-and-shutters-tbd-in-favor-of-bitcoin-mining/>
- Tidal launched podcasts 2017 — <https://variety.com/2017/digital/news/tidal-podcasts-1202471082/>
