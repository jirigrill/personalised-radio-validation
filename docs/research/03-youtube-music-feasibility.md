# 03 — YouTube Music: feasibility verdict

Research for ticket `issues/03-youtube-music-feasibility.md`. **Not legal advice.** Every claim is
tagged VERIFIED (read in a primary source, URL given), INFERRED (my reasoning from those sources),
or NO EVIDENCE FOUND. Each is also marked CARRIED OVER (from
`../podcasts-notes-validation/docs/research/01-audio-access-legality.md`, re-verified in this pass)
or NEW.

## Verdict up front

**BLOCKED — both by policy and by absence of API. Redundantly, and by more clauses than prior work
found. This is the cleanest dead end in the platform sweep.**

| Requirement of the concept | Status | Governing clause / absence |
|---|---|---|
| Third-party playback of YouTube Music tracks | **Blocked by absence of API** | No YouTube Music playback SDK exists (VERIFIED by absence, NEW confirmation) |
| Background playback (screen off, commuting) | **Blocked by policy** | Developer Policies III.I.9 (VERIFIED, CARRIED OVER) |
| Audio-only music (no video on screen) | **Blocked by policy** | III.I.7, III.I.8 (VERIFIED, CARRIED OVER) |
| Programmed sequencing: podcast → song → podcast | **Blocked by policy** | III.I.7 (mixing/altering), III.I.1 substitute-service ban (VERIFIED, **NEW**) |
| Buffering ahead / offline for commute | **Blocked by policy** | III.E.1(a) and (b) (VERIFIED, CARRIED OVER) |
| Read user's YouTube Music library / liked songs | **Blocked by absence of API** (partial: YouTube *channel* likes only) | Data API `channels.contentDetails.relatedPlaylists.likes` exists but is YouTube video likes, not a Music library (VERIFIED, **NEW**) |
| Read listening / watch history | **Blocked by absence of API** | `watchHistory` / `watchLater` fully deprecated and removed (VERIFIED, **NEW**) |
| Non-API route to the audio (yt-dlp etc.) | **Blocked by policy** | III.I.14, III.E.6 (VERIFIED, CARRIED OVER) — a violation, not a workaround |
| IFrame Player API as a route | **Blocked by policy** | Visible-viewport minimums + no-overlay/no-obscure rules in Required Minimum Functionality, plus III.I.9 (VERIFIED, **NEW**) |
| YouTube podcast hosting as a route | **Changes nothing** | Podcast episodes are ordinary YouTube videos in a Studio container; identical clauses apply (VERIFIED, **NEW**) |

Characterisation: **both**. The two failures are independent — even if YouTube shipped a playback
SDK tomorrow, III.I.7 and III.I.9 would still bar audio-only and background use; and even if those
clauses were relaxed, there would be no API to call. Neither branch is a workaround for the other.

**Currency of the clauses.** VERIFIED, NEW — the YouTube API Services Developer Policies page states
"Last updated 2026-06-24 UTC" and the API Services Terms of Service page states "Last updated
2026-04-28 UTC". All four clauses prior work relied on (III.E.1(a), III.I.7, III.I.9, III.I.14) are
present and unchanged in the current text. Nothing has loosened.

## 1. Technical surface — is there any playback route?

