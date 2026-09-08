# 04b — The wider platform sweep: depth findings

Depth research for `issues/04b-wider-platform-sweep.md`, building on
`docs/research/04b-scout-candidate-list.md`. Every claim is tagged VERIFIED (primary source, URL
given), SECONDARY (press/forum/third-party, labelled), INFERRED, or NO EVIDENCE FOUND.

**Not legal advice.** Section 2 touches US statutory copyright licensing. Where the analysis turns on
statutory interpretation, that is flagged as requiring a lawyer.

## Headline

**No legal route lets this app play music the user already knows and likes, at this budget, with the
AI DJ announcing tracks.** The blocker is not price. It is a structural incompatibility discovered in
Section 2: the statutory licence that makes cheap radio possible **expressly forbids pre-announcing
the titles or artists of specific upcoming recordings** (17 U.S.C. §114(d)(2)(C)(ii)) — which is
step 5 of the concept. Everything cheap is unfamiliar music; everything familiar is either
enterprise-priced or bars the announcement feature.

## Verdict table

| Candidate | Verdict | Deciding clause / limitation |
|---|---|---|
| **Feed.fm** | **PARTIAL — best B2B fit, price unknown** | Genuine rights provider (T&C §5.8) with major-label catalogue + indemnification. But T&C §5.6/§5.5(a) **bar insertion into or alteration of the stream** — kills TTS DJ inserts absent written consent. Enterprise pricing NO EVIDENCE FOUND (Setup Fee + nonrefundable Quarterly Fees, no amounts, §4.1–4.3) |
| **Adaptr** (Feed.fm self-serve arm) | **PARTIAL — only self-serve route found** | §2.1 permits on-demand + subscription. Live registration at app.adaptr.com. Archived 2021 tiers $99/$999/$2,399 (SECONDARY, web.archive.org) — current price NO EVIDENCE FOUND. Same insertion/alteration ban |
| **Tuned Global** | **BLOCKED — not a rights provider** | Client holds the licences; Tuned Global is technology + licensing consultancy/introducer. Cannot license you music. Scout's table was **wrong**. No pricing ever published or archived |
| **7digital / MassiveMusic** | **BLOCKED at this budget** | "You will be responsible for paying all royalties." Per-client label approval judged on "sufficient up-sell opportunities"; DDEX DPID required; App Store gated on compliance sign-off. Routes straight back to Section 2 |
| **Mubert** (AI music) | **BLOCKED on product + arithmetic** | Published tiers cap at ~16 daily-hour listeners; 50–500 users is 3–30× beyond top tier. Vocals only on unpriced Custom tier. Binding API licence unpublished; readable legal text bars distribution "via Music Streaming Services" |
| **Statutory US webcasting (SoundExchange §114)** | **BLOCKED by the AI DJ** | §114(d)(2)(C)(ii) bars advance announcement of titles/artists. Minimum fee **$1,000/yr per channel** (37 CFR §380.10) — affordable — but per-listener personalised streams likely = many "channels", and the announcement feature forfeits eligibility outright |
| **Personalisation itself under §114** | **FEASIBLE — surprising good news** | *Arista Records v. Launch Media* (2d Cir. 2009) held LAUNCHcast's personalised radio was **not** interactive: users had influence, not control. "Plays your liked songs" is *not* automatically fatal |
| **Deezer** | **BLOCKED — non-commercial ToS** | ToS **Section IV is titled "NON-COMMERCIAL USE"**: developer "shall not perceive, receive, generate, benefit or create directly or indirectly, any moneys, incomes, revenues". Use "limited to a strictly private use within a family scope." Also: local storage "strictly forbidden"; native SDKs effectively abandoned (samples last pushed 2019) |
| **Subsonic / OpenSubsonic** | **FEASIBLE technically, fails on market** | `stream` endpoint + `playCount`, `starred`, `userRating`, `played` all VERIFIED. No licensing cost. But addressable market cannot reach $1–3k MRR |
| **Jellyfin** | **FEASIBLE technically, fails on market** | `UserItemDataDto` exposes PlayCount, IsFavorite, LastPlayedDate, Played (VERIFIED in source). Official **actively-maintained Swift SDK**. Same market ceiling |
| **iOS `MPMediaItem`** | **PARTIAL — local files only** | VERIFIED against iOS 27.0 SDK header: `playCount`, `skipCount`, `rating`, `lastPlayedDate` all present, none deprecated. But `assetURL` is nullable and `isCloudItem`/`hasProtectedAsset` exist precisely to flag unplayable items → Apple Music tracks unreachable |
| **Android `MediaStore`** | **PARTIAL — confirmed poor** | No PLAY_COUNT, no LAST_PLAYED, no RATING. `IS_FAVORITE` only, API 30+ |
| **ListenBrainz** | **FEASIBLE as taste layer** | Open API, 1 req/sec, token auth. Commercial use → MetaBrainz **Bronze tier $100/mo** ("popular mobile apps"). Affordable. No audio |
| **Last.fm** | **BLOCKED for commercial use** | ToS 3.2: "If at any time you wish to use the Last.fm Data for commercial purposes, you must apply for a commercial use agreement." No published price; 100 MB storage cap |
| **Icecast / Liquidsoap** | **Technically FEASIBLE, licensing fatal** | `output.icecast`, `cross`/`cross.smart`, `switch`, `fallback`, `add` all VERIFIED. Solves interleaving outright. But you become the webcaster — full Section 2 burden, and per-listener streams are both costly and more likely interactive |

