# 08 — Wider competitive landscape: indie apps, living and dead

Type: research
Status: resolved
Blocked by: —

## Question

The complement to the incumbent-feature teardown. In the prior project the **wider** sweep produced
the decisive finding, so treat this as high-value, not a formality.

1. **Direct competitors.** Find apps that programme a personalised mixed music-and-talk stream.
   Search App Store, Play Store, Product Hunt, Hacker News, Indie Hackers, GitHub, r/SideProject.
   Likely search terms: "personal radio station", "AI radio", "AI DJ", "personalised radio",
   "music and podcasts mixed", "radio show generator", "your own radio station". Also look for
   open-source projects doing this.
2. **Adjacent categories** that solve the same underlying job — filling a commute with a curated
   mix without touching the phone. Include: audio aggregators/queue apps that mix sources, TTS
   article-to-audio apps that build a listening queue, AI-generated podcast products, and CarPlay-
   focused audio apps. Note where the real substitute is simply *a playlist plus a podcast app*.
3. **The graveyard.** Every dead attempt found, with cause of death where discoverable — licensing,
   acquisition, no demand, cost. Prior project found 3 of 4 dedicated competitors dead and nobody
   noticed; check whether the same pattern holds here.
4. **For the closest living competitor**, do a focused teardown: what it does, what it costs, its
   ratings volume and trend, what its users praise, complain about, and ask for. If it gives the
   core feature away free, say so plainly.
5. **Differentiator audit.** List every candidate differentiator this concept could claim, and for
   each mark: already occupied / one API call away / structurally safe. Prior project found that of
   nine candidates, only one was structurally safe — apply the same discipline.

Conclude with a **saturation verdict**: is the space (a) empty because nobody thought of it,
(b) empty because everyone who tried failed, or (c) already solved? These have opposite
implications and the difference is the point.

## Answer

Research: [`docs/research/08-indie-competitive-landscape.md`](../docs/research/08-indie-competitive-landscape.md)

### Headline finding

**The exact concept is built by nobody living — and it has been attempted at least a dozen times over
twelve years.** Every living AI-DJ product mixes *synthesised* talk with music; every living AI-talk
product has *no* music. The join is empty because it is a licensing problem wearing a product-problem
costume, not because it was overlooked.

The three closest analogues are all dead, two of them killed by Apple:

- **Scout FM** — "radio-like stations for podcasts" curated from listening history, with CarPlay.
  Acquired by Apple 2020 and **shut down on all platforms**. HN noticed with **1 upvote**.
- **Swell** — personalised talk radio. Acquired by Apple 2014-06-29 for **$30M**, discontinued.
- **AudioOne** — the only product ever found that promised *news + podcasts + music* in one personal
  stream (Show HN 2022: 59 pts). **Domain is now NXDOMAIN.**

