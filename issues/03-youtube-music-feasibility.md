# 03 — YouTube Music: feasibility verdict

Type: research
Status: open
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