## Hypothesis verification (a)–(e)

| # | Hypothesis | Status | Evidence |
|---|---|---|---|
| **(a)** | Tuned Global supplies technology only, not rights — not a peer of Feed.fm | **CONFIRMED** | Its own pages state the **client holds the licences**; its "music licensing" page positions it as consultancy/introducer. The single marketing use of "sub-licenses" (homepage feature grid) refers to royalty-chain/metadata handling, not a music licence to you (INFERRED). **The scout's table is corrected: Tuned Global cannot license you music.** |
| **(b)** | Feed.fm SDKs actively maintained; only vendor bundling rights + SDK + major-label catalogue + indemnification | **CONFIRMED (both limbs)** | GitHub org `feedfm`: iOS-SDK v5.8.0 released 2026-06-02; iOS-SDK-Demo pushed 2026-08-12 — genuinely current. Rights provider status VERIFIED at T&C §5.8(a)(b); major-label/chart catalogue at feed.fm/major-label-music; Warner Music named as licensor and third-party beneficiary. No competitor found bundling all four. |
| **(c)** | 7digital makes you the licensee/royalty payer, and DMCA constraints bar the AI DJ pre-announcing tracks | **CONFIRMED (both limbs)** | 7digital docs: "You will be responsible for paying all royalties." The announcement bar is VERIFIED statutory text at §114(d)(2)(C)(ii). **This directly damages concept step 5** and generalises far beyond 7digital — it applies to *every* statutory-licence route. |
| **(d)** | Royalty-free and CC licences cover master recordings only, leaving PRO/SoundExchange performance rights unresolved | **PARTIAL — overstated as written** | CC BY 4.0 legalcode expressly includes "public performance" in the definition of "Share", and "Copyright and Similar Rights" expressly includes "performance, broadcast, sound recording". So a CC licence from a rightsholder who controls **both** layers does convey performance rights for both. The real residual risk is narrower but genuine: the grant is "limited to… rights that the Licensor has authority to license", so where the composer is PRO-affiliated or publishing splits are unclear, the composition may remain uncleared. Most established royalty-free libraries control publishing too and do clear both. **Verdict: not the blanket blocker claimed, but a real diligence burden per track.** |
| **(e)** | Mubert's binding API terms are unpublished and contradict its public personal-use-only language | **CONFIRMED** | No API licence is published; binding terms require sales contact. Every readable legal text says personal, non-commercial, non-transferable, no sublicensing, and specifically bars distribution "via Music Streaming Services" — while the API is marketed commercially. Direct contradiction. |

---

## Section 1 — The B2B licensing route

### Feed.fm / Adaptr

**Feed.fm IS a genuine rights provider. VERIFIED.** feed.fm/music-licensing and T&C §5.8(a)(b)
establish that Feed.fm holds the licences and extends them to you — unlike Tuned Global. Warner Music
is named as a licensor and third-party beneficiary in the Adaptr terms, which is strong corroboration
that real label rights sit behind the product. Catalogue tiers and territories are documented at
docs.feed.fm/docs/Welcome; chart/major-label artists at feed.fm/major-label-music. **This is the only
vendor examined that can actually license you music a consumer would recognise.**

