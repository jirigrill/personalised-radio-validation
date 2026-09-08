# 03 — YouTube Music: feasibility verdict

Type: research
Status: resolved
Blocked by: —

## Question

Produce a **feasibility verdict for YouTube Music**: feasible / partial / blocked, with the specific
clause. Quote verbatim with URLs.

Prior work (`../podcasts-notes-validation/docs/research/01-audio-access-legality.md`) found this
**redundantly dead** for a podcast-capture concept: no third-party playback SDK; Developer Policies
III.E.1(a) bars caching/storing copies, III.I.7 bars separating or isolating audio from video,
III.I.9 bars background players, III.I.14 bars non-API routes to content. **Verify these clauses are
current and determine whether they equally bar this concept** — which needs background playback and
audio-only music, both apparently named. State clearly what is carried over vs new.

Then, since this is expected to be the fastest verdict, spend the remaining effort on:

1. Is there **any** sanctioned route — YouTube Music has no developer programme of its own, but does
   the YouTube Data API expose a user's YouTube Music library, liked songs, or listening history?
2. Does the **IFrame Player API** offer anything, and is audio-only or background use permitted?
   (Expected: no, III.I.9 — confirm.)
3. YouTube also hosts **podcasts** now. Does that change any part of the analysis?
4. Is this best characterised as blocked-by-policy, blocked-by-absence-of-API, or both?

Keep it proportionate. If it is dead, say so in a short verdict and do not pad.

## Answer

**Headline verdict: BLOCKED — both by policy and by absence of API. Eliminated. Not "blocked for
now."**

Full research: [`docs/research/03-youtube-music-feasibility.md`](../docs/research/03-youtube-music-feasibility.md)

### Findings

**Clauses are current.** Developer Policies page-dated "Last updated 2026-06-24 UTC"; API Services
ToS "Last updated 2026-04-28 UTC". All four clauses prior work relied on — III.E.1(a), III.I.7,
III.I.9, III.I.14 — are present and unchanged. Nothing has loosened. (CARRIED OVER, re-verified.)

**They bar this concept more directly than the prior one.** Prior work needed *capture*, so the
caching clauses were load-bearing. This concept needs the two things the policies name explicitly:
background playback (III.I.9) and audio-only music (III.I.7, III.I.8). That moves the violation from
an occasional feature to the app's continuous operating mode. (NEW judgement.)

**NEW clause prior work missed: III.I.1** — must not "create, offer, or act as a substitute for, or
substantially similar service to, any YouTube Applications", and "must not mimic or replicate
YouTube's core user experiences by recreating features or process flows". YouTube Music's core UX
*is* algorithmically programmed radio mixing music and podcasts. This is a second, independent
policy kill that does not rely on the audio or background arguments at all.

1. **No sanctioned data route.** (NEW) `channels.contentDetails.relatedPlaylists` exposes only
   `likes`, `uploads`, and deprecated `favorites` — YouTube video likes, not a Music library.
   `watchHistory`/`watchLater` were announced deprecated 11 Aug 2016, effective 12 Sep 2016, and
   removed from the documentation 9 Sep 2020. **Listening history is unobtainable**, which alone
   kills concept step 2 regardless of policy. `ytmusicapi`-style libraries violate III.I.14 and
   III.E.6 — a violation, not a workaround.
2. **IFrame Player API offers nothing usable.** (NEW) Confirmed as expected. Required Minimum
   Functionality mandates a ≥200×200 visible viewport, bars overlays that obscure the player, bars
   autoplay until the player is visible and more than half visible, and permits only one
   auto-playing player — which also breaks gapless podcast→song transitions. Combined with III.I.9
   and III.I.6, every hidden/1×1/off-screen/screen-locked trick is separately non-compliant.
3. **YouTube podcasts change nothing.** (NEW) Episodes are ordinary videos in a Studio container, so
   identical clauses apply. RSS ingestion flows *into* YouTube; YouTube emits no episode-audio feed.
   For RSS-origin podcasts the publisher's own feed is the better door — and prior work's finding
   that open RSS has no ToS layer stands.
4. **Characterisation: both, and independently.** Absence-of-API can expire when a platform ships an
   SDK; a policy wall of this density reflects YouTube's licensing obligations to rightsholders, not
   a product gap. Both would have to change together.

### Hard blockers

| Blocker | Type |
|---|---|
| No YouTube Music playback SDK; only official iOS helper archived 5 Dec 2024 | absence-of-API |
| No listening-history endpoint (removed 2020) | absence-of-API |
| No Music library / liked-songs endpoint | absence-of-API |
| III.I.9 — background players prohibited (kills the commuting case) | policy |
| III.I.7 / III.I.8 — separating and separately promoting the audio component | policy |
| III.E.1(a)/(b) — no caching, no offline playback (kills buffer-ahead) | policy |
| III.I.1 — no substantially similar service / no replicating core UX | policy |
| III.I.14 / III.E.6 — no non-API routes, no scraping | policy |
| Required Minimum Functionality — visible, unobscured, ≥200×200 player | policy |
| API ToS §16.3 — no reproduction/distribution rights granted | policy |

### Transferable lesson for the remaining platform tickets

The clause that killed this fastest was **background playback**, not audio separation. Check every
remaining platform for a background-playback entitlement *first* — it is cheap to check and the
developer's own commuting use case cannot survive without it.

### Gaps, stated plainly

- Full-sentence quotation of policy clauses was not possible (fetch-tool span cap); fragments are
  verbatim. Nothing in the verdict turns on a marginal reading.
- YouTube's detailed podcast support articles were unreachable (404s; WebSearch unavailable this
  session). Immaterial — the policies do not distinguish podcast videos from other videos.

