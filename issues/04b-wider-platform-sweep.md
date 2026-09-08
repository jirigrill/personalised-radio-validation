# 04b — The wider platform sweep: Deezer, self-hosted, and the licensed-catalogue route

Type: research
Status: open
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