**SDKs are alive. VERIFIED** via the GitHub API on org `feedfm`: iOS-SDK v5.8.0 released 2026-06-02,
iOS-SDK-Demo pushed 2026-08-12. Dual-stream audio is documented (feed.fm/radio,
docs.feed.fm/docs/going-live-checklist), so coexistence with a second player is supported.

**The fatal clause for this concept. VERIFIED.** T&C **§5.6 and §5.5(a)** prohibit insertion into or
alteration of the stream — identical text appears in adaptr.com/terms-and-conditions. **INFERRED
(high confidence): TTS DJ announcements inserted between tracks fall within this ban absent written
consent.** No express permission for third-party podcast interleaving or TTS announcements was found
at either vendor. This is the same shape of problem that killed Epidemic Sound, arriving through a
different door: the rights are real, but they are rights to play *their* stream, not to cut it up.

**Two different products, two different licensing postures. VERIFIED.**
- **Feed.fm Radio** is built on the US §114 non-interactive basis (docs.feed.fm/docs/Welcome,
  docs.feed.fm glossary) — which means it inherits the whole of Section 2's constraints, including
  the advance-announcement bar.
- **Adaptr** (§2.1) expressly permits **on-demand and subscription** use, escaping the
  non-interactive straitjacket. Adaptr is also the only **self-serve** route found anywhere in this
  research: app.adaptr.com/account/register returns HTTP 200 and there is a documented free
  developer tier.

**Pricing: NO EVIDENCE FOUND for any current figure.**
- Feed.fm enterprise: the fee *structure* is VERIFIED (T&C §4.1–4.3 — a Setup Fee plus nonrefundable
  Quarterly Fees) but **no amounts are published anywhere**. Quarterly, nonrefundable, plus a setup
  fee is INFERRED to be an enterprise motion, i.e. likely four-to-five figures — but **no number was
  found and none is invented here**.
- Adaptr: **SECONDARY** — archived pricing pages (web.archive.org snapshots 20210804005645 and
  20210212212111 of adaptr.com/pricing.html) show tiers at **$99 / $999 / $2,399** and
  **$69 / $249 / $549**. This implies roughly **$0.10/stream** (INFERRED). **These are 2021 prices
  and the page no longer exists; the current price is NO EVIDENCE FOUND.**
- Eligibility thresholds for Adaptr are VERIFIED (terms Exhibit B, adaptr.com/faqs,
  adaptr.com/how-it-works quiz) — worth reading before assuming a consumer radio app qualifies.

**→ TASK ticket required.** Two enquiry emails the developer must send: (1) Feed.fm, asking the
minimum annual commitment and whether TTS DJ announcements and third-party podcast interleaving are
permitted; (2) Adaptr, asking current pricing and whether a personalised consumer radio app is
eligible. Note that even a favourable price answer does not clear §5.6.

### Tuned Global — the scout's table was wrong

**Hypothesis (a) CONFIRMED. Tuned Global cannot license you music.** Its own pages
(tunedglobal.com/streaming-services/music-licensing, /content-delivery, /white-label-music) establish
it as a technology and consultancy provider where **the client holds the licences**. The single
marketing occurrence of "sub-licenses" on the homepage feature grid refers to royalty-chain and
metadata handling (INFERRED), not a music licence to you.

**AutomixIQ is not the crossfade rescue it appeared to be. VERIFIED**
(tunedglobal.com/streaming-services/pacemaker-autmixiq-music-ai-dj): it operates on *your already-licensed
tracks* and does **no TTS**. So it presupposes exactly the label deals that are out of scope per the
map.

**Pricing: NO EVIDENCE FOUND.** No price, minimum, or setup fee published — and **no pricing URL has
ever been archived for the domain**, which is itself evidence of a pure enterprise sales motion.
INFERRED: targets accounts far above a $1–3k/month app. **Recommendation: drop from consideration.**
It is not a peer of Feed.fm and adds nothing the map can use.

### 7digital / MassiveMusic