VERIFIED by absence, NEW confirmation of a CARRIED OVER finding — Google's developer landing page
for YouTube (https://developers.google.com/youtube) enumerates the complete set of developer
surfaces: embedded players (IFrame, iOS, player parameters), YouTube Data API v3, Analytics &
Reporting APIs, subscribe buttons, the Live Streaming API, and the YouTube Researcher Program.
**There is no YouTube Music entry and no YouTube Music developer programme.** YouTube Music is a
consumer product with no developer platform of its own.

VERIFIED, NEW — the one official iOS artefact, `youtube-ios-player-helper`, is described as a
"Lightweight helper library that allows iOS developers to add inline playback of YouTube videos
through a WebView." It is a WebView wrapper around the IFrame player, not an audio pipeline. The
repository "was archived by the owner on Dec 5, 2024" and is read-only.
Source: https://github.com/youtube/youtube-ios-player-helper

INFERRED — the only playback primitive available to a third party is a *visible video player*. The
concept requires a continuous audio programme with the screen off. Those are not the same object,
and no amount of engineering converts one into the other within sanctioned means.

## 2. The policy clauses, verbatim and current

All quotes in this section are from the **YouTube API Services — Developer Policies**,
https://developers.google.com/youtube/terms/developer-policies, page-dated **"Last updated
2026-06-24 UTC"**.

Caveat on quotation, stated plainly: the fetch tool used caps individual quoted spans, so the
fragments below are verbatim clause text but not always the complete sentence including
sub-clause preamble. Every fragment is the operative prohibition; none is paraphrase.

**III.E.1** — prefaced by a prohibition on doing, or encouraging/enabling/requiring others to do,
the listed acts:
- (a) "download, import, backup, cache, or store copies of YouTube audiovisual content without
  YouTube's prior written approval" — VERIFIED, CARRIED OVER.
- (b) "make content available for offline playback" — VERIFIED, CARRIED OVER.

**III.E.6** — must not "scrape YouTube Applications or Google Applications, or obtain scraped
YouTube data or content." VERIFIED, CARRIED OVER.

**III.I.7** — must not "separate, isolate, or modify the audio or video components of any YouTube
audiovisual content" made available via the API Services. The document's own worked example is that
you "must not apply alternate audio tracks to videos." VERIFIED, CARRIED OVER.

**III.I.8** — must not "promote separately the audio or video components of any YouTube audiovisual
content" provided in connection with the API Services. VERIFIED, CARRIED OVER.

**III.I.9** — must not "create, include, or promote features that play content, including audio or
video components, from a background player", where a background player is one not shown in the page,
tab, or screen the user is viewing. VERIFIED, CARRIED OVER.

**III.I.14** — must not "use any technology other than YouTube API Services to access or retrieve
API Data," including any portion of YouTube audiovisual content. VERIFIED, CARRIED OVER.

### NEW clauses prior work did not find, and they matter

**III.I.1** — must not use the APIs to "create, offer, or act as a substitute for, or substantially
similar service to, any YouTube Applications", adding that "API Clients must not mimic or replicate
YouTube's core user experiences by recreating features or process flows" absent significant
independent value. VERIFIED, **NEW**.

INFERRED — this is the most conceptually on-point clause in the document and prior work missed it.
YouTube Music's own core user experience *is* algorithmically programmed radio ("Your Mix", auto-
generated radio stations, mixed music-and-podcast playback). An app that programmes a personalised
radio station from a user's YouTube Music library is close to a definitional "substantially similar
service". This is not the marginal clause — it is a second, independent policy kill that does not
depend on the audio-separation or background-player arguments at all.

**III.I.6** — must not modify, build upon, or block any part of a YouTube player. VERIFIED, **NEW**.
INFERRED: forecloses hiding, shrinking, or wrapping the player to approximate audio-only.

**III.I.15** — bars offering "an API service that allows users to access any data or functionality"
from the YouTube APIs. VERIFIED, **NEW**. INFERRED: relevant only if the app exposed its own API;
not binding on this concept as described.

**III.I.2** — "you must not automate or trigger views, uploads, comments, likes, dislikes, or other
actions" without the user's express consent. VERIFIED, **NEW**. INFERRED: a radio programmer that
auto-advances to the next track is programmatically triggering views. Arguably curable by an
explicit user consent flow, but it is a compliance surface the concept did not anticipate.

**III.F.3.b** — must not "gate access to a video by requiring a user to take an action other than
clicking the play button." VERIFIED, **NEW**.

**API Services Terms of Service §16.3** — "no rights or licenses are granted to reproduce or
distribute audiovisual content or make audiovisual content available", the only permitted route
being "through the use of the YouTube API Services in accordance with the Agreement", with "All
rights not expressly granted to you are reserved by YouTube." VERIFIED, CARRIED OVER. Page-dated
"Last updated 2026-04-28 UTC".
Source: https://developers.google.com/youtube/terms/api-services-terms-of-service

### Do these bar *this* concept as opposed to the podcast-capture concept?

**Yes — more directly.** INFERRED, and this is the substantive NEW judgement of this ticket:

Prior work's concept needed to *capture* audio, so the load-bearing clauses there were the
caching/storing ones, III.E.1(a) and III.I.7. This concept does not need to capture anything — but
it needs the two things the policies name most explicitly:

1. **Background playback.** The commuting use case in `map.md` is the developer's own primary
   scenario: screen off, in a pocket or on a dashboard. III.I.9 prohibits exactly this by name. There
   is no consent-based carve-out, no paid tier that unlocks it, and no configuration flag. (YouTube
   Premium grants *the end user* background playback inside YouTube's own apps; VERIFIED by absence —
   nothing in the Developer Policies extends that permission to an API Client, whatever the user's
   subscription status.)
