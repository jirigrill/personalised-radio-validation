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

---

## 1. The integration clause (highest priority)

### 1a. The current clause, verbatim

VERIFIED. Spotify Developer Policy, **Section III "Some prohibited applications"**, rule 5. The
section is prefaced verbatim: "This section of the Developer Policy identifies some types of SDA and
functionality that you must not build, nor enable or encourage others to build:". Rule III.5 reads,
in full:

> "Do not create any product or service which is integrated with streams or content from another
> service."

Source: https://developer.spotify.com/policy (Effective as of 15 May, 2025)

CARRIED OVER + VERIFIED-CURRENT. The prior project quoted this clause and flagged it INFERRED-fatal.
**The clause is still present, verbatim identical, in the policy effective 15 May 2025.** The prior
finding is confirmed as to text. What is NEW here is (i) the clause number (III.5), (ii) the
identification of two further clauses that hit the same mechanic, and (iii) Spotify's own published
interpretation of them.

### 1b. Does III.5 prohibit this app? Yes — and it is not even the strongest clause

INFERRED, high confidence. The concept is: one continuous session that alternates Spotify music with
podcast audio fetched from RSS and news audio from third parties. That is a product which is
integrated with streams *and* content from another service. There is no reading of III.5 on which
"integrated" excludes "plays in alternation inside a single programmed session under our control".
If anything, sequencing another service's stream into the same listening session is a *tighter* fit
to III.5 than the prior project's multi-source clip-capture concept was.

But III.5 is arguably the *weakest* of the three applicable clauses, because "integrated" is at least
an arguable word. The other two are not arguable.

VERIFIED — **Policy III.7**, in full:

> "Do not permit any device or system to segue, mix, re-mix, or overlap any Spotify Content with any
> other audio content (including other Spotify Content)."

Source: https://developer.spotify.com/policy

This is the clause that actually decides the ticket, and it is worse than the prior project realised,
for two reasons:

1. **"segue"** is the first verb in the list. Segue is precisely the radio-programming act the
   concept is named after: moving from one item to the next as a programmed transition. The clause
   does not require blending, ducking, or overlap. Sequencing counts.
2. **"(including other Spotify Content)"** removes the obvious workaround. A developer might think:
   fine, keep the third-party audio out of it and just programme Spotify tracks into a station. III.7
   forbids segueing Spotify Content with *other Spotify Content* too.

VERIFIED — **Policy III.4**, in full:

> "Do not create any product or service which includes any non-interactive internet webcasting
> service. For example, you can't create an application which plays content from a single source to
> several simultaneous listeners."

Source: https://developer.spotify.com/policy

INFERRED — the illustrative example ("several simultaneous listeners") does not describe this app,
which is a per-user personalised station. So III.4 is the *weakest* fit of the three and I am not
resting weight on it. But note the prohibition is on "any non-interactive internet webcasting
service" as such; the "For example" is illustrative, not limiting. A lean-back, programmed,
non-skippable "radio station" is closer to non-interactive webcasting than a normal player is. If the
app were designed with limited skips (as terrestrial-radio-style licensing normally requires), it
would drift toward III.4. **INFERRED: this creates a squeeze.** Make it interactive and you are a
player that segues (III.7). Make it lean-back and radio-like and you approach III.4.

### 1c. Spotify's own published interpretation — this is the decisive new finding

**NEW.** The prior project did not read the Compliance Tips page. It is a primary source on
developer.spotify.com and it is Spotify telling developers how it reads its own policy: "This
resource serves to complement the Spotify Developer Policy and has been created to help you
understand some of its guiding principles. We also give a few examples that demonstrate how our
policies would apply in practice."

VERIFIED — under the heading **"Disallowed Use Cases"**, introduced verbatim as "Here are some common
app use cases that break our policy, and you should not build:", the list includes:

> "DJ/Mixes: Using Spotify's catalog to segue, mix, re-mix, or overlap any Spotify Content with any
> other audio content (including other Spotify content)."

and:

> "Synchronization: Syncing sound recordings accessed via the Spotify Platform with other recordings,
> lyrics, or video."

Source: https://developer.spotify.com/compliance-tips

The same page states enforcement intent explicitly:

> "We review the applications on our platform from time to time and may take action if you are found
> to violate our policies, including suspension, disabling your application or permanently revoking
> your access to the Spotify Platform."

