# 04b — The wider platform sweep: Deezer, self-hosted, and the licensed-catalogue route

Type: research
Status: resolved
Blocked by: —

## Question

A scout pass has already produced the **candidate list** (findings summarised in
`docs/research/04b-scout-candidate-list.md`). This ticket does the **depth work** on the candidates
that could actually carry the concept, and produces feasibility verdicts in the same shape as
tickets 01–04: feasible / partial / blocked, with the deciding clause or API limitation.

Do **not** re-survey the field. The scout already ruled out, against actual licence text:
Epidemic Sound and Artlist (both bar standalone music-listening experiences by name — the
sync-only licensing pattern), Pixabay, Free Music Archive (API shut down), Napster (decommissioned),
and every linear radio aggregator (TuneIn, iHeart, Radio Garden, SomaFM, Audacy et al. — they return
a stream someone else programmes, so there is no track-selection or splice point). Treat those as
closed unless you find the scout was wrong.

Work these four groups, in priority order:

### 1. The B2B licensing route — highest priority

This is the most promising finding of the scout pass and the least understood. **Feed.fm**,
**Tuned Global**, and **7digital / MassiveMusic** all appear to sell exactly what this concept needs:
pre-cleared catalogues plus rights to build your own player. Feed.fm additionally advertises
automated compliance/reporting and **full indemnification**; Tuned Global advertises an **AutomixIQ**
SDK doing "seamless song transitions, akin to a DJ mix" — directly relevant to the crossfade problem
that blocks the platform-native variants.

For each of the three, establish:

- **Do the rights actually cover this use case?** A consumer-facing, subscription-priced,
  personalised radio app that interleaves their music with third-party podcast and news audio and
  AI-generated speech. The interleaving and the third-party-audio mixing are the specific things to
  check — sync-only or background-music-only rights would be fatal, as they were for Epidemic Sound.