**Hypothesis (c) CONFIRMED. VERIFIED** from the MassiveMusic docs: **"You will be responsible for
paying all royalties."** You are the licensee and the royalty payer; 7digital is delivery
infrastructure. The consequences stack up badly for a solo developer:

- Per-client **label approval**, judged on whether there are "sufficient up-sell opportunities"
  (VERIFIED) — a solo consumer app at $1–3k MRR is a poor candidate on its face.
- A **DDEX DPID** is required.
- **App Store release is gated on compliance sign-off.**
- "Accessing the Songtradr Pre-Cleared Catalogue" exists in the docs but contains **no rights scope,
  no territories, no media types, no catalogue size** (VERIFIED) — and warns that steps "may not be
  accessible under your commercial agreement."

**Pricing: NO EVIDENCE FOUND.** **Songtradr is not an independent alternative** — it owns both
MassiveMusic/7digital and Bandcamp, so it is the same enterprise funnel with the same unpublished
terms. **Verdict: BLOCKED at this budget.** Because you become the licensee, this route routes
directly into Section 2 and inherits the advance-announcement bar too.

### Mubert — the AI fallback

**BLOCKED on three independent grounds.**

1. **Product (VERIFIED, fatal).** Output is generated on demand — "200+ million tracks generated",
   each "an exclusive music composition". A track that did not exist before your API call **cannot be
   a track your user knows.** "Vocals" appear only in the **Custom** plan feature list
   (mubert.com/api), so by exclusion the published Trial/Startup/Startup+ tiers are
   **instrumental-only**. Positioning is functional background music — "150+ moods and themes",
   filterable by "genre, mood, activity, or BPM", with integrations at Canva, Picsart, Restream.
2. **Economics (VERIFIED arithmetic from published limits).** The published tiers cap out at roughly
   **16 listeners at one hour/day**. A 50–500 user range is **3–30× beyond the top published tier**,
   landing in the unpriced Custom tier.
3. **Legal (hypothesis (e) CONFIRMED).** The binding API licence is **unpublished**. Every readable
   legal text says personal, non-commercial, non-transferable, no sublicensing, public distribution
   prohibited — and specifically bars distribution **"via Music Streaming Services"**, naming "Music
   Stocks (Epidemic Sound, Artlist, Jamendo, etc.)" as a restricted category. The scout's
   "sub-licensing on a paid tier" claim is **not supported by any readable licence text**.

**And it makes the headline feature worse than useless.** The differentiator is an AI DJ announcing
tracks in advance. Announcing an instrumental AI track that never existed before, by an artist who
does not exist, is anti-value — it draws attention to the fact that there is nothing to anticipate.

### Uppbeat / Soundstripe / Songtradr / Melodie — NO EVIDENCE FOUND

All four were blocked at the network layer (Uppbeat HTTP 429 ×3 and DNS failure on help.uppbeat.io;
Soundstripe 404 on both attempted licence paths; Songtradr JS-rendered with no body text; Melodie 307
→ corporate proxy → 403). **No clause, permissive or prohibitive, was read for any of them.**

INFERRED (not verified): the prior is strongly negative, because this licence class sells
*synchronisation* rights and standalone listening competes with rightsholders' streaming income. The
one comparable datapoint that *was* verified supports it — Mubert explicitly bars distribution via
Music Streaming Services. **This is pattern-matching, not evidence about these four licences.** Not
worth further effort: even if permitted, they fail the familiarity test that decides ticket 05.

---

## Section 2 — Statutory / blanket licensing (the most important section)

**Not legal advice.** Two of the findings below turn on statutory interpretation and require counsel.

### 2.1 The good news: personalisation is NOT automatically interactive

The ticket's premise — that "plays your liked songs" is *interactive by definition* — is **too
pessimistic, and the leading precedent says so.**

**VERIFIED**, §114(j)(7) (law.cornell.edu/uscode/text/17/114): an interactive service is "one that
enables a member of the public to receive a transmission of a program specially created for the
recipient," or, on request, "a transmission of a particular sound recording… selected by or on behalf
of the recipient."

