# 16 — Legal review: does a liked-songs pool clear §114, and how are channels counted?

Type: task
Status: open
Blocked by: 15

## Question

**HITL — requires a qualified music-licensing lawyer. Not resolvable by research, and explicitly not
legal advice from this project.**

Blocked on 15: if both vendor enquiries come back closed and no statutory route is affordable, this
review is moot and should not be paid for. Open it only if a licensed route survives.

Ticket 04b resolved the law as far as public sources allow and hit three questions that need a
professional opinion. Each is decision-relevant, not academic — the cost model and the concept's
central feature both hang on them.

### 1. Does a "plays your liked songs" pool qualify as non-interactive under §114(j)(7)?

04b **corrected the map's own premise** here: personalisation is *not* interactive by definition.
*Arista Records LLC v. Launch Media, Inc.* (2d Cir. 2009) held LAUNCHcast non-interactive on a
control-versus-influence test — user input *shaped* the stream without *selecting* tracks.

But a liked-songs pool is a materially harder case, because **LAUNCHcast survived precisely by not
being a library player**. The question for counsel:

- Does drawing from a pool the user has explicitly assembled (their saved/liked tracks) cross from
  *influence* into *control*, and thereby forfeit the statutory licence?
- Does it matter how large the pool is, how much randomisation is applied, or whether the app blends
  in tracks the user has not liked?
- If it does forfeit eligibility, the only route to familiar music is direct label licensing — which
  the map has already ruled **out of scope** for a solo developer. Confirm that consequence.

### 2. How does SoundExchange count "channels" for per-user personalised streams?

**The highest-leverage unknown in the entire cost model.** 37 CFR §380.10 sets a **$1,000/yr minimum
per channel** and **$0.0032 per performance** (both VERIFIED). At 1,000 subscribers the
per-performance component alone is ≈**$17,500/yr** against a $12–36k/yr revenue target.

- If each listener's personalised stream counts as a separate **channel**, the minimum fee scales
  per-user and the model dies immediately at any scale.
- If the service is one channel, the minimums are affordable and only the per-performance rate binds.

These two readings differ by orders of magnitude. Get a defensible answer.

### 3. Insertion rights for podcast and TTS audio

- May a statutorily-licensed webcast **interleave** third-party podcast audio and AI-generated speech
  between sound recordings, or does that breach the terms of the statutory licence independently of
  the announcement bar?
- Confirm the reading that **§114(d)(2)(C)(ii)'s advance-announcement bar is a sound-recording rule
  only**, so announcing *podcast* episodes in advance is unrestricted. 04b flagged this as the
  concept's escape hatch and it deserves confirmation before anything is built on it.
- The **UK/EU position** (PPL/PRS, GEMA) at a high level, if the product would ship outside the US.

## Output

A written opinion, or a clear record of what counsel would not opine on without more work and what
that work would cost. Feed the channel-counting answer directly into ticket 11's cost model and the
§114(j)(7) answer into ticket 05.