- **Pricing.** None publishes it (Feed.fm's `/pricing` 404s). Find any published rate, case study,
  press report, or customer disclosure. **The critical question is the minimum commitment**: if the
  floor is a five-figure annual minimum, the route is dead at a $1–3k MRR bar and the whole
  licensed-catalogue variant collapses. Say so plainly if that is what the evidence suggests. If
  pricing is genuinely unobtainable without a sales call, report NO EVIDENCE FOUND and flag it as a
  **task ticket** the developer must do (an enquiry email), not a gap to paper over.
- **Catalogue quality.** Major-label vs indie vs royalty-free tiers. This decides whether the
  product plays music users recognise — which is the crux of the personalisation question in 05.
- **Platform support**: iOS SDK? Is the audio mixable with other players in one session?
- **Mubert** as the AI-generated-music fallback: it is the only one with **public pricing**
  ($99/$249/$999 per month, with sub-licensing on a paid tier). Cost it against the MRR bar, and
  assess whether AI-generated music is acceptable for a product whose premise is *your* music.

### 2. Statutory / blanket licensing — the alternative to a B2B vendor

Assess whether this concept could license music **directly** under non-interactive webcasting rules
rather than through a vendor:

- US: SoundExchange / DMCA §114. The per-performance rates, the **minimum annual fee**, and above
  all the **interactivity rules** — the sound-recording performance complement, skip limits, and the
  bar on announcing tracks in advance. "Plays your liked songs" sounds **interactive by definition**
  and therefore likely ineligible for statutory rates. Resolve this: it may be the single most
  important finding in the ticket, because it determines whether the concept's personalisation
  premise is compatible with cheap licensing *at all*.
- Note the equivalent position in the UK/EU (PPL/PRS, GEMA) at a high level only.
- Not legal advice; flag where a lawyer is required.

### 3. Deezer — the live subscription-platform surprise

The scout found Deezer's developer portal live and `/user/me/history` returning 200, contrary to the
assumption it was dead. Give it the same treatment as tickets 01–04:

- Is the playback SDK current or deprecated? The scout found a deprecation rumour it could neither
  confirm nor refute, and noted the docs are login-walled. **Resolve it.**
- The scout found the REST API serves **30-second extracts only** — confirm whether full-track
  playback exists for authenticated subscribers via the SDK, because 30-second clips would be fatal.
- Caching is "strictly forbidden" — verify and quote.
- Any clause on combining Deezer content with third-party audio, and whether its audio can coexist
  with another player in one session.
- Subscriber base and addressable audience, as for Tidal.

### 4. Self-hosted and local library — the floor case

- **Subsonic / OpenSubsonic** is the best-value target: one client implementation reaches Navidrome,
  Gonic, Airsonic, Ampache, Funkwhale, Nextcloud and Jellyfin, and the spec exposes `stream` plus
  `playCount`, `starred`, `userRating`. **Jellyfin** separately offers the richest single API
  (PlayCount, IsFavorite, LastPlayedDate). Confirm both, and confirm that streaming the user's own
  files carries no licensing cost.
- **iOS `MPMediaItem`**: the scout found `assetURL` is nil for DRM/cloud items, so Apple Music tracks
  are unreachable and only genuinely local files play. Verify — this bounds the local variant hard.
- **Android `MediaStore`**: no play-count, last-played, or rating columns; only `IS_FAVORITE`. Verify.
- Then the question that decides this variant: **how large is the population that still owns music
  files and self-hosts a music server?** Self-hosting means no cold start — every user must already
  run a server. Size it honestly; a technically perfect variant with a 10,000-person addressable
  market fails the bar before pricing.

### 5. The taste layer, and one inverted architecture

- **ListenBrainz** (open history + recommendations) and **Last.fm** (history, similar-artist;
  commercial use needs approval) can supply *taste data* even where the music itself comes from
  elsewhere. Confirm their terms permit commercial use. This decouples personalisation from the
  music source and may rescue the licensed-catalogue variant's biggest weakness.
- **Icecast / Liquidsoap inverted architecture**: the app becomes the *broadcaster*, splicing music,
  podcast, news and TTS into its own stream. This solves the interleaving problem outright — the
  splice is yours. Assess seriously, but cost the consequence: the entire licensing burden becomes
  the developer's, which routes straight back to §2 above.

## Output

A verdict table covering every candidate examined, matching tickets 01–04's format. Then a clear
statement of **which non-platform music routes are live**, what each costs, and — the decisive
question for ticket 05 — whether any of them lets the app play **music the user already knows and
likes**, or whether every legal route means unfamiliar music.

## Answer

Full findings: [`docs/research/04b-wider-platform-sweep.md`](../docs/research/04b-wider-platform-sweep.md)

### Headline verdict

**The blocker is not price. It is that the AI DJ announcing tracks in advance is legally incompatible
with every affordable route to familiar music.**

17 U.S.C. §114(d)(2)(C)(ii) (VERIFIED) forbids a statutorily-licensed webcaster from publishing "an
advance program schedule or prior announcement" of the titles or featured artists of specific upcoming
recordings. That is concept step 5. It is a flat prohibition with no de minimis exception, and it binds
**every** statutory-licence route — including Feed.fm Radio and any 7digital build, not just direct
licensing. The parallel clause on the vendor side is Feed.fm T&C §5.6/§5.5(a), which **bars insertion
into or alteration of the stream** — the same outcome by a different mechanism.

**One genuine piece of good news, which corrects this ticket's own framing:** personalisation is *not*
interactive by definition. *Arista Records v. Launch Media* (2d Cir. 2009) held LAUNCHcast's
personalised radio non-interactive on a **control vs. influence** distinction (SECONDARY — the opinion
text itself was unretrievable). Pandora operates this way today. However, INFERRED at medium-high
confidence: a station built from *the user's own liked songs* is a materially harder case than
LAUNCHcast on the "program specially created for the recipient" prong, because LAUNCHcast survived
precisely by *not* being a your-library player. **Lawyer required.**

### Live non-platform music routes, and what each costs

| Route | Status | Cost (verified figures only) |
|---|---|---|
| **User's own library** — Subsonic/OpenSubsonic, Jellyfin, iOS local files | **Live, legal, free** | **$0** licensing |
| **Feed.fm / Adaptr** | **Live but two blockers** | Enterprise: **NO EVIDENCE FOUND** (structure verified: setup fee + nonrefundable quarterly fees). Adaptr 2021 archived tiers $99/$999/$2,399 — SECONDARY and stale |
| **Direct US statutory licence** | Live, but loss-making | **$1,000/yr per channel** (37 CFR §380.10) + **$350/yr** BMI lowest tier. **$0.0032/performance** is the real cost — ≈**$17,500/yr at 1,000 subscribers**. ASCAP/SESAC/GMR: NO EVIDENCE FOUND |
| **ListenBrainz** (taste data only, no audio) | **Live, cleanly priced** | **$100/month** (MetaBrainz Bronze) |
| **Mubert** | Dead | $99/$249/$999/mo published — but caps at ~16 daily-hour listeners |
| **Tuned Global** | Dead — **cannot license you music** | n/a |
| **7digital / MassiveMusic** | Dead at this budget | "You will be responsible for paying all royalties" |
| **Deezer** | Dead — ToS §IV "NON-COMMERCIAL USE" | n/a |
| **Last.fm** | Dead — commercial use needs a negotiated agreement | NO EVIDENCE FOUND |

### The decisive question for ticket 05

**Does any legal route play music the user already knows and likes? Yes — three do. None is
simultaneously familiar, affordable, and compatible with the AI DJ.**

1. **The user's own library — the only route that is legal, free AND familiar.** It is their music by
   definition. Jellyfin's `PlayCount`/`IsFavorite`/`LastPlayedDate` (VERIFIED in source) is the richest
   taste signal found anywhere in this sweep, with an official actively-maintained Swift SDK. It is also
   the only route where crossfade and TTS interleaving work natively, **and the only route where the AI
   DJ can announce freely** — private performance sits outside §114 entirely, so step 5 survives fully
   intact. **It fails on addressable market, not on law or engineering.** The entire incumbent iOS field
   (play:Sub, SubStreamer, Amperfy, Finamp, iSub) sits at **82–425 App Store ratings each**; $1k MRR
   needs ~400–500 concurrent subscribers from a cold start, against free and mature competitors, in a
   population unwilling to pay for software. **Cannot plausibly reach $1–3k MRR.**
2. **Feed.fm / Adaptr — the only route worth an enquiry.** Genuine rights provider (not just tech),
   major-label and chart catalogue, full indemnification, live self-serve signup at Adaptr, iOS SDK
   v5.8.0 shipped 2026-06-02. Blocked pending two answers: the §5.6 insertion ban, and price.
3. **Direct statutory licensing (US only)** — full repertoire, affordable minimum, but structurally
   loss-making per-performance, incompatible complement limits for artist-concentrated libraries, and
   **the AI DJ reduced to back-announcing only** ("that was…", or "some Fleetwood Mac at some point
   today" — both expressly permitted).

**The strategic choice this research forces:** advance track announcement and licensed familiar music
are mutually exclusive at this budget. Either drop advance announcement (retreat to DMCA-compliant
radio, still gated by per-performance economics), or drop the music layer for a spoken-audio product
where announcement is legal and cheap — third-party podcasts via open RSS, news audio, and TTS are all
tractable at $1–3k, and **announcing podcast episodes in advance is perfectly legal**, since the
pre-announcement bar is a sound-recording rule. **Do not budget for "find a cheaper music API."**

### Follow-on TASK tickets required

1. **Enquiry email to Feed.fm** — minimum annual commitment, and whether TTS DJ announcements and
   third-party podcast interleaving are permitted notwithstanding T&C §5.6/§5.5(a). This is the single
   highest-value unknown remaining.
2. **Enquiry to Adaptr** (app.adaptr.com registration is live) — current pricing, and whether a
   personalised consumer radio app satisfies Exhibit B eligibility.
3. **Legal review**, if either route survives: whether a liked-songs pool clears §114(j)(7) after
   *Launch Media*; how SoundExchange counts channels for per-user personalised streams (the
   highest-leverage unknown in the cost model); and podcast/TTS insertion rights.

### Hypothesis verification

(a) Tuned Global is technology-only, not a rights provider — **CONFIRMED**; the scout's table is
corrected. (b) Feed.fm SDKs actively maintained and the only vendor bundling rights + SDK +
major-label catalogue + indemnification — **CONFIRMED, both limbs**. (c) 7digital makes you the
licensee/royalty payer, and DMCA constraints bar pre-announcing tracks — **CONFIRMED, both limbs**, and
the second limb generalises to every statutory route. (d) Royalty-free/CC licences cover masters only,
leaving PRO liability — **PARTIAL, overstated**: CC 4.0 expressly includes "public performance" in
"Share" and covers the composition where the licensor controls it; the real risk is *authority*
(PRO-affiliated writers, unclear splits), not scope. (e) Mubert's binding API terms are unpublished and
contradict its public personal-use-only language — **CONFIRMED**.

**Method limitation:** WebSearch was unavailable for the entire session (HTTP 400 on every call).
Findings rest on direct primary-source fetches, Wayback CDX, GitHub/Docker Hub/iTunes APIs, and direct
inspection of the local iOS 27.0 SDK headers. "NO EVIDENCE FOUND" means *not found in this session*.
Not legal advice.

