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