**SECONDARY (Wikipedia summary of the opinion; the opinion text itself was not retrievable — Justia
403, FindLaw 404):** in *Arista Records LLC v. Launch Media, Inc.*, 578 F.3d 148 (2d Cir. 2009), the
Second Circuit held on 21 Aug 2009 that **LAUNCHcast's personalised radio was NOT an interactive
service** and therefore qualified for the cheap statutory SoundExchange licence. The reasoning turned
on **control versus influence**: in a truly interactive service "users can play songs on demand",
whereas LAUNCHcast listeners could "only influence whether or not a particular song appears in their
station" by rating songs. Lacking "sufficient control over the playlists", personalisation did not
convert it into an on-demand service.

**So personalisation per se is survivable.** Pandora operates this way in production. That is a
genuine correction to the ticket's framing.

**But INFERRED (medium-high confidence, and this is where a lawyer is mandatory):** a station built
from *the user's own liked songs* is a much harder case than LAUNCHcast, on the "program specially
created for the recipient" prong. LAUNCHcast survived precisely because it was **not** a
your-library player — a large majority of its ~10,000-song pool entered without regard to the user's
stated preferences, with a substantial unrated quota, making the next track genuinely unpredictable.
A pool that simply **is** the user's library is maximally predictable and therefore substitutes for
purchase, which is the harm the statute targets. Diluting the user's own tracks into a minority would
restore compliance — but destroys the premise. **This is question 1 for counsel.**

### 2.2 The bad news: the AI DJ forfeits statutory eligibility outright

**VERIFIED**, §114(d)(2)(C)(ii): the transmitting entity must not cause, induce or facilitate
publication — via **an advance program schedule or prior announcement** — of the titles of specific
recordings to be transmitted, the phonorecords embodying them, or featured artists' names.

**This is a flat prohibition with no de minimis and no medium-based exception. Concept step 5, as
specified, breaks statutory eligibility.** Hypothesis (c)'s second limb is CONFIRMED, and it
generalises: it binds *every* statutory-licence route, including Feed.fm Radio and any
7digital-based build — not just 7digital.

**Cheaply fixable, at the cost of the hook.** The statute **expressly permits** announcing that an
artist "will be featured within an unspecified future time period." So:
- **Back-announcing is fine** — "that was Fleetwood Mac".
- **Vague forward reference is fine** — "some Fleetwood Mac at some point today".
- **"Coming up next: Dreams by Fleetwood Mac" is prohibited.**

The engineering cost is trivial. The product cost is the most distinctive feature in the concept.

### 2.3 The other statutory constraints, all VERIFIED

From §114(j)(13), the **sound recording performance complement** — per 3-hour period, per channel:
- no more than **3 selections from any one phonorecord**, and no more than **2 consecutively**; or
- no more than **4 selections by the same featured artist** (or from one compilation), and no more
  than **3 consecutively**.

**This is brutal for artist-concentrated personal libraries** — exactly what a "your liked songs"
pool is. A user who loves four artists cannot be served a compliant station from their own library.

From §114(d)(2)(C)(iii): no archived programme under 5 hours; archived programmes of 5+ hours not
available beyond 2 weeks; **continuous programmes must be ≥3 hours**; and limits on repeating
identifiable programmes with recordings in a predetermined order. Plus: no replay on demand, no
automatic channel switching, and encoded title/artist information must be passed through.

### 2.4 Costs — minimums are affordable, per-performance is not

| Line item | Amount | Status |
|---|---|---|
| SoundExchange minimum, 1 channel | **$1,000/yr** | **VERIFIED** 37 CFR §380.10 (law.cornell.edu/cfr/text/37/380.10) |
| §112 ephemeral recordings | $0 additional — bundled (5% of payments allocated) | **VERIFIED** same section |
| Aggregate minimum-fee cap | **$100,000/yr** (≈100 channels) | **VERIFIED** same section |
| Per-performance, subscription | **$0.0032** | **VERIFIED** same section (2025 rates, CPI-adjusted annually) |
| Per-performance, non-subscription | **$0.0025** | **VERIFIED** same section |
| BMI Digital Multi-Use, lowest published | $350/yr | **VERIFIED** bmi.com/licensing/entry/digital_licensing — but excludes "commercial music services", so a paid consumer music subscription may be ineligible for this tier (INFERRED) |
| ASCAP / SESAC / GMR | **unknown** | **NO EVIDENCE FOUND** (ascap.com returned 403) |
| MLC §115 mechanical | Likely **not engaged** for pure non-interactive streaming (no server reproductions/DPDs). $2,500/yr new-licensee minimum if it is | **VERIFIED** §390.3; **INFERRED** on non-engagement. themlc.com describes coverage only as "eligible streaming and download services" without distinguishing interactivity |

