# 05 — Platform sweep: which music source survives?

Type: grilling
Status: open
Blocked by: 01, 02, 03, 04, 04b

## Question

A decision ticket, worked live with the developer. The synthesis point of the feasibility sweep —
and the gate that unblocks most of the rest of the map.

By this point the per-platform verdicts are in (01 Spotify, 02 Apple Music, 03 YouTube Music,
04 Tidal, 04b the wider platform sweep including licensed catalogues and local library).

Decide: **which music source, if any, the concept is built on.**

Bring to the conversation:

- The **per-platform verdict table**: feasible / partial / blocked, with the deciding clause.
- The **hard blockers vs workarounds** distinction, with any workaround that violates a clause
  marked as not a workaround.
- For each surviving option, what it **costs the concept**:
  - A platform-native survivor keeps "your own liked songs" — the concept's core personalisation
    hook — but may forbid interleaving, caching, or mixing.
  - A **licensed/royalty-free catalogue** survives legally but **loses the personalisation premise
    entirely**: it is no longer *your* music. Confront this rather than glossing it — the concept's
    step 2 reads listening history and liked songs, and a stock catalogue cannot honour that. Is a
    radio station of unfamiliar music still the product the developer wants?
  - A **local-library** variant keeps personalisation and costs nothing, but the addressable audience
    is people who still own music files — likely a small and shrinking segment. Size it honestly.
- The **transition ceiling**: if audio cannot be mixed or crossfaded on the surviving platform, the
  "feels like a real radio show" premise degrades to hard cuts. Is that still the product?

Questions to resolve with the developer:

1. If platform-native music is blocked, is a **stock-catalogue** variant still interesting, or was
   "my own liked songs" the entire point?
2. Would they accept a **hard-cut** transition rather than a crossfade, if mixing is unavailable?
3. If the only feasible platform is a small one (e.g. Tidal), is building for a small addressable
   audience acceptable, or does that fail the bar before the arithmetic is even run?
4. Is a **local-library-only** MVP an acceptable way to test the concept cheaply, even if it is not
   the eventual product?

Output: one named music source to carry forward, a ranked shortlist, or the conclusion that none
survive — in which case the map short-circuits to the verdict.

## What 04b, 07 and 08 have already established — read before this session

The shape of this conversation has changed. Three findings have arrived that reframe it, and the
question is no longer only "which platform survives".

**1. The trilemma.** 04b's decisive finding: three routes can play music the user already knows, but
**none is familiar + affordable + DJ-compatible**. 17 U.S.C. §114(d)(2)(C)(ii) (VERIFIED twice) bars
any statutorily-licensed webcaster from announcing specific titles or featured artists in advance —
flat, no de minimis. That is **concept step 5**, and it binds every statutory route including
Feed.fm Radio. Spotify Policy **III.7** independently bars segueing, mixing or overlapping Spotify
Content with any other audio (07, VERIFIED twice) — the transition mechanic, by name.

**2. The user's own library is the only route that is legal, free, familiar and DJ-compatible** —
private performance sits outside §114 entirely, so the DJ may announce whatever it likes. It fails
on **market size alone**: the incumbent iOS field sits at 82–425 App Store ratings each, and $1k MRR
needs roughly 400–500 concurrent subscribers, cold-start, against free mature competitors.

**3. The escape hatch.** The advance-announcement bar is a **sound-recording** rule. Announcing
*podcast* episodes in advance is unrestricted. A **spoken-audio-first** product keeps the DJ premise
and drops the one structurally unreachable component. 08's structural lesson points the same way:
*talk you own scales; talk you license does not.*

So add these questions to the session, ahead of the original four:

5. **Given the trilemma, which corner does the developer give up?** Familiar music (licensed
   catalogue, unfamiliar tracks), affordability (direct licensing, out of scope), or the DJ naming
   tracks (keep familiar music, mute the announcements)? This is the central decision of the map and
   it cannot be researched away.
6. **Is a spoken-audio-first product still the thing they want to build?** Podcasts, news and an AI
   DJ that talks freely, with music as a minor or absent element. It is the one variant with no
   identified legal blocker — but it is a different product from the one in the brief, and it lands
   near what Spotify killed (Daily Drive) and what Apple bought and switched off (Scout FM).
7. **Is a local-library MVP worth building to test the concept**, accepting that it can never be the
   commercial product? It is legal, free and buildable now.
8. **Do they want to spend money on the two open task tickets** (15 vendor enquiries, 16 legal
   review) before deciding, or decide without them? 15 is free and slow; 16 costs real money and is
   only worth it if a licensed route survives.

Also carry in: **Deezer is blocked** on non-commercial ToS, not its SDK (04b); **Tuned Global is
technology-only and cannot license music** (04b, correcting the earlier scout pass); **Apple ships a
real `crossfade(duration:)` from iOS 18+** but MusicKit has **no podcast type anywhere** (07); and
**Tidal's `@tidal-music/player` supports `AssetPresentation.FULL`** — the only documented third-party
full-track path found (07).