INFERRED — Spotify has pre-labelled the category this app falls into ("using Spotify's catalog to
segue ... any Spotify Content with any other audio content") as a use case that "break[s] our policy".
The developer would not be arguing an ambiguity in a contract; they would be arguing against
Spotify's own published example of the thing you must not build. That is close to the worst possible
posture.

### 1d. Prohibited by the letter, or prohibited in practice?

The ticket asks for this distinction, so here it is honestly.

**Prohibited by the letter: yes, unambiguously.** VERIFIED across III.5, III.7 and Compliance Tips.
This is not a stretched reading.

**Prohibited in practice: I have less evidence, and I will not overstate it.** NO EVIDENCE FOUND of a
named third-party app being shut down specifically under III.5 or III.7 — my web-search tool failed
repeatedly during this pass and I could not gather SECONDARY enforcement anecdotes. **This is a real
gap.** What I *can* offer from primary sources is that the practical question is largely moot:

INFERRED, and this is the important point — **enforcement is not the binding constraint, because the
gate comes first.** To ship to more than 5 users the developer must pass a Spotify app review for
extended quota mode, in which "Spotify's app review team will take a look at your app and evaluate it
for compliance with our Developer Policy" and will "test out your app" (VERIFIED, quota-modes page).
So the app must be *shown to Spotify* and *approved against the very policy it violates*, before it
can have a sixth user. There is no version of this where the developer ships quietly and hopes not to
be noticed at any scale that matters. The "in practice" risk profile is therefore not "might get
caught later" — it is "cannot get through the door".

### 1e. Is there a carve-out, waiver, quota extension, or commercial-partner route?

I looked for all four. Answers:

**Carve-out in the policy: NO EVIDENCE FOUND.** VERIFIED by reading Section III in full: III.1
("ringtone or alert tone ... unless you receive Spotify's written approval") and III.11 ("without our
prior written permission") each carry an explicit written-approval escape hatch. **III.4, III.5 and
III.7 carry none.** INFERRED, and this is a meaningful drafting signal: Spotify knows how to write a
waivable prohibition, and chose not to make the mixing and multi-service clauses waivable.

**Quota-extension route: closed to this developer on eligibility grounds, before merits.** VERIFIED,
quota-modes page: "Please note that as of May 15th 2025, Spotify only accepts applications from
organizations (not individuals)." The application "must be sent through a company email". Stated
implementation requirements, verbatim:

> "Established Business Entity (legally registered business or organisation)"
> "Operating an active, and Launched Service"
> "Maintaining a minimum of active users (at least 250k MAUs)"
> "Being available in key Spotify markets"
> "Commercial Viability"
> "Adherence to Terms"

Source: https://developer.spotify.com/documentation/web-api/concepts/quota-modes

INFERRED — note the circularity, which is fatal for a cold start: you need 250k MAU on a "Launched
Service" to get the access you need in order to launch. The map states the developer is solo with no
audience. A 250k-MAU floor is roughly three orders of magnitude beyond what a $1–3k/mo MRR indie app
would ever reach. And "Adherence to Terms" is listed as a requirement, so even a hypothetical
250k-MAU applicant would still fail on III.5/III.7.

**Commercial-partner / Separate Agreement route: exists in the contract, out of reach in fact.**
VERIFIED — Developer ToS Section IX.1 recognises that the terms are superseded "to the extent that
you have entered into a Separate Agreement", defined verbatim as: "A 'Separate Agreement' is a
written agreement between and executed by Spotify and you governing your use of the Spotify
Platform." Source: https://developer.spotify.com/terms

INFERRED — so the *only* legitimate route to this product is a negotiated bilateral agreement with
Spotify AB. That is the same class of thing the map already ruled out of scope ("Becoming a licensed
broadcaster / negotiating direct label deals ... out of reach for a solo developer at a $1–3k MRR
bar"). NO EVIDENCE FOUND of any self-serve or published-price route to a Separate Agreement.

**Workarounds that are not workarounds** — stated plainly, per the ticket's instruction:

| Proposed workaround | Verdict |
|---|---|
| Don't mix; just hard-cut between Spotify and podcast | **Not a workaround.** III.7 bars "segue". Compliance Tips names segueing explicitly. |
| Keep the third-party audio in a separate "session" the user switches to manually | **Possibly compliant, but it is no longer the product.** If the app cannot programme the sequence, it is just two players with a button between them — the concept's entire value is the programming. |
| Only sequence Spotify tracks, no podcasts | **Not a workaround.** III.7's "(including other Spotify Content)" closes it. Also loses the concept. |
| Launch quietly in development mode and grow | **Not a workaround.** 5-user cap (VERIFIED). Spotify states dev mode "should not be relied on as a foundation for building or scaling a business on Spotify." |
| Use Spotify only for *metadata*, play music from elsewhere | **Arguably compliant, but it is the licensed-catalogue variant, not the Spotify variant.** Note ToS IV.1.b's improper-use bar on using data to build compilations, and Policy III.13's bar on analysing Spotify Content to derive metrics or "building profiles of users" — see §2c. |
| Ask Spotify for permission | Legitimate, and the honest answer. But III.5/III.7 have no written-approval clause, and the developer cannot meet the organizations-only / 250k-MAU gate to even open the conversation. |

---

## 2. Reading user data

**All NEW** — the prior project did not examine the personalisation or library endpoints.

### 2a. The endpoints exist and are documented

VERIFIED. All from https://developer.spotify.com/documentation/web-api/reference/…

| Need | Endpoint | Scope | Notes |
|---|---|---|---|
| Listening history | `GET /me/player/recently-played` | `user-read-recently-played` | "Get tracks from the current user's recently played tracks." |
| Liked / saved tracks | `GET /me/tracks` | `user-library-read` | Still available post-Feb-2026. |
| Top artists & tracks | `GET /me/top/{type}` | `user-top-read` | `time_range`: `long_term` (~1 year), `medium_term` (~6 months, default), `short_term` (~4 weeks). |
| Saved **podcasts** (shows) | `GET /me/shows` | `user-library-read` | Max `limit` 50, `offset` paging. |
| Saved podcast episodes | `GET /me/episodes` | `user-library-read` | Still available post-Feb-2026. |
| Followed artists | `GET /me/following` | `user-follow-read` | Still available post-Feb-2026. |
| Current playback | `GET /me/player`, `GET /me/player/currently-playing` | `user-read-playback-state` | |
| Queue | `GET /me/player/queue` | | |

**Two limits that matter for a radio programmer**, both VERIFIED:

1. `GET /me/player/recently-played` — "**Currently doesn't support podcast episodes.**" INFERRED: so
   the single most important input for interleaving ("where is the user in their podcast listening")
   is *not* in listening history. You can read *saved* shows and episodes, but not podcast play
   history. That is a real product hole independent of the legal question.
2. `limit` on recently-played: "Default: 20. Minimum: 1. Maximum: 50", cursor-paged via
   `before`/`after` Unix-ms timestamps (mutually exclusive). INFERRED: 50 items per call is workable
   for a personalisation seed but there is no bulk history export.

### 2b. Rate limits and quota — the wall

VERIFIED, https://developer.spotify.com/documentation/web-api/concepts/rate-limits: "Spotify's API
rate limit is calculated based on the number of calls that your app makes to Spotify in a rolling 30
second window." The numeric ceiling is **not published**: "The limit varies depending on whether your
app is in development mode or extended quota mode." 429 responses "will normally include a
`Retry-After` header with a value in seconds". NO EVIDENCE FOUND of any published numeric rate limit —
Spotify deliberately does not state one, so capacity planning is impossible from documentation.

VERIFIED, quota-modes page — development mode:
- "Newly-created apps begin in development mode."
- "The app owner must have a Spotify Premium account for apps in development mode to function."
- "**Up to 5 authenticated Spotify users can use an app that is in development mode**" — each must be
  added to an allowlist. Non-allowlisted users' calls "will receive a 403 status code error."
- Separate quota system: 429 with `"reason": "QUOTA_EXCEEDED"`. Per July 2026 changelog, quotas are
  "now counted per developer account rather than per Client ID."

VERIFIED, extended quota mode eligibility — organizations only, 250k MAU minimum (full quotes in
§1e). Review "can take up to six weeks."

**NEW and important — the platform has been tightening, not loosening.** VERIFIED from Spotify's own
blog and changelogs:

- 27 Nov 2024 (https://developer.spotify.com/blog/2024-11-27-changes-to-the-web-api): "Effective
  today, new Web API use cases will no longer be able to access or use the following endpoints and
  functionality in their third-party applications" — listing **Related Artists, Recommendations, Audio
  Features, Audio Analysis, Get Featured Playlists, Get Category's Playlists, 30-second preview URLs
  in multi-get responses, and Algorithmic and Spotify-owned editorial playlists**. Applies to "New
  apps that are registered on or after today's date".
- 6 Feb 2026 (https://developer.spotify.com/blog/2026-02-06-update-on-developer-access-and-platform-security):
  "Going forward, Development Mode will be reduced in scope while continuing to support learning,
  experimentation, and personal projects for **non-commercial use** by individual developers."
  Restrictions from 11 Feb 2026: Premium required; "Developers will be limited to one Development Mode
  Client ID"; "Each Client ID will be limited to up to five authorized users"; "API access will be
  limited to a smaller set of supported endpoints". And verbatim: "Development Mode provides a
  sandboxed environment for learning and experimentation. It is intentionally limited and **should not
  be relied on as a foundation for building or scaling a business on Spotify.**"
  - Update on that page, 9 March: endpoint-access changes for *existing* integrations were postponed,
    but "The Spotify Premium requirement, the authorized user cap and one Client ID per developer
    limit will take effect as planned for existing Development Mode integrations."
  - VERIFIED, July 2026 changelog: client IDs per developer subsequently "Increased from 1 to 25".
    So that one restriction was relaxed. The 5-user cap was not.

INFERRED, and this is decisive for the *Recommendations* piece of the concept: **Recommendations and
Audio Features are gone for any new app.** A radio programmer that wants to pick "one or more liked
songs" that fit a mood after a podcast segment has lost the two endpoints that would have made
musically sensible sequencing possible. The developer would be reduced to sequencing from liked/top
lists with no audio-similarity signal. NEW finding; the prior project did not cover this.

### 2c. Restrictions on retaining or deriving from that data

VERIFIED, Developer ToS (https://developer.spotify.com/terms):

- IV.3.a.i: "Except as otherwise set out in these Developer Terms, you may not store, aggregate or
  create compilations or databases of Spotify Content, other than as strictly necessary to operate
  your SDA. You must use reasonable efforts to ensure that any data you display to users is the most
  up to date data available through the Spotify Platform, and to delete older data. **Do not store
  Spotify Content indefinitely.**"
- V.5: "You will not sell any Spotify Content or other data obtained from Spotify."
- V.7: "You may only process Spotify Personal Data for as long as is necessary to provide your SDA to
  the applicable user, and consistent with the specific consent they've granted to you."
- IV.1.e (transfer to third parties): bars "directly or indirectly transferring any data (**including
  aggregate, anonymous or derivative data**) received from Spotify to, or use such data in connection
  with, any ad network, ad exchange, data broker, or other advertising or monetization-related
  toolset, **even if a user consents**". A named carve-out permits transfer "to third party data
  processors, such as server providers for providing your SDA".
- Note the definition of Spotify Content is extremely broad (ToS II): "any content, data, information
  or material made available through the Spotify Platform ... including ... playlists, metadata ...
  and user data including Spotify Personal Data."

VERIFIED, Developer Policy **III.13**, which the prior project did not flag and which is directly
on-point for a personalisation engine:

> "Do not analyze the Spotify Content or the Spotify Service for any purpose, including without
> limitation, creating new or derived listenership metrics, benchmarking, functionality, usage
> statistics, user metrics, or **building profiles of users**, including for the purpose of targeting
> them with advertising or marketing."

INFERRED — this is a **fifth independent problem**, and it is a serious one that the prior project
missed. A personalised radio programmer works by building a taste profile of the user from their
Spotify listening data and using it to derive a playout order. On its face "building profiles of
users" is what the app does. Two counter-readings are available and I flag both honestly: (i) the
trailing "including for the purpose of targeting them with advertising or marketing" may be read as
limiting the mischief to ad-targeting; (ii) an app that shows a user their own data back is arguably
not "analysing the Spotify Service". Neither reading is safe to build a business on, and the clause
has no written-approval escape hatch. **Where a lawyer is needed.**

VERIFIED, Policy III.14 / ToS IV.1.a.i: "Do not use the Spotify Platform or any Spotify Content to
train a machine learning or AI model or otherwise ingest Spotify Content into a machine learning or AI
model." CARRIED OVER from the prior project as to text; NEW in relevance. The concept step 5 is
"AI-generated intros/outros to feel like a real radio show". INFERRED: generating an AI DJ line that
references the track you are about to play requires feeding that track's metadata (Spotify Content)
into a model. "Otherwise ingest Spotify Content into a machine learning or AI model" reads naturally
onto inference-time prompting, not just training. The prior project left the training-vs-inference
question open; the "otherwise ingest" wording makes the inference reading materially more likely to
bite. This is a **sixth** independent problem, and it hits the AI-DJ feature specifically.

VERIFIED, ToS IX.8.g: on termination you must delete all Spotify Content including Spotify Personal
Data, "(including from your servers)". VERIFIED, Appendix A §5.c: delete applicable personal data
"within five (5) days" of disconnection.

VERIFIED, ToS VI.3 ("App Reviews"): "We may review your SDA for compliance from time to time, in our
sole discretion. You will cooperate with our reviews and provide any information we request thereof."
And VI.5 (Monitoring): "At our sole discretion we may request, and you will provide, proof that your
SDA and any content within your SDA is properly licensed."

INFERRED — that last clause is worth pausing on. The app mixes in "freely broadcastable news
segments" (map, step 4). Spotify can demand proof that this third-party content is properly licensed,
at any time, at its sole discretion. The developer would need to have that paperwork.

---

## 3. Playback control

### 3a. iOS SDK is remote-control only — CONFIRMED, and now confirmed from source

CARRIED OVER from the prior project (which VERIFIED it from the landing page) and **independently
re-verified here at a deeper level: I cloned the SDK and read the public headers.**

VERIFIED, https://developer.spotify.com/documentation/ios: the SDK lets your app "interact with the
Spotify app running in the background as a service", offloading "playback, authentication, networking,
and offline caching"; capabilities include "Always-in-sync Playback (via the Spotify main
application)"; "A physical iOS device is needed to install the Spotify app"; "your application will
need to get the user's permission to control playback remotely first".

VERIFIED, README.md in https://github.com/spotify/ios-sdk (v5.0.1), verbatim:

> "**Can I use this SDK without having the Spotify app installed?** For App Remote functionality
> (playback control), the Spotify app must be installed since it handles the actual music playback."

> "Music must be playing when you connect with `SPTAppRemote` to ensure the Spotify app is not
> suspended in the background. iOS applications can only stay active in the background for a few
> seconds unless they are actively doing something like navigation or playing music."

> "**Note:** A **Spotify Premium** account is required to play tracks on-demand using URIs."

VERIFIED — the complete public method surface of `SPTAppRemotePlayerAPI.h` (read directly from
`SpotifyiOS.xcframework/ios-arm64/SpotifyiOS.framework/Headers/`):

`play:callback:`, `play:asRadio:callback:`, `playItem:callback:`, `resume:`, `pause:`,
`skipToNext:`, `skipToPrevious:`, `seekToPosition:callback:`, `seekForward15Seconds:`,
`seekBackward15Seconds:`, `setShuffle:callback:`, `setRepeatMode:callback:`, `getPlayerState:`,
`subscribeToPlayerState:`, `unsubscribeToPlayerState:`, `enqueueTrackUri:callback:`,
`getAvailablePodcastPlaybackSpeeds:`, `getCurrentPodcastPlaybackSpeed:`,
`setPodcastPlaybackSpeed:callback:`, `getCrossfadeState:callback:`.

VERIFIED, `SPTAppRemotePlayerState.h` exposes: `track`, `playbackPosition` (NSInteger),
`playbackSpeed`, `paused`, `playbackRestrictions`, `playbackOptions`, `contextTitle`, `contextURI`.

VERIFIED **by exhaustive absence** — I grepped the entire public header directory
(`grep -riE "volume|pcm|sampleBuffer|audioSession|AVAudio|duck"`) and it returned **zero matches**.
There is no volume control, no PCM buffer, no sample-buffer callback, no audio-session handle, and no
ducking API anywhere in the iOS SDK's public interface. This upgrades the prior project's INFERRED
"no supported path to the PCM buffer" to VERIFIED-by-header-inspection.

### 3b. Can it pause mid-track, play third-party audio, and resume at position?

**Technically: YES.** VERIFIED, and this is a NEW, sharper answer than the prior project gave.

The mechanism exists and is fully documented:
1. `subscribeToPlayerState:` / `getPlayerState:` → read `playbackPosition` (ms into the current item).
2. `pause:` → Spotify stops.
3. Play your own audio through your own `AVAudioPlayer` — your app's audio, no Spotify surface
   involved.
4. `play:` the same track URI, then `seekToPosition:` with the saved `playbackPosition` — or on Web
   API, `PUT /me/player/play` with `position_ms` (VERIFIED: the endpoint accepts `position_ms`, and
   "This API only works for users who have Spotify Premium").

So the ticket's technical question is answered **affirmatively**: resume-at-position is a first-class,
documented capability.

INFERRED — three practical frictions, none fatal on its own:
- The resume is a fresh `play` + `seek`, i.e. a re-start of the stream, so expect an audible gap and a
  new buffering event. Not a seamless resume.
- iOS backgrounding: per the README, App Remote should be disconnected when your app backgrounds
  ("you should always disconnect App Remote when your app enters a background state"), and connection
  requires music to be playing to keep the Spotify app alive. INFERRED: a programmed session that
  deliberately stops Spotify to play podcast audio, while the user's phone is in their pocket during a
  commute, is fighting the exact lifecycle the SDK documents as fragile. This is a genuine
  reliability risk in the developer's own primary use case.
- Two audio sources means two `AVAudioSession` regimes, one of which (Spotify's) is in another
  process and outside your control.

**Contractually: NO.** INFERRED, high confidence — and this is the point that matters. Doing steps
1–4 *once*, at the user's explicit request, is ordinary remote control. Doing them *as a programmed
sequence* that the app schedules, in order to alternate Spotify music with non-Spotify audio, is
exactly the act III.7 describes ("segue ... any Spotify Content with any other audio content") and
exactly what Compliance Tips lists as "DJ/Mixes". The API's technical permissiveness is not a licence;
ToS IV.1.b.i separately bars "enabling access to, or use of, the Spotify Service or Spotify Content
in violation of the Developer Agreement." **The capability existing does not make its use lawful, and
a technically-available API call is not a workaround for a clause that forbids the pattern.**

Note also `play:asRadio:` in the header — Spotify ships its *own* radio primitive. INFERRED: relevant
to Policy III.11, "Do not build products or services that mimic, or replicate or attempt to replace a
core user experience of Spotify or its group companies without our prior written permission. Your
product or service must add independent value or functionality that improves users' interactions with
Spotify." A personalised radio station built on Spotify's catalogue is arguably replicating Spotify's
own radio/Daylist/AI DJ experience. Unlike III.5 and III.7, **III.11 does have a written-permission
escape hatch** — but it is one more clause to clear, not a route around the others.

### 3c. Web Playback SDK — and is it usable on iOS?

VERIFIED, https://developer.spotify.com/documentation/web-playback-sdk: it is "a client-side only
JavaScript library designed to create a local Spotify Connect device in your browser", to "stream and
control audio tracks from Spotify inside your website and get metadata about the current playback".

- Premium: "The Web Playback SDK requires a Spotify Premium subscription (mobile only types of premium
  subscriptions are excluded)". INFERRED: the exclusion of mobile-only Premium tiers is a real
  addressable-market cut, not a footnote.
- Platforms: "supported by most web browsers (Chrome, Firefox, Safari and Microsoft Edge) in both
  mobile (Android and iOS) and Desktop".
- **CORRECTION to a common assumption:** the docs do *not* say mobile browsers are unsupported. They
  say iOS is supported with a caveat: "iOS support has some limitations", specifically "The playback
  does not start automatically after transfering playback. The user must interact with the SDK events
  to play audio."
- EME: the SDK "require iframes to allow encrypted-media and autoplay in cases of cross origin
  iframes."
- Method surface: `connect`, `disconnect`, `addListener`, `removeListener`, `getCurrentState`,
  `setName`, `getVolume`, `setVolume`, `pause`, `resume`, `togglePlay`, `seek`, `previousTrack`,
  `nextTrack`, `activateElement`. **Note `setVolume`/`getVolume` exist here** — unlike the iOS SDK.

**WKWebView: NO EVIDENCE FOUND.** VERIFIED by absence — "Webviews are not mentioned anywhere on the
page." INFERRED: this is the single most consequential documentation gap for the concept, because a
WKWebView-hosted Web Playback SDK is the *only* architecture in which the app could both (a) control
Spotify volume and (b) run its own audio in the same process. It requires EME/FairPlay inside a
WKWebView plus a documented autoplay-gesture problem, and Spotify does not commit to supporting it.
Treat as unsupported-until-tested. **This is the one empirical test worth running if any Spotify path
were being pursued — which, per §1, it should not be.**

INFERRED, and it must be said plainly: `setVolume` on the Web Playback SDK is the *closest thing to a
ducking primitive* that exists anywhere in Spotify's platform. Using it to fade Spotify down under an
AI DJ voice-over would be a textbook violation of III.7 ("overlap any Spotify Content with any other
audio content"). **It is a technical capability, not a permission.**

---

## 4. Crossfading and mixing

### 4a. No decoded-audio surface — CONFIRMED and upgraded to VERIFIED

CARRIED OVER from the prior project as INFERRED; **upgraded to VERIFIED here** by direct inspection.

- iOS SDK: VERIFIED by exhaustive grep of the public headers — no `volume`, no `pcm`, no
  `sampleBuffer`, no `AVAudio*`, no ducking. Zero matches. Playback happens inside the Spotify app's
  own process; your app never touches the audio graph.
- Web Playback SDK: VERIFIED — playback runs through the browser's EME/DRM pipeline ("require iframes
  to allow encrypted-media"). INFERRED: EME exists specifically to prevent the host page from reading
  decoded frames. There is no supported route to the PCM.
- VERIFIED, ToS IV.1.a.ii bars "modifying, editing, altering, creating derivative works,
  disassembling, decompiling, reverse-engineering, or extracting source code from the Spotify Platform
  ... Spotify Service, or Spotify Content". INFERRED: any circumvention route to the buffer is barred
  contractually as well as blocked technically. Two independent locks.

### 4b. There *is* a crossfade API — and it proves the point rather than helping

**NEW.** VERIFIED — the iOS SDK exposes `getCrossfadeState:callback:`, returning an
`SPTAppRemoteCrossfadeState` whose entire public interface is two **read-only** properties:
`enabled` (BOOL) and `duration` (NSInteger). Source: `SPTAppRemoteCrossfadeState.h` in
https://github.com/spotify/ios-sdk.

INFERRED — this is the cleanest possible evidence of Spotify's design intent. Crossfade exists as a
*user* setting inside the Spotify app; a third-party app is permitted to **observe** it and nothing
more. There is no setter. You cannot enable it, set its duration, or trigger it. And it would not help
regardless: Spotify's crossfade operates between two Spotify tracks inside Spotify's own player. It
cannot crossfade into your podcast audio, because your podcast audio does not exist inside Spotify's
player.

### 4c. What is the best achievable transition — and is even that prohibited?

**Best achievable: a hard cut with an audible gap.** INFERRED from the surface established above:
`pause:` → play your audio → `play:` + `seekToPosition:`. There is no overlap, no fade, and the resume
is a fresh stream start, so expect a perceptible gap (buffering + connection latency). On the Web
Playback SDK you additionally get `setVolume`, so a *volume ramp* on the Spotify side is technically
possible — but see below.

**And yes — even the hard cut is prohibited.** This is the ticket's sharpest question and the answer is
unambiguous:

VERIFIED, Policy III.7: "Do not permit any device or system to **segue**, mix, re-mix, or overlap any
Spotify Content with any other audio content (including other Spotify Content)."

INFERRED, high confidence — "segue" is a distinct verb from "mix", "re-mix" and "overlap", and in
radio and DJ usage it means precisely a *transition from one item to the next*, with or without
overlap. A drafter listing four verbs did not intend "segue" to be a synonym of "overlap"; it is there
to catch sequential transitions that the other three verbs would miss. And VERIFIED, Compliance Tips
uses the identical four verbs to define the disallowed "DJ/Mixes" category.

So the ordering of possibilities is:

| Transition | Technically possible? | Permitted? |
|---|---|---|
| Overlap / true crossfade with third-party audio | **No** — no PCM surface, no volume control on iOS | No — III.7 "overlap" |
| Duck Spotify under an AI DJ voice | Only via Web Playback SDK `setVolume`, iOS-in-webview unverified | No — III.7 "overlap" |
| Hard cut, gap, sequential playback | **Yes** — `pause:` / `play:` / `seekToPosition:` | **No — III.7 "segue"** |
| User manually leaves your app, opens Spotify, comes back | Yes | Yes, but it is not the product |

**Conclusion for §4: the best *achievable* transition and the best *permitted* transition do not
overlap at all.** Everything the platform lets you build here, the policy forbids. Additionally
VERIFIED, Policy III.6 bars synchronising "any sound recordings with any visual media" — not directly
applicable to an audio-only app, but it forecloses any visual/lyric/video companion layer.

---

## 5. Offline and caching

### 5a. ToS IV.3.b re-verified verbatim

CARRIED OVER from the prior project and **re-verified as current, verbatim**. VERIFIED,
https://developer.spotify.com/terms:

> "**Local caching.** Do not locally cache any Spotify Content, except as strictly necessary to enhance
> the performance of your SDA and its functionality, and limited to the temporary caching of:
> [i] metadata and cover art; or
> [ii] Conditional Downloads of sound recordings. Caching of Conditional Downloads of sound recordings
> must only be available to subscribers to the Premium Service. '**Conditional Downloads**' means
> time-limited offline syncing."

Adjacent, VERIFIED, IV.3.a.i: "you may not store, aggregate or create compilations or databases of
Spotify Content, other than as strictly necessary to operate your SDA ... **Do not store Spotify
Content indefinitely.**"

### 5b. What this means for the commute — a second, independent blocker

The map flagged this as a possible independent blocker. **It is one.**

INFERRED, and the reasoning is short. The `Conditional Downloads` limb looks like an opening, but it
closes on inspection:

1. The third-party app cannot perform a Conditional Download itself. VERIFIED, §3a: on iOS the SDK
   "offload[s] complexity to the main Spotify application such as: playback, authentication,
   networking, and **offline caching**". Offline caching is explicitly named as a Spotify-app
   responsibility, and there is **no download, prefetch, or offline API anywhere in the public SDK
   headers** (VERIFIED by inspection in §3a/§4a). NO EVIDENCE FOUND of any Web API or SDK endpoint that
   initiates an offline download.
2. So "Conditional Downloads" describes the Spotify app's own Download toggle, which the *user* may
   set inside the *Spotify app*. A third-party app can neither trigger it nor rely on it.
3. VERIFIED, Web Playback SDK: it is "client-side only ... in your browser" running through EME. There
   is no offline mode.

**Result.** The music leg of the stream is online-only and outside the developer's control; the
podcast leg (own player over open RSS) *can* be cached freely, since no Spotify terms attach. INFERRED:
this produces the worst possible failure mode for the stated use case — in a tunnel or a dead spot the
podcast keeps playing perfectly and the music silently fails. The developer's own primary scenario
(commuting) is the scenario where the product is least reliable, and the asymmetry will read to users
as the app being broken.

INFERRED — worth stating: this blocker is **architectural, not contractual**. Even if Spotify granted a
Separate Agreement blessing the interleaving, there is still no third-party offline surface to build
on. §1 and §5 fail for genuinely independent reasons.

---

## 6. Attribution and UX requirements

**All VERIFIED from https://developer.spotify.com/policy unless noted. Mostly NEW** — the prior project
quoted the cover-art clause only.

### 6a. Mandatory attribution and link-backs

Policy **II.4** ("Attribution"):

> "If you display any Spotify Content you must clearly attribute the content as being supplied and made
> available by Spotify, by using the Spotify Marks. Our Branding Guidelines include some examples of
> what you should and shouldn't do."
> "Metadata, cover art and Audio Preview Clips must be accompanied by a link back to the applicable
> album, content or playlist on the Spotify Service."
> "You must not offer metadata, cover art, and/or Audio Preview Clips as a standalone service or
> product."

Policy **II.5** ("Display metadata and cover art when streaming"):

> "If you offer a Streaming SDA, metadata and cover art may only be used in connection with the
> underlying content, and there shall be no playback of Spotify Content without showing relevant cover
> art and metadata in your SDA."

INFERRED — II.5 is a genuine UX constraint for a *radio* app, though not a fatal one. A lean-back,
screen-off, in-car radio experience must still render cover art and metadata whenever Spotify audio is
playing. That is satisfiable (a now-playing screen), but it means the app can never be purely audio,
and CarPlay templating would need to carry it.

Policy **VI** ("Naming and Branding"): use of Spotify Marks requires following the Branding Guidelines;
the SDA's name "should not begin with 'Spot' or be confusing in sound or spelling to Spotify"; and
"Unless you have applicable permissions, don't imply any endorsement, tie-in, co-branding or promotion
by Spotify."

Policy **II.1** ("Endorsement and promotion"): don't imply "any endorsement, tie-in, co-branding or
promotion by Spotify ... or any creator, artist, rightsholders, or sound recording", and "your SDA must
not have a specific artist or artists as the sole subject of the SDA unless you have applicable
permissions."

VERIFIED, ToS VII.1.d: "you may not remove or alter any copyright, copyright protection technology,
trademark, or other intellectual property notice contained in or provided through Spotify's
Intellectual Property."

VERIFIED, ToS IX.7: you can't imply "a partnership with, sponsorship by, or endorsement by Spotify"
without prior written approval.

**"Powered by Spotify": NO EVIDENCE FOUND.** VERIFIED by absence — that exact phrase does not appear in
the Developer Policy. The requirement is the more general "attribute ... by using the Spotify Marks"
plus adherence to the Branding Guidelines.

### 6b. Primary-player prohibition — NO EVIDENCE FOUND, but adjacent clauses bite

**NO EVIDENCE FOUND** of any clause prohibiting the SDA from appearing to be the primary player. That
specific concern is not in the Developer Policy.

But two adjacent clauses do constrain the posture, and one is from the Design Guidelines
(https://developer.spotify.com/documentation/design), which is a primary source:

- VERIFIED, Policy III.11: "Do not build products or services that mimic, or replicate or attempt to
  replace a core user experience of Spotify or its group companies without our prior written
  permission. Your product or service must add independent value or functionality that improves users'
  interactions with Spotify." INFERRED: an app whose whole purpose is to *be* the listening session is
  the closest thing to replacing a core Spotify experience. Written permission is theoretically
  available here, unlike for III.5/III.7.
- VERIFIED, Design Guidelines, on multi-provider surfaces: "Spotify content should never be seated next
  to content from similar services", and "Pairing of brands is not permitted under our Developer
  Terms." INFERRED — the first of these is a *design-guideline* restatement of III.5's logic, and it
  bears directly on the concept: a now-playing UI that alternates a Spotify track and a third-party
  podcast is, quite literally, Spotify content seated next to content from another service.

### 6c. The clause that independently kills the business model

**NEW, and it is the second-most important finding in this document after §1.**

VERIFIED, Policy **IV.2** ("Commercial use restrictions"), in full:

> "Except for the limited commercial uses for Non-Streaming SDAs (set out below) commercial uses are
> not permitted for SDAs. Some examples of commercial uses that are not allowed include:
> [a] the sale, whether integrated or as a standalone product, of a Streaming SDA, the Spotify
> Platform, Spotify Content or Spotify Service, or sale of access to any of the foregoing;
> [b] any e-commerce (e.g., in-app payment or monetization) initiated via a Streaming SDA;
> [c] the sale of advertising, sponsorships, or promotions on the Streaming SDA itself."

VERIFIED, Policy IV.3 — the *only* permitted commercial uses are for **Non-Streaming** SDAs: selling
ads/sponsorships on them, and "the sale of, or sale of access to, a Non-Streaming SDA".

VERIFIED, ToS Section II definitions:

> "'**Streaming**' means using the Spotify Platform to enable playback of sound recordings available
> through the Spotify Service, **including using the Spotify Platform to control a background Spotify
> application**."
> "'**Streaming SDA**' is an SDA or Approved Device that provides Streaming functionality or some subset
> of Streaming functionality, **regardless of whether it also provides additional functionality**."

INFERRED — the definitions are drafted to close exactly the escape the developer would reach for. "You
are only *controlling* Spotify, not streaming" fails: controlling a background Spotify application *is*
Streaming by definition. "Most of my app is podcasts, the music is a small part" fails: "regardless of
whether it also provides additional functionality". This app is a Streaming SDA and cannot be
monetised.

VERIFIED, Compliance Tips confirms the classification with a list of what counts, verbatim:

> "Streaming apps include: Apps using Web Playback SDK to play music • Apps using our
> /v1/me/player/play endpoint to play music • Apps using the /v1/me/player/next endpoint where music
> could be next • ... • Apps using the iOS or Android SDKs to start, stop or modify playback of music •
> Apps using any other Spotify API or SDK to control playback of music or stream music"

Every single one of those describes this app.

VERIFIED, Compliance Tips, "Commercial Use", disallowed examples:

> "Any commercialization of streaming apps such as: **Charging users $5/month for a home automation app
> that includes the ability to trigger music with the press of a button** • Creating a Spotify player
> with the Web Playback SDK that includes advertising served via Google Ads • ..."

And the permitted examples, for contrast:

> "Selling access to a non-streaming app such as: Making a playlist manager app and charging users
> $5/month to use it. • Building an app that notifies users when a podcast episode is released with
> their chosen keyword in the description, and charging $2 for it in an app store. • Advertising served
> via Google Ads on a non-streaming app."

INFERRED — read those two lists side by side. Spotify's own disallowed example is an app that is mostly
about something else and "includes the ability to trigger music", charging $5/month. That is
structurally identical to a $5/month radio app that is mostly about podcast programming and includes
the ability to play the user's liked songs. The permitted examples are all *non-streaming*: playlist
managers, notification apps, discovery websites.

**So: the map's success bar cannot be reached through Spotify by any permitted route.** Subscription
revenue, in-app purchase, and in-app advertising are each named as prohibited for a Streaming SDA. Even
if §1's mixing prohibition somehow evaporated, the developer would be left with an app that is
contractually forbidden to earn money. VERIFIED-grounded and independent of every other finding in this
document.

Also VERIFIED, ToS III.1.a: the licence to develop and distribute SDAs is granted "(i) for private
personal use; and (ii) on Approved Devices; and (iii) in accordance with the Developer Policy" — the
licence itself is conditioned on policy compliance, so a III.5/III.7 breach is not merely a breach of a
covenant, it is operating outside the licence grant.

---

## 7. Hard blockers vs workarounds

### Hard blockers — no workaround exists

Each of these is independently sufficient to kill the Spotify variant.

| # | Blocker | Basis | Waivable? |
|---|---|---|---|
| B1 | Interleaving Spotify audio with third-party audio in one programmed session | Policy **III.7** ("segue, mix, re-mix, or overlap"), reinforced by Compliance Tips "DJ/Mixes" | **No written-approval clause exists** |
| B2 | Being a product integrated with another service's streams/content | Policy **III.5** | **No written-approval clause exists** |
| B3 | Charging money, running ads, or any in-app monetisation | Policy **IV.2** + ToS II definition of Streaming SDA + Compliance Tips $5/month example | No |
| B4 | More than 5 users | Quota modes: dev mode 5-user cap; extended quota mode is organizations-only, ≥250k MAU | No — eligibility, not discretion |
| B5 | Offline/cached music for the commute | ToS **IV.3.b**; no offline API in any SDK | No — architectural |
| B6 | Any mixing, ducking, or crossfade | No PCM surface (VERIFIED by header inspection); ToS IV.1.a.ii bars circumvention | No |

Two further clauses I rate as **probable blockers**, flagged honestly as less certain:

- **B7 — Policy III.13**, "Do not analyze the Spotify Content or the Spotify Service for any purpose,
  including ... building profiles of users". A personalisation engine builds a taste profile. Two
  narrower readings exist (§2c); neither is safe. **Lawyer question.**
- **B8 — Policy III.14 / ToS IV.1.a.i**, no using Spotify Content to "train a machine learning or AI
  model or otherwise ingest Spotify Content into a machine learning or AI model". The AI-DJ feature
  (concept step 5) needs track metadata in a prompt. "Otherwise ingest" reads onto inference.
  **Lawyer question.** CARRIED OVER as an open question from the prior project; the "otherwise ingest"
  wording makes the adverse reading more likely than the prior project assumed.

### Things that look like workarounds but are not

Per the ticket's instruction — a workaround that violates a clause is not a workaround.

1. **"Hard cut instead of crossfade."** Violates III.7's "segue". Not a workaround.
2. **"Only sequence Spotify tracks, no podcasts."** Violates III.7's "(including other Spotify
   Content)". Not a workaround, and abandons the concept.
3. **"Stay in development mode."** Complies, but caps the product at 5 users, and Spotify states dev
   mode "should not be relied on as a foundation for building or scaling a business on Spotify." Not a
   business.
4. **"Free app, monetise elsewhere."** IV.2 bars "any e-commerce (e.g., in-app payment or monetization)
   initiated via a Streaming SDA" and any sale of "access to" it. INFERRED: a paywall outside the app
   that gates the app is still selling access. And it does not touch B1/B2/B4 anyway.
5. **"Use `setVolume` on the Web Playback SDK to duck."** A capability, not a permission. III.7
   "overlap". Not a workaround.
6. **"Ship it quietly; enforcement is unlikely."** Even accepting the premise, extended quota mode
   requires Spotify's review team to "test out your app" against the Developer Policy. The gate
   precedes the enforcement. Not a workaround.
7. **"Spotify for metadata only; play music from a licensed catalogue."** This is *arguably*
   compliant — but note it is no longer the Spotify variant; it is the **licensed-catalogue variant
   with a Spotify login bolted on**, and it still runs into III.13 (profiling), the 5-user cap (B4),
   and ToS IV.3.a's "Do not store Spotify Content indefinitely". INFERRED: the Spotify login would add
   legal exposure while adding little product value over asking the user their taste directly. Not
   recommended as a bridge.
8. **"Get a Separate Agreement."** The only genuinely legitimate route (ToS IX.1). Requires a
   negotiated written agreement with Spotify AB, gated behind an organizations-only, ≥250k-MAU
   application. Out of scope per the map, and out of reach in fact.

### The one thing that is genuinely feasible

INFERRED — a **Spotify-linked companion** that is a *Non-Streaming SDA*: reads the user's saved shows
and liked tracks to *build and export a Spotify playlist*, or to *deep-link* into Spotify, while your
own player handles only open-RSS podcast audio, and never programmes music playback. That is
monetisable (IV.3 permits selling access to a Non-Streaming SDA — Spotify's own permitted example is
"Making a playlist manager app and charging users $5/month"). But it is a playlist tool, not a radio
station: it cannot interleave, cannot control the transition, and cannot deliver the concept's core
promise. Still capped at 5 users (B4) unless the developer becomes an organization with 250k MAU.

---

## 8. Where a lawyer would be required

Listed only for completeness. INFERRED — **none of these is worth paying for**, because B1–B6 are not
close calls and B3 alone forecloses the business model. A lawyer would be needed only if the developer
intended to pursue a Separate Agreement, which the map places out of scope.

1. Whether III.13's "building profiles of users" reaches a personalisation engine, or is limited by its
   ad-targeting tail.
2. Whether III.14's "otherwise ingest Spotify Content into a machine learning or AI model" reaches
   inference-time prompting with track metadata.
3. Whether an external paywall gating a Streaming SDA is "sale of access" under IV.2.a.

## 9. What I could not verify — stated plainly so it is not mistaken for a finding

- **Enforcement in practice.** NO EVIDENCE FOUND, and this is a genuine gap: my web-search tool errored
  on every attempt during this pass, so I gathered **no SECONDARY evidence** of named apps suspended
  under III.5 or III.7. My "prohibited in practice" reasoning rests on the primary-source fact that
  extended quota mode requires a policy-compliance review, not on observed enforcement. If the developer
  wants that evidence, the Spotify Developer Community forum is the place to look.
- **Numeric rate limits.** Not published by Spotify in either mode. Capacity planning is impossible from
  documentation.
- **Web Playback SDK inside a WKWebView on iOS.** Not mentioned anywhere in Spotify's documentation.
  Untested here. The only empirical test that would matter, and only if §1 were somehow resolved.
- **Whether App Store review would independently object.** Not examined in this pass; the prior
  project's Guideline 5.2.2 finding ("ensure that you are specifically permitted to do so under the
  service's terms of use. Authorization must be provided upon request") CARRIES OVER and would
  INFERRED-independently bite, since Spotify's terms demonstrably do not permit this. Not re-verified
  here.
- **The Branding Guidelines document itself.** I read the Design Guidelines page but did not audit the
  full Branding Guidelines for specific logo/placement requirements. Low stakes given the verdict.
- **Whether III.4's "non-interactive internet webcasting" would be applied to a single-listener
  personalised station.** My reading is that it probably would not be the clause Spotify reaches for;
  III.7 is. Flagged as the weakest of the three.

---

## 10. Findings inventory: carried over vs new

**CARRIED OVER from `podcasts-notes-validation/docs/research/01-audio-access-legality.md`, and
re-verified as current:**

- iOS SDK is remote-control only, requires the Spotify app installed. *Confirmed, and deepened from
  landing-page prose to header-level inspection.*
- Policy "Do not create any product or service which is integrated with streams or content from another
  service." *Confirmed verbatim, now numbered III.5.*
- Policy "Do not permit any device or system to segue, mix, re-mix, or overlap..." *Confirmed verbatim,
  now numbered III.7.*
- Policy "there shall be no playback of Spotify Content without showing relevant cover art and
  metadata". *Confirmed verbatim, now numbered II.5.*
- ToS IV.3.b local caching, metadata/cover-art and Conditional Downloads only. *Confirmed verbatim.*
- ToS IV.1.a.ii no reverse-engineering; IV.1.a.i / Policy III.14 AI-model clause; IV.3.a.i no
  indefinite storage; IX.8.g deletion on termination. *All confirmed.*
- No decoded-audio surface anywhere. *Confirmed and upgraded from INFERRED to VERIFIED.*

**NEW in this pass:**

- Clause numbering throughout, enabling precise citation.
- **Compliance Tips is a primary source and names "DJ/Mixes" as a disallowed use case** — Spotify's own
  published interpretation of III.7. Decisive.
- **Policy IV.2 + the ToS "Streaming SDA" definition together prohibit monetisation**, and Compliance
  Tips' "$5/month home automation app" example maps onto this concept. Independently fatal to the MRR
  bar.
- **Extended quota mode is organizations-only (since 15 May 2025) with a ≥250k-MAU floor**; dev mode is
  capped at 5 authorised users and Spotify says it "should not be relied on as a foundation for building
  or scaling a business".
- **Recommendations, Audio Features, Audio Analysis and Related Artists are unavailable to any new app**
  (27 Nov 2024 blog) — removes the signal a music programmer would need.
- **Feb 2026 dev-mode reduction** — Premium required, user cap, "non-commercial use" framing.
- **Policy III.13** bars "building profiles of users" — a new, serious, previously-unflagged problem for
  a personalisation engine.
- **Policy III.4** (non-interactive webcasting) and **III.11** (mimicking a core Spotify experience) as
  additional applicable clauses.
- **III.5 and III.7 have no written-approval escape hatch**, whereas III.1 and III.11 do — a deliberate
  drafting signal.
- `getCrossfadeState:` is **read-only**; `SPTAppRemoteCrossfadeState` has no setter.
- Resume-at-position **is** technically supported (`seekToPosition:`, `playbackPosition`,
  `position_ms`) — a sharper technical answer than the prior project gave, and it does not help.
- Web Playback SDK **does** claim iOS/mobile Safari support (with an autoplay-gesture caveat), and
  **does** expose `setVolume` — correcting a common assumption. WKWebView is undocumented.
- `recently-played` **does not cover podcast episodes** — a product hole independent of the legal one.
- ToS VI.5: Spotify may demand "proof that your SDA and any content within your SDA is properly
  licensed" — bears on the "freely broadcastable news segments" leg.