**Verified partial floor, one channel, US only: $1,000 + $350 = $1,350/yr ≈ $113/month.** Against a
$12k–$36k/yr revenue target that is 4–11% — apparently survivable, and **misleading**, for four
reasons:

1. **Three of four PROs are missing from that arithmetic.** The true floor is unknown and strictly
   higher.
2. **Per-performance royalties dominate at scale, and the minimum is a floor not a cap** (it is
   creditable against per-performance, VERIFIED). At the verified $0.0032 subscription rate, 1,000
   subscribers × 15 tracks/day ≈ 5.48m performances/yr ≈ **$17,500/yr** — which alone exceeds the low
   end of the revenue target. **Webcasting royalties scale with listening hours while subscription
   revenue scales with headcount, so heavy listeners are structurally loss-making with no lever to
   fix it.** This is the arithmetic that actually kills the direct route.
3. **The minimum is per channel or station (VERIFIED).** **NO EVIDENCE FOUND** on how SoundExchange
   counts per-user personalised streams. If each listener's stream is a channel, $1,000 multiplies to
   the $100k cap. Pandora evidently does not pay $1,000 per listener, so a favourable reading exists
   — but its basis was not verified. **This is the single highest-leverage unknown in the model and
   question 2 for counsel.**
4. **Podcast and news audio are entirely outside all of the above. NO EVIDENCE FOUND** — not
   researched. Note the irony that **announcing podcast episodes in advance is perfectly fine**,
   since the pre-announcement bar is a sound-recording statutory-licence rule.

### 2.5 UK / EU — high level only

**VERIFIED** (ppluk.com/music-licensing): PPL offers a **Linear Webcast Licence** for internet radio
and simulcasts. Separately, its "other online licences" signposting page **names personalised/customised
online radio explicitly**, and states that for some such uses you "may need a PPL licence **or may
need to contact individual rightsholders**."

**So the UK answer mirrors the US: "linear" is the licensable shape, and personalised radio is
expressly flagged as possibly requiring direct rightsholder deals.** INFERRED: label-by-label
negotiation for this design. The detail page 404'd — **NO EVIDENCE FOUND** on precise terms, and **NO
EVIDENCE FOUND** on PPL minimum fees.

**PRS for Music: NO EVIDENCE FOUND** — pages returned title-only content. Whether a Limited Online
Music Licence currently exists, and any minimum fee, is unconfirmed. **Do not cite a figure.**
Structurally, UK online use engages **both PRS (performing) and MCPS (mechanical)**.

**Germany — GEMA / GVL: NO EVIDENCE FOUND.** Structurally the same two-licence requirement.

**Critically: the EU has no US-style statutory non-interactive webcasting licence.** INFERRED from
the PPL structure above — the UK/EU position is *tighter*, not looser: you negotiate licences rather
than qualifying for one. So the §114 escape hatch, such as it is, is a US-only benefit.

### 2.6 Hypothesis (d) — the two-copyright question

**PARTIAL, and overstated as put to me.** The claim that royalty-free and CC licences cover master
recordings only is **not** correct as a blanket statement.

**VERIFIED** (creativecommons.org/licenses/by/4.0/legalcode): CC 4.0 defines "Share" to include
"public performance", and defines "Copyright and Similar Rights" to expressly include "performance,
broadcast, sound recording". So a CC licence **does** grant public performance, and where the
licensor holds rights in both the recording and the composition, it reaches **both layers**.

**The genuine residual risk is narrower but real.** The grant is "limited to all Copyright and Similar
Rights… that the Licensor has authority to license". So the exposure is **authority, not scope**: a
PRO-affiliated writer may already have assigned the performance right away, making the grant narrower
than it appears; and publishing splits are often unclear with weak chain of title. Most established
royalty-free libraries control publishing too and do clear both copyrights. **And SoundExchange is not
a gap at all where you hold direct master licences** — it is the statutory collector, bypassed by
direct licensing.

**But this route is moot for this product anyway:** you cannot build a user's-liked-songs radio from a
royalty-free or CC catalogue. The user's songs are not in it.