2. **Audio-only music.** III.I.7 bars separating or isolating the audio component; III.I.8 bars
   promoting the audio component separately. A music radio app is, definitionally, the audio
   component presented alone.

So the concept is worse off here than the podcast-capture concept was, not better. It moves the
violation from a feature the app performs occasionally (capture) to the app's continuous operating
mode.

INFERRED — and this is the point on which the developer should not be encouraged: there is no
architecture that satisfies III.I.7 and III.I.9 while delivering the product. A visible, on-screen,
never-backgrounded video player that shows each music video in full is compliant, and is not the
product. "Show a full-screen video the user isn't looking at" is not a workaround — it is either a
compliant product nobody wants or, if the screen is allowed to lock, a III.I.9 violation.

## 3. The ticket's four questions

### Q1 — Does the YouTube Data API expose a user's YouTube Music library, liked songs, or history?

**No, on all three. Blocked by absence of API.**

VERIFIED, **NEW** — the `channels` resource's `contentDetails.relatedPlaylists` object documents
exactly three properties: `likes`, `uploads`, and `favorites`. `likes` is "The ID of the playlist
that contains the channel's liked videos." `uploads` is "The ID of the playlist that contains the
channel's uploaded videos." `favorites` carries "This property has been deprecated."
Source: https://developers.google.com/youtube/v3/docs/channels

INFERRED — `likes` is a *YouTube* liked-videos playlist, not a YouTube Music library. A YouTube Music
"liked song" and a liked YouTube video are different objects in different products; NO EVIDENCE FOUND
that Music likes surface in this playlist, and no documentation asserts they do. Even if there were
partial overlap, the playlist is undifferentiated — it would contain tutorials, talks and vlogs
alongside music with no reliable way to filter to *songs*, and no Music-library metadata (album,
track, artist as first-class fields).

VERIFIED, **NEW** — `watchHistory` and `watchLater` are gone. The Data API revision history records
them as announced deprecated on **11 August 2016**, effective **12 September 2016** (after which
`contentDetails.relatedPlaylists.watchHistory` returned only the fixed value `HL` and retrieval
requests returned nothing), with the **9 September 2020** entry recording that they are "fully
deprecated, and the two properties have been removed from the documentation."
Source: https://developers.google.com/youtube/v3/revision_history

INFERRED — this is decisive independently of every policy clause. The concept's step 2 is "App reads
listening history, liked songs, followed/liked podcasts." **Listening history is not obtainable.**
There is no API, deprecated or otherwise, that returns what a user has listened to. Personalisation
without history is not the concept.

NO EVIDENCE FOUND of any YouTube Music-specific endpoint, scope, or resource anywhere in Google's
developer documentation. There is no `music` resource, no Music library scope, and no Music
subscription-status API.

Stated as a gap: unofficial reverse-engineered libraries (e.g. `ytmusicapi`) do expose Music library
and history data. VERIFIED as a policy matter, CARRIED OVER — these operate by calling YouTube
Music's internal endpoints, which III.I.14 ("must not use any technology other than YouTube API
Services to access or retrieve API Data") and III.E.6 (scraping) prohibit. **This is a violation,
not a workaround, and it is not a route.** It also has no stability guarantee and would put the app
inside App Store Guideline 5.2.2's "specifically permitted ... under the service's terms of use"
requirement with no defensible answer.

### Q2 — Does the IFrame Player API offer anything? Is audio-only or background use permitted?

**It offers video playback control only. Audio-only and background use are not permitted. Confirmed
as expected.**

VERIFIED, **NEW** — the IFrame Player API reference documents queueing, "play, pause, or stop those
videos", volume adjustment, metadata retrieval, and player event subscription. The page says nothing
about audio-only mode, background playback, or hidden players — there is no such documented support.
Source: https://developers.google.com/youtube/iframe_api_reference

VERIFIED, **NEW** — the **Required Minimum Functionality** document imposes affirmative visibility
requirements that are independently fatal:
- "Embedded players must have a viewport that is at least 200px by 200px."
- If controls are displayed, the player "must be large enough to fully display the controls without
  shrinking the viewport below the minimum size."
- "an API Client must not initiate an automatic playback until the player is visible and more than
  half of the player is visible"
- "A page or screen must not have more than one YouTube player that automatically plays content
  simultaneously."
