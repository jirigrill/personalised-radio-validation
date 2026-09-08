# 09 — Demand evidence and willingness to pay

Type: research
Status: open
Blocked by: —

## Question

Is there evidence that people **want** this and would **pay** for it? Evidence, not plausibility.
Prior project's discipline applies: a "does an app do this?" post with 7 upvotes is a real datum and
a small one — report volume honestly, and count the absence of evidence as evidence.

1. **Demand signals.** Search for people asking for this in their own words: Reddit
   (r/spotify, r/podcasts, r/AppleMusic, r/TIdal, r/commuting, r/CarPlay, r/SideProject,
   r/androidauto), streaming-platform community/idea boards (Spotify Community feature requests are
   well-indexed and carry vote counts — get real numbers), app-store reviews of competitors and of
   the platforms themselves, Hacker News, Twitter/X, YouTube comments. Report **volume and vote
   counts**, not just existence.
2. **Spotify Community idea board** deserves specific attention: search for requests to mix podcasts
   and music, for a radio-show format, for DJ commentary, and record vote counts and any official
   staff response (including "not planned" / "closed"). An official rejection is a strong signal in
   both directions.
3. **The counter-evidence.** Go looking for it deliberately:
   - Do people actually want music and talk interleaved, or do they deliberately separate the two
     by mood and context? Look for evidence on listening behaviour — is podcast time and music time
     substitutable or complementary?
   - Is the commute already solved by a queue plus a playlist? What is the honest incremental value?
   - Does AI DJ commentary annoy people? Spotify DJ reception is directly relevant evidence.
4. **The segment.** Who specifically is the acute user? The developer is one (a commuter who wants
   both). Estimate how large that segment plausibly is, and **do not invent funnel fractions** — if
   a number has no source, decline it and say so, as the prior project did.
5. **Willingness to pay.** What do comparable apps charge, and what do people actually pay for in
   audio? Note the hard problem: the user is **already paying** for Spotify/Apple Music and a podcast
   app. This asks them to pay a third subscription for a layer on top. Find evidence about that
   specific pattern — do add-on-to-a-subscription apps sustain paid users, and what do they charge?
6. **Pricing models in the space**: one-off, monthly, annual, freemium, lifetime. What is standard?

## Handed to this ticket by 07 and 08 — verify and extend, do not merely restate

Tickets 07 and 08 surfaced demand evidence pointing in **opposite directions**. Both had `WebSearch`
broken and Apple's review RSS returning empty feeds, so **no user review text was obtained anywhere** —
that is this ticket's largest single gap to close, and the reason these are leads rather than findings.

Counter-evidence to verify and extend (from 08):

- On **Spotify's ideas board**, ideas asking to **separate** podcasts from music score
  **500 / 463 / 443 votes**, while ideas asking to **interleave** them score **12 and 10**. Three
  ideas ask to *disable* the AI DJ. Re-find these, confirm the vote counts and any staff response,
  and check whether the asymmetry survives a fair search (it is the single most damaging demand
  datum found so far).
- In the **294-comment Hacker News thread on Spotify's AI DJ**, nobody asked for podcasts mixed with
  music — requests ran the other way. Verify.
- **No user anywhere asked for AI DJ intros.** If true after a genuine search, that guts step 5
  independently of Spotify DJ occupying it.
- **Auddia/faidr**: $907k of marketing in six months → 687 lifetime App Store ratings and **$0
  consumer revenue** (SEC-verified). Corroborates the prior project's paid-acquisition arithmetic
  from an independent direction — carry it into 12.

Evidence in the concept's favour, to be weighed honestly and not inflated (from 07):

- Users on **Spotify's own board** did ask for something close to this, saying the AI DJ does not
  substitute for it and wanting "one-click, automatic". Ticket 07 flagged this as a
  **complainer-weighted sample** fully consistent with Spotify having killed Daily Drive for sound
  reasons. Treat it as a lead, not validation.
- The highest-engagement audio-in-car community post found in 08's research was **Non Stop Radio in
  r/AndroidAuto — 174 and 160 upvotes, 151 and 107 comments**. That is the developer's own commute
  context and the largest engagement signal in the space; establish what those users actually wanted.

The asymmetry between "separate them" at 500 votes and "interleave them" at 12 is the crux of this
ticket. If it holds, the concept's central premise is contradicted by its own would-be users, and no
amount of technical feasibility rescues it. Resolve it properly.