**The decisive datum is a primary-source SEC filing, not an indie app.** Auddia Inc. (NASDAQ: AUUD)
built *faidr*, which solves the hardest technical sub-problem this concept needs — detecting
boundaries between commercials, songs and DJ talk in a live stream. Results: **revenue $0 in FY2024
and FY2025**; **$7,584 in H1 2026, from B2B not consumers**; **net loss $7,693,197 in FY2025**;
**687 App Store ratings** after **$907,411 of sales & marketing in H1 2026 alone**; going-concern cash
runway into Q2 2027; and in 2026 it **abandoned direct-to-consumer entirely** ("evolving its business
model from direct-to-consumer to business-to-business") while merging into its own CEO's unrelated
holding company. Its audited risk factors state its "freedom to operate" rests on the **"personal use
exemption" for time-shifting** — the clearest available statement of where the legal wall sits, and
prior art for tickets 01–05.

Volume across the living field is negligible: closest competitor **SUB/WAVE = 4 iOS ratings** (1,307
GitHub stars, ~1,000 Android installs); **AITUNE = 2**; **musen = 0**; **doremiradio = 0**; largest
genuinely-relevant indie **InstaPodz = 156**. A dozen AI-radio GitHub projects exist from 2026; 14 of
16 have **zero stars**.

**And the interleaving premise itself took damage.** In the 294-comment HN thread on Spotify's AI DJ,
**nobody asked for podcasts mixed with music** — the traffic ran the other way. On Spotify's ideas
board, **separating** podcasts from music scores **500 / 463 / 443 votes** while **interleaving** them
scores **12 and 10**, and three separate ideas request the ability to **turn AI DJ features off**. No
user anywhere asked for AI DJ intros. This belongs in ticket 09's evidence base.

**The honest substitute is a playlist plus a podcast app** — free, installed, offline, CarPlay-native
(Spotify n = 41,965,524; Apple Podcasts n = 2,073,804). The friction removed is one tap at a red
light. FM radio and SiriusXM (n = 3,075,988) already mix music and talk, professionally and licensed.

### Differentiator audit — 12 candidates, 0 unambiguously safe

| # | Candidate | Verdict |
|---|---|---|
| 1 | AI-generated DJ intros | **Already occupied** — Spotify AI DJ + 5 apps + ~16 GitHub projects; users ask to disable it |
| 2 | Continuous stream, no queue | **Already occupied** — SUB/WAVE, Pandora, Deezer Flow, every FM tuner |
| 3 | Reads listening history / liked songs | **One API call away** — it *is* an API call; AITUNE shipped it |
| 4 | Personalised news segments | **One API call away** — RSS + TTS; SUB/WAVE ships it as a plugin |
| 5 | Mood/context awareness (time, weather, location) | **One API call away** — AITUNE and SUB/WAVE both ship it |
| 6 | CarPlay / commute focus | **Already occupied and constrained** — SUB/WAVE ships it; Scout FM shipped it and Apple switched it off; CarPlay reportedly forbids custom radio UI |
| 7 | **Interleaving your own podcasts with your own liked music** | **Structurally safe — for a bad reason.** The only unoccupied item, unoccupied because it is *blocked*: needs simultaneous rights to programme licensed music and control third-party podcast playback. The one product that promised it is a dead domain. A moat you cannot legally cross is not a moat you can occupy |
| 8 | Intelligent "suitable moment" pause-and-resume | **Structurally safe, commercially inert** — Auddia built exactly this and earned $0 of consumer revenue over two fiscal years |
| 9 | Zero-decision / press play / configure once | **Already occupied** — SUB/WAVE, InstaPodz post-pivot, and FM radio. Correct instinct, not a moat |
| 10 | Uses the music you already pay for | **One API call away** — AITUNE ships it on Apple Music today ("fully licensed"), n = 2 |
| 11 | Self-hosted / own-library variant | **Already occupied comprehensively** — SUB/WAVE: free, MIT, 1,716 commits in 4 months |
| 12 | Developer is in the target segment | **Not a differentiator** — sample of one; SUB/WAVE's author is too, with 4 ratings |

Worse than the prior project's 1-of-9: there, the single safe differentiator was at least *reachable*.
Here both structurally-uncopied items are uncopied for reasons that **also stop this concept**.
Everything reachable is occupied or trivially copyable.

### Saturation verdict

> **(b) Empty because everyone who tried failed** — with a strong secondary reading of
> **(c) already solved by an adequate free substitute.**
>
> It is emphatically **not (a).** Do not build on the premise that nobody thought of this.

The void is real and confirmed. It is also a graveyard: Scout FM, Swell, AudioOne, Otto Radio, Rivet,
60dB, Spotify Stations, Curio, Huxe, Radiant, WFH.FM, Moodagent, Entale — and Auddia's $7.7M/yr,
$0-revenue, now-abandoned public-market attempt. Causes of death are **acquisition, consolidation,
silent demand failure, and content-rights economics** — never technology. Meanwhile the underlying job
is adequately served for free by FM radio, SiriusXM, and two apps the user already has.

The one genuine positive: three independent teams (SUB/WAVE, post-pivot InstaPodz, this concept)
converged on the same shape — **zero decision, press play, configure once, no skip**. That validates
the *form*. Both of the others give it away free.

**Largest gap in this ticket:** no user review text was obtained for any app (Apple's review RSS
returned empty feeds throughout) and `WebSearch` was broken for the whole session, so several rows are
UNVERIFIED. All Reddit figures come from a mirror, uncross-checked. No competitor MRR figure was found
anywhere.