- "You must not display overlays, frames, or other visual elements in front of any part of a YouTube
  embedded player", nor use such elements "to obscure any part of an embedded player, including
  player controls."
- "You must not make changes to the YouTube player that are not explicitly described by the API
  documentation."

Source: https://developers.google.com/youtube/terms/required-minimum-functionality

INFERRED — taken together with III.I.9 and III.I.6, this closes every engineering trick: a 1×1
player, an off-screen player, a player behind an opaque radio-UI overlay, a player in a hidden
WebView, or a screen-locked player are each separately non-compliant. The autoplay rule also breaks
the gapless podcast→song→podcast transition the concept depends on, since each new item's automatic
playback requires the player to be visible and more than half visible at that moment.

INFERRED — one further structural problem worth naming: the "more than one player automatically
playing" prohibition means you cannot pre-roll the next item while the current one plays. Even a
compliant, always-visible version of this app would have an audible gap at every transition, which
is precisely the seam a "real radio show" must not have.

### Q3 — Does YouTube hosting podcasts change the analysis?

**No. It changes nothing, and it slightly worsens the position.**

VERIFIED, **NEW** — YouTube's own creator-facing podcasts page frames podcasts as video first
("Podcasts aren't just audio anymore"). A podcast is created in YouTube Studio via Create → New
podcast as a container with a title, description, visibility setting and square thumbnail, to which
episodes are added "by uploading new videos or assigning existing ones", with an "RSS upload tool"
as an alternative ingestion path. The page states: "YouTube Studio makes it easy to get your podcast
on YouTube and YouTube Music."
Source: https://www.youtube.com/creators/podcasts/

INFERRED — a YouTube podcast episode is therefore an ordinary YouTube video inside a playlist-like
container. It is "YouTube audiovisual content" for policy purposes, so III.E.1, III.I.7, III.I.8,
III.I.9 and III.I.14 apply to it identically. There is no separate podcast API, no separate podcast
terms, and no audio-only entitlement.

INFERRED, and this is the one genuinely useful observation in this section — the RSS ingestion path
runs *the wrong way for this concept*. Publishers push their existing RSS feed **into** YouTube;
YouTube does not emit an RSS feed of episode audio. NO EVIDENCE FOUND of any YouTube-provided RSS
feed exposing podcast episode audio enclosures. So for any podcast that originates in open RSS, the
correct route is the publisher's own feed — which prior work VERIFIED has no ToS layer at all (that
finding is CARRIED OVER and stands). YouTube is a strictly worse door to the same content.

INFERRED — worsens the position slightly, because a podcast-plus-music radio app sourcing both from
YouTube is squarely within III.I.1's "substantially similar service" concern: YouTube Music itself
already plays podcasts and music in one app.

### Q4 — Blocked by policy, by absence of API, or both?

**Both, and the two are independent.**

- **Blocked by absence of API** (VERIFIED by absence): no YouTube Music playback SDK; no Music
  library endpoint; no liked-songs endpoint; no listening-history endpoint (`watchHistory` removed
  from the documentation as of 9 September 2020); the only official iOS player helper is an archived
  WebView wrapper.
- **Blocked by policy** (VERIFIED): III.I.9 bars background players; III.I.7 and III.I.8 bar
  separating and separately promoting the audio component; III.E.1(a)/(b) bar caching and offline
  playback, which kills the commute/buffering requirement; III.I.14 and III.E.6 bar the
  reverse-engineered route; III.I.1 bars substantially similar services; Required Minimum
  Functionality mandates a visible, unobscured, minimum-200×200 player.

INFERRED — the independence matters for how the developer should treat this. A
blocked-by-absence-of-API finding can expire: platforms ship SDKs. A blocked-by-policy finding of
this density does not expire on any timescale relevant to a solo developer's build decision, because
the clauses reflect YouTube's music-licensing obligations to rightsholders, not a product gap. Both
would have to change together. **Do not keep this variant on the board as "blocked for now."**

## 4. Carried over vs new — explicit ledger

**CARRIED OVER and re-verified as current** (all still present in the 2026-06-24 policy text):
- No YouTube Music third-party playback SDK.
- III.E.1(a) caching/storing ban; III.E.1(b) offline-playback ban.
- III.I.7 audio/video separation ban.
- III.I.9 background-player ban.
- III.I.14 non-API-route ban; III.E.6 scraping ban.
- III.I.8 separate-promotion ban.
- API Services ToS §16.3 no-reproduction/no-distribution.
- App Store Guideline 5.2.2 (third-party service terms must permit the use) — bears on any
  reverse-engineered route.

**NEW in this pass:**
- Currency confirmed: Developer Policies "Last updated 2026-06-24 UTC"; API ToS "Last updated
  2026-04-28 UTC".
- III.I.1 substitute-service / no-replicating-core-UX ban — the most conceptually on-point clause,
  missed by prior work.
- III.I.6 no-modifying-the-player; III.I.15 no-derivative-API; III.I.2 no-automated-actions;
  III.F.3.b no-gating-playback.
- Required Minimum Functionality visibility, minimum-size, autoplay-visibility, single-autoplaying-
  player, and no-overlay rules.
- `watchHistory` / `watchLater` deprecation timeline: announced 11 Aug 2016, effective 12 Sep 2016,
  documentation removal 9 Sep 2020. Listening history is unobtainable.
- `relatedPlaylists` contains only `likes`, `uploads`, and deprecated `favorites`; no Music library.
- `youtube-ios-player-helper` archived 5 Dec 2024.
- YouTube podcasts are videos in a Studio container; RSS ingestion flows *into* YouTube, not out.
- The judgement that these clauses bar **this** concept more directly than the prior concept, because
  background playback and audio-only music are the app's continuous operating mode rather than an
  occasional feature.

## 5. What I could not verify

- The complete sentence text (including sub-clause preambles) of each Developer Policies clause. The
  fetch tool caps quoted span length. Fragments are verbatim; full-sentence reproduction was not
  possible. **A lawyer reading the policy directly should confirm the framing, though nothing in the
  reading is close enough to a line to turn on it.**
- Whether YouTube Music liked songs ever appear in the Data API `likes` playlist. NO EVIDENCE FOUND
  either way; no documentation addresses it. Immaterial to the verdict since history is absent
  regardless.
- YouTube's specific podcast support articles. Six candidate `support.google.com` URLs returned 404
  and WebSearch was unavailable in this session. The `youtube.com/creators/podcasts/` page was
  reachable and is sufficient for the structural finding (podcasts are videos, RSS flows inward), but
  the detailed RSS-ingestion mechanics are unverified. **Gap, stated plainly** — it does not change
  the verdict, because the policy clauses do not distinguish podcast videos from other videos.
- Whether YouTube has ever granted "prior written approval" under III.E.1(a) to a third party.
  INFERRED as irrelevant at this scale: the map's own scope excludes bespoke rightsholder
  negotiation, and III.I.9/III.I.7 are not written as approval-waivable.
- Whether App Review has ever passed a YouTube-Music-backed audio app. Not investigated; moot, since
  the terms question is settled against the concept and 5.2.2 keys off the terms.

## 6. Consequence for the map

INFERRED — YouTube Music is eliminated as a music source. Per `map.md`, "a blocked-on-all-platforms
finding does not end the map", so the surviving candidates are Spotify (ticket 02, and note prior
work's INFERRED-fatal "integrated with streams or content from another service" clause), Apple
Music/MusicKit (new ground — Apple is the only major platform with a first-party framework that plays
audio in the developer's own app), Tidal, licensed/royalty-free catalogues, and the user's own local
library.

INFERRED — one transferable lesson, not a YouTube finding: the clause that killed this fastest was
not the audio one, it was **background playback**. Every remaining platform should be checked for a
background-playback entitlement *first*, before library and history access, because it is cheap to
check and it is the requirement the developer's own commuting use case cannot survive without.

## Sources

Primary:
- https://developers.google.com/youtube/terms/developer-policies (Last updated 2026-06-24 UTC)
- https://developers.google.com/youtube/terms/api-services-terms-of-service (Last updated 2026-04-28 UTC)
- https://developers.google.com/youtube/terms/required-minimum-functionality
- https://developers.google.com/youtube/iframe_api_reference
- https://developers.google.com/youtube/v3/docs/channels
- https://developers.google.com/youtube/v3/revision_history
- https://developers.google.com/youtube
- https://github.com/youtube/youtube-ios-player-helper

Vendor / creator-facing (labelled as such in text):
- https://www.youtube.com/creators/podcasts/

Attempted and unreachable: `developers.google.com/youtube/v3/docs` (500),
`developers.google.com/youtube/v3/getting-started` (500), six `support.google.com/youtube/answer/*`
podcast article IDs (404), WebSearch (tool error throughout the session).





