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

---

## Section 3 — Deezer

**BLOCKED, and not for the reason the scout expected.** The SDK question turns out to be secondary:
the developer terms forbid commercial use outright.

### 3.1 The deciding clause

**VERIFIED** (developers.deezer.com/termsofuse) — **Section IV is titled "NON-COMMERCIAL USE"**:

> "The Developer agrees that the use of the Services is strictly limited for a non-commercial purpose
> and in a non-commercial environment. It means that the Developer shall not perceive, receive,
> generate, benefit or create directly or indirectly, any moneys, incomes, revenues, data or any other
> consideration in connection with the use of neither the Services themselves, nor any and all Content
> accessed through the Services."

And, in the same section:

> "The use of the Content is limited to a strictly private use within a family scope."

Section I confines the grant to "personal web pages, blog… and personal applications". **A
subscription-priced consumer app is excluded on every count** — it is commercial, it is not private,
and it is not within a family scope. There is no self-serve commercial tier: any commercial use needs
a partner agreement, i.e. enterprise sales, which the map puts out of scope.

Also **VERIFIED**, same page, Section IV: full-length playback requires a **Premium+** account, and
content is "otherwise accessible only up to 30 seconds".

### 3.2 The 30-second question, resolved

**VERIFIED** (developers.deezer.com/guidelines, "Content access rules" table): the REST **API** returns
a 30-second extract for *every* user level including Premium. Full-track playback is available only via
**Plugins** (web) and the **native SDKs**, and only for Premium users. The guidelines footnote is
explicit: "*Full data access, but no audio streaming available except 30s extracts."

Direct probe confirms this — `curl https://api.deezer.com/track/3135556` returns a `preview` field
holding a signed 30s MP3 URL, and no full-length stream field. **Full-track playback does exist beyond
30s extracts, but only through the SDK/plugin path, never the REST API.**

### 3.3 Two further blocking clauses, VERIFIED

- **Caching:** "Local Storage/Offline Storage of audio data is **strictly forbidden**." This kills the
  commuting/offline case exactly as Spotify's ToS IV.3.b did.
- **Streaming URL privacy:** "Except for 30s. extracts mp3s, full-length tracks URLs must never be
  accessible to the end-user from your application" — you are responsible for preventing it. That
  forbids handing the audio to your own mixing pipeline in any form the user could reach.
- **Mandatory feature set:** the guidelines require any "full Deezer-enabled application" to implement
  My Music, My Playlists, My Albums, Mixes and Search — a UI mandate, not a radio product.

**No clause was found specifically addressing mixing Deezer content with third-party audio
(NO EVIDENCE FOUND)** — but this is moot given Section IV.

### 3.4 SDK status — effectively abandoned

**VERIFIED** via the GitHub API on org `deezer`: the only SDK sample repos are `iOS-sdk-samples`
(last pushed **2019-05-20**, 7 stars) and `android-sample` (**2019-07-04**, 42 stars);
`javascript-samples` last pushed 2021-11-14. No SDK binary is distributed via GitHub — the Android
README instructs you to "download the latest version of Deezer SDK for Android and copy the aar",
i.e. manual download from a docs page.

**The doc pages are login-walled**: developers.deezer.com/sdk/ios, /sdk/android and /sdk/javascript all
return HTTP 200 but render only "You have to login to accept the terms and conditions of the simple
API." So the version and release date of the current SDK are **NO EVIDENCE FOUND**.

**The developer docs are stale. VERIFIED:** a 2024 web.archive.org snapshot of
developers.deezer.com/guidelines is byte-for-byte equivalent in substance to the live page today. No
formal deprecation notice was found (**NO EVIDENCE FOUND** — WebSearch was unavailable and DuckDuckGo
served CAPTCHAs), so the scout's deprecation rumour is **neither confirmed nor refuted on the
documentation record**. **INFERRED (high confidence): the platform is unmaintained** — 2019-era
samples, no public SDK artifacts, login-walled docs, unchanged guidelines. Whether or not it is
formally deprecated, it is not a platform to build a business on.

**INFERRED:** the scout's "`/user/me/history` returns 200" was misleading. It does return HTTP 200,
but the body is `{"error":{"type":"OAuthException","message":"An active access token must be used…"}}`
— Deezer returns 200 with an error envelope. That is not evidence the endpoint is usable.

### 3.5 Audience and podcasts

**Deezer's own investor site (deezer-investors.com) — VERIFIED:** **9.1m total subscribers** as of
31 Dec 2025, **€534m** IFRS revenue for FY2025, ~530 staff. Self-described as "#2 independent music
platform globally" on MIDiA data, with conglomerate-owned services excluded. Named partnerships:
Orange, TIM Brazil, RTL, Sonos. (Wikipedia's infobox is badly out of date — €451m/2022 and a 2012
subscriber figure — so use the IR site.)

9.1m subscribers is roughly an order of magnitude below Spotify. **INFERRED:** heavy France/partner-telco
concentration, given the Orange and TIM Brazil relationships and Deezer's French origin — precise
geographic split **NO EVIDENCE FOUND**.

**Podcasts: a podcast API exists but is thin.** `GET /podcast/1` returns a show with a `fans` count,
and `/chart/0/podcasts` returns 200 — but the show probed returned `"available": false`, and
`/episode/{id}` returned `{"error":{"type":"DataException","message":"no data"}}`. **No user-followed-podcasts
endpoint was found: NO EVIDENCE FOUND.**

---

## Section 4 — Self-hosted and local library

**Technically the cleanest route in the whole sweep. It fails on market size, not on law or APIs.**

### 4.1 OpenSubsonic — confirmed, and better than the scout reported

**VERIFIED** (opensubsonic.netlify.app/docs):
- **`stream`** endpoint (since 1.0.0) streams a given media file; params `id`, `maxBitRate`, `format`
  (`raw` skips transcoding from 1.9.0), `timeOffset`, `size`, `converted`. Returns binary audio.
- The **`Child`** response object carries **`playCount`** (long), **`starred`** (ISO 8601),
  **`userRating`** (int 1–5) — all legacy Subsonic fields — plus **`played`** (ISO 8601) and **`bpm`**
  as OpenSubsonic additions. `bpm` is a genuine bonus for a radio-programming product.
- Also present: `getStarred` / `getStarred2`, `getRandomSongs`, `getTopSongs` (OpenSubsonic addition),
  `scrobble`, `setRating`, `star`/`unstar`, `getPlayQueue`/`savePlayQueue`, `getNowPlaying`,
  `getBookmarks`, `hls`.
- **Important design note, VERIFIED:** servers "must not count access to this endpoint as a play and
  increase playcount" — clients must call `scrobble` explicitly. So play-count fidelity is the
  client's responsibility, and other clients' scrobbles are what populate the taste signal.
- Auth: legacy query-param username/password (`u=`/`p=`), with token/salt and an **API Key
  Authentication** extension plus a `tokenInfo` endpoint available.
- **Capability detection is standardised:** a server supporting an added field "must return it with an
  empty / default value when not present", so a client can detect support cleanly.

**One-client-reaches-many is TRUE but not uniform — the scout slightly overstated it.**
- **Funkwhale: PARTIAL. VERIFIED** (docs.funkwhale.audio/user/subsonic/): "Funkwhale supports a subset
  of the Subsonic API", and "Pod admins can choose to disable access to the Subsonic API." So coverage
  is partial and can be switched off server-side.
- **Jellyfin: NOT native. VERIFIED by absence** — Subsonic compatibility exists only via third-party
  bridges (e.g. `andrewrabert/jellysub`, 61 stars, last pushed **2022**, i.e. unmaintained). **Jellyfin
  should be integrated via its own API, not via Subsonic.**
- Navidrome, Gonic, Airsonic(-advanced), Ampache: Subsonic-compatible as reported.

### 4.2 Jellyfin — confirmed, with a genuinely good iOS story

**VERIFIED from source** (`MediaBrowser.Model/Dto/UserItemDataDto.cs` on jellyfin/jellyfin master):
`PlayCount` (int), `IsFavorite` (bool), `LastPlayedDate` (DateTime?), `Played` (bool),
`PlaybackPositionTicks` (long), `PlayedPercentage`, `Rating` (double?), `Likes` (bool?). This is the
richest taste signal of any source in the entire sweep — richer than most commercial APIs.

**VERIFIED** via the GitHub API on org `jellyfin`: **`jellyfin-sdk-swift` is official and actively
maintained** (pushed 2026-09-08), alongside `jellyfin-sdk-kotlin`, `-csharp`, `-typescript` and the
`Swiftfin` iOS/tvOS client (pushed 2026-09-06). Jellyfin core has 56,657 stars. **This corrects a
common assumption: there is a first-party Swift SDK, not merely Swiftfin's internal client.**

### 4.3 Licensing of the user's own files

**INFERRED (high confidence, but a lawyer should confirm before shipping).** In the self-hosted model
the developer hosts nothing: the user's server streams the user's own files to the user's own client.
That is a **private, not public, performance** — no §106(4) public-performance right is implicated and
no licence is needed. The *Cablevision* line of authority (Cartoon Network v. CSC Holdings) supports
one-copy-per-user private transmission; *Aereo* and *MP3tunes* are distinguishable precisely because a
**service provider** did the hosting or transmitting.

**The load-bearing point: this only holds while the developer touches no audio.** The moment the app
proxies, caches, transcodes server-side, or hosts any music, the analysis changes and *Aereo* becomes
the live risk. **Design constraint: all audio must flow user-server → user-device, never through
developer infrastructure.** Note this sits in direct tension with the Liquidsoap architecture in §5.3.

### 4.4 iOS `MPMediaItem` — verified against the shipping SDK

**VERIFIED by direct inspection of the iOS 27.0 SDK header** (`MediaPlayer.framework/Headers/MPMediaItem.h`,
Xcode-beta):

| Property | Availability | Status |
|---|---|---|
| `playCount` | `MP_API(ios(7.0))` | present, **not deprecated** |
| `skipCount` | `MP_API(ios(7.0))` | present, **not deprecated** |
| `rating` | `MP_API(ios(7.0))` | present, **not deprecated** |
| `lastPlayedDate` | `MP_API(ios(7.0))` | present, **not deprecated** |
| `assetURL` | `MP_API(ios(8.0))`, `nullable` | present, **nullable** |
| `isCloudItem` | `MP_API(ios(8.0))` | present, **filterable** |
| `hasProtectedAsset` | `MP_API(ios(9.2))` | present, **filterable** |

**The scout's finding is CONFIRMED and now has a mechanism.** `assetURL` is declared `nullable`, and
Apple ships `isCloudItem` and `hasProtectedAsset` as *filterable* properties precisely so a developer
can exclude the items whose asset cannot be read. **Apple Music / iCloud Music Library tracks are
therefore unreachable for custom mixing; only genuinely local, unprotected files yield a usable
`assetURL`.** No deprecation of the MediaPlayer query surface was found in the iOS 27.0 header — so
MediaPlayer is *not* deprecated in favour of MusicKit for library queries.

**INFERRED, and important for the crossfade problem:** because a local file yields a real `assetURL`,
it can be loaded into `AVAudioPlayer`/`AVPlayer`/`AVAudioEngine` under the developer's own control —
which means **genuine crossfading and TTS interleaving are possible in the local-files variant**, and
in *only* that variant among the platform routes. That is the one place the concept's step-3/step-5
mechanics work natively.

### 4.5 Android `MediaStore` — scout confirmed

**VERIFIED** (developer.android.com/reference/android/provider/MediaStore.Audio.AudioColumns): there is
**no PLAY_COUNT, no LAST_PLAYED, and no RATING** column. `IS_FAVORITE` exists but is declared on
`MediaStore.MediaColumns` (inherited) and was added in **API 30 (Android 11)**; writing it for files
you do not own requires user consent via `MediaStore.createFavoriteRequest()`. There is a `BOOKMARK`
column and `DATE_ADDED`/`DATE_MODIFIED`, but nothing that models listening behaviour. **Android cannot
supply a taste signal from the local library at all** — an app there would have to build its own play
history from zero, i.e. a cold start within a cold start.

### 4.6 Market sizing — the honest answer is no

This is where the variant dies. Signals gathered (all VERIFIED via the GitHub and Docker Hub APIs on
2026-09-08 unless noted):

| Signal | Figure |
|---|---|
| Navidrome GitHub stars | 23,397 (actively developed, pushed 2026-09-07) |
| Jellyfin GitHub stars | 56,657 |
| `deluan/navidrome` Docker pulls | 53,470,889 |
| `jellyfin/jellyfin` Docker pulls | 415,569,110 |
| Plex users | ~16m monthly active streaming users, 2023 (**SECONDARY**, Wikipedia) |
| Plexamp iOS ratings | 10,054 |
| play:Sub iOS ($4.99) | 425 ratings |
| Finamp iOS (free) | 207 ratings |
| SubStreamer iOS (free) | 139 ratings |
| Amperfy iOS (free) | 138 ratings |
| iSub iOS (free) | 82 ratings |

Reddit subscriber counts: **NO EVIDENCE FOUND** (r/selfhosted, r/jellyfin, r/navidrome, r/plexamp all
blocked; subredditstats served no numbers and warns its own data is unreliable). IFPI download-revenue
share: **NO EVIDENCE FOUND** (only the 2024 streaming share, 69.0% of global recorded music revenues,
was readable at ifpi.org/our-industry/industry-data).

**Docker pull counts must not be read as user counts.** They count automated CI and update pulls;
Watchtower-style auto-updaters inflate them by orders of magnitude. They establish that the ecosystem
is real, nothing more.

**The App Store ratings are the load-bearing number, and they are brutal.** The *entire* field of
third-party Subsonic/Jellyfin iOS clients — the direct incumbents, several free and well-liked, some a
decade old — sits at **82 to 425 ratings each**. Plexamp, backed by a company with ~16m users and a
marketing budget, has 10,054. A rating is typically a low single-digit percentage of installs
(INFERRED), so even generously the paid-Subsonic-client niche looks like low tens of thousands of
installs across all apps and all time.

**The arithmetic against the bar.** $1k MRR at, say, $3/month net of Apple's cut needs roughly
**400–500 concurrent paying subscribers**; $3k needs ~1,200–1,500. That is on the order of the *total
lifetime rating count of the single most successful paid app in the category* — and it must be
sustained concurrently, reached from a cold start with no audience, against free and mature
incumbents, in a population that is famously unwilling to pay for software and technically capable of
building its own. **Verdict: this variant cannot plausibly reach $1–3k MRR.** It is a technically
perfect, legally clean product with an addressable market too small to clear the bar — precisely the
failure mode the ticket asked to be tested for.

---

## Section 5 — The taste layer, and the inverted architecture

### 5.1 ListenBrainz — the one clean, cheap, commercially-usable component

**VERIFIED** (listenbrainz.readthedocs.io/en/latest/users/api/): open HTTPS API, **1 request/second**
per client application, `Authorization: Token <token>` per-user auth, `429` on excess, higher limits
possible for authenticated calls. A valid contactable `User-Agent` is **mandatory** — requests without
one "may be blocked without further notice". Provides listening history, playlists and recommendations.
**No audio.**

**Commercial use is permitted and affordably priced. VERIFIED** (metabrainz.org/supporters/account-type):
MetaBrainz asks any organisation "with a current revenue stream or an expected revenue stream" to take a
commercial supporter tier. Published monthly minimums:

| Tier | Price | Intended for |
|---|---|---|
| Non-profit / open-source | $0.00+ | research bodies, "authors of open-source software using our data" |
| Stealth Start-Up | $0.00+ | "start-ups… not publicly offering their services" |
| **Bronze** | **$100.00+** | **"popular mobile apps and… small to mid-size start-ups with public products"** |
| Silver | $600.00+ | mid-size start-ups, established companies |
| Gold | $1250.00+ | established companies, >20 people |
| Unicorn | $2000.00+ | larger companies, special agreements |

**Bronze at $100/month is the correct tier for this app and is the only affordable, verified,
commercially-clean price in this entire research file.** Note that pre-revenue start-ups anticipating
income are pushed to a commercial account, so the $0 stealth tier is a development-phase option, not a
launch posture.

**VERIFIED** (metabrainz.org/social-contract): "We won't object to commercial use of our content,
companies can use the work of our volunteers without any charge, but charging for the content itself is
forbidden." Factual database content is public domain; non-factual content is under Creative Commons
licences. **CC0 is not named — NO EVIDENCE FOUND** on the specific licence identifier for ListenBrainz
listen data (the /data/ page requires JavaScript and returned no text; the social contract explicitly
"is not a legally binding contract" and defers to per-project licence pages).

**The strategic point: this genuinely decouples personalisation from the music source.** ListenBrainz can
supply the taste model even where audio comes from somewhere else entirely. It does *not*, however,
rescue the licensed-catalogue variant — knowing what a user likes is worthless if the catalogue you can
legally play does not contain it. **It rescues the *modelling* problem, not the *repertoire* problem.**

### 5.2 Last.fm — BLOCKED for this use

**VERIFIED** (last.fm/api/tos): the default grant is non-commercial only — "You are permitted to use the
Last.fm Data solely for non-commercial purposes and for no other purpose". Clause 3.2: "If at any time
you wish to use the Last.fm Data for commercial purposes, **you must apply for a commercial use
agreement**." Commercial use without approval is a material breach permitting immediate cutoff and
damages; enquiries go to partners@last.fm.

**Pricing: NO EVIDENCE FOUND** — no fee schedule is published. The licence is royalty-free for permitted
uses, but "Last.fm reserves the right to share in revenue generated from your use of Last.fm Data in
future" and may attach "additional terms and/or charges".

Other verified constraints: a **"Reasonable Usage Cap" of 100 MB** of stored data; mandatory caching per
returned HTTP headers; mandatory "powered by AudioScrobbler" attribution and backlinks; **no
sub-licensing**; audio, video, images and artwork expressly excluded; and the agreement "doesn't grant
any music streaming rights". Rate limits are set at Last.fm's sole discretion with no published number.
Governing law England.

**Verdict: use ListenBrainz, not Last.fm.** ListenBrainz publishes a price; Last.fm requires a
negotiation with an unknown outcome and an unknown price, and caps stored data at 100 MB.

### 5.3 Icecast / Liquidsoap — technically elegant, legally the worst option

**The technical claim is CONFIRMED. VERIFIED** (liquidsoap.info docs 2.4.5 quick_start and crossfade
pages):
- **`output.icecast`** exists and "actively pulls data out of the graph and sends it out"; `%vorbis`,
  `host`, `port`, `password`, `mount` parameters documented. `output.file.hls` is an HLS alternative.
- **Splicing multiple heterogeneous sources is core functionality:** `playlist`, `single`, `switch`,
  `fallback`, `random`, `add`, `sequence`. The shipped example composes a music playlist, a jingles
  playlist and a fallback file — structurally identical to music + podcast + TTS.
- **Crossfading is first-class:** `cross` ("an advanced crossfading operator"), the ready-made
  `crossfade` wrapper in `utils.liq`, and `cross.smart` with `~fade_in`, `~fade_out`, `~high`,
  `~medium`, `~margin`, `~default` — transitions chosen per-rule from measured dB levels at the track
  boundary.
- Practical gotcha: playlists are *fallible* and outputs demand infallible input, so `mksafe`, a
  `fallback` security source, or `fallible=true` is required.

**So this architecture does solve the interleaving and crossfade problem outright** — the splice is
yours, which is exactly what every platform-native route denies. It is the only architecture in the
whole sweep where concept steps 3–5 are fully implementable.

**And it is the worst option on every other axis.**

1. **Licensing burden becomes entirely the developer's** — straight back to Section 2, including the
   $0.0032/performance escalator, the unknown ASCAP/SESAC/GMR minimums, and the advance-announcement
   bar.
2. **It makes the interactivity problem *worse*, not better. INFERRED (high confidence):** a
   per-listener personalised stream is closer to "a program specially created for the recipient" —
   §114(j)(7)'s exact language — than anything else considered here. The architecture that best solves
   the engineering problem is the one most likely to be classed **interactive** and therefore
   ineligible for the statutory licence.
3. **The one-to-many economics invert. VERIFIED** (quick_start): "It is then sent by the server to
   every listener. The more listeners you have, the more bandwidth you need." Icecast's efficiency comes
   from duplicating *one* feed to many listeners. Personalisation requires **one Liquidsoap source
   chain and one mount point per listener** — so the developer pays for N independent
   encode-plus-crossfade pipelines. **NO EVIDENCE FOUND** in the Liquidsoap docs on per-user stream
   patterns; the documentation index has no such topic, which is itself telling. **INFERRED:** CPU cost
   scales linearly with concurrent listeners because every stream is separately mixed and encoded, and
   bandwidth scales linearly too — the opposite of the broadcast economics Icecast was built for. A
   server-side per-user pipeline is a per-user marginal cost against a $3/month subscription.
4. **It breaks §4.3's safe harbour.** Server-side mixing means the developer handles the audio, which
   forfeits the private-performance argument that made the self-hosted variant licence-free.

**Verdict: technically feasible, and the correct answer only if the licensing problem were already
solved.** It is not a route around Section 2; it is the most expensive way into it.

---

## Synthesis — which routes are live, and what each costs

| Route | Live? | Cost (verified only) | Plays music the user knows? |
|---|---|---|---|
| **Feed.fm / Adaptr** | Conditionally | Enterprise: NO EVIDENCE FOUND. Adaptr 2021 archived: $99–$2,399/mo (SECONDARY, stale) | **YES** — major-label, chart artists |
| **Tuned Global** | No | n/a — cannot license you music | n/a |
| **7digital / MassiveMusic** | No, at this budget | NO EVIDENCE FOUND + you pay all royalties | Yes, but unreachable |
| **Mubert** | No | $99/$249/$999/mo published, caps ~16 listeners | **No** — AI-generated, instrumental |
| **Direct statutory (US)** | Conditionally | $1,000/yr/channel + $350 BMI verified; ASCAP/SESAC/GMR unknown; **$0.0032/performance is the real cost** | **YES** — full repertoire |
| **Deezer** | No | n/a — non-commercial ToS | n/a |
| **Subsonic / Jellyfin** | **YES** | **$0** licensing | **YES** — it is literally their own library |
| **iOS local files** | Partially | **$0** | **YES** — but only non-DRM local files |
| **ListenBrainz** (taste only) | **YES** | **$100/mo** (Bronze) | n/a — no audio |
| **Last.fm** (taste only) | No | NO EVIDENCE FOUND; needs negotiated agreement | n/a |
| **Icecast / Liquidsoap** | Technically | Section 2's burden + per-listener server cost | Depends on the licence you obtain |

### The decisive question for ticket 05

**Does ANY legal route let the app play music the user already knows and likes?**

**Yes — three do. But each carries a different disqualifying cost, and none is both familiar and
affordable with the AI DJ intact.**

1. **The user's own library (Subsonic / Jellyfin / iOS local files).** The *only* route that is
   simultaneously legal, free, and guaranteed-familiar — it is the user's own music by definition, and
   Jellyfin's `PlayCount`/`IsFavorite`/`LastPlayedDate` give the richest taste signal in the entire
   sweep. It is also the only route where full crossfade and TTS interleaving work natively. **It fails
   on addressable market, not on law or engineering** (§4.6): the whole incumbent field sits at 82–425
   App Store ratings each, and $1k MRR needs ~400–500 concurrent subscribers from a cold start against
   free mature competitors. **And note it needs no advance-announcement compromise — private
   performance is outside §114 entirely, so the AI DJ can announce freely. This is the only route where
   step 5 survives fully intact.**
2. **Feed.fm / Adaptr.** Real major-label catalogue, real indemnification, live self-serve signup at
   Adaptr, actively-maintained iOS SDK. **Two unresolved blockers:** the §5.6/§5.5(a) insertion and
   alteration ban (which appears to prohibit exactly the TTS DJ inserts the concept requires), and
   pricing that is entirely unknown for the enterprise tier. **This is the route worth an enquiry
   email**, and it is the only one where the answer could plausibly change the verdict.
3. **Direct statutory licensing (US only).** Full repertoire and an affordable-looking $1,000/yr
   minimum — but the per-performance rate makes it structurally loss-making (≈$17,500/yr at 1,000
   subscribers against a $12k–36k/yr target), the artist/album complement limits are incompatible with
   artist-concentrated personal libraries, per-user channel counting is unresolved, and **the AI DJ
   must be reduced to back-announcing only.**

**The finding that reframes the brief.** The risk was framed as five-figure minimums. The real
constraint sits upstream and is not about price at all: **the pre-announcement prohibition in
§114(d)(2)(C)(ii) means "an AI DJ that announces tracks in advance" and "cheap licensed familiar music"
are mutually exclusive.** Every route to familiar music at low cost runs through either the statutory
licence (which bans advance announcement) or a vendor's stream (which bans insertion). Only the user's
own files escape both — and that route's market is too small.

**A live secondary option worth noting:** the concept's *non-music* components carry no equivalent
blocker. Third-party podcasts via open RSS, news audio, ListenBrainz taste data at $100/month, and TTS
DJ segments are all tractable for a solo developer. **And announcing podcast episodes in advance is
entirely legal**, because the pre-announcement bar is a sound-recording statutory-licence rule. A
spoken-audio-first product would keep the DJ premise and drop the component that is structurally out of
reach.

**Do not treat as settled:** Feed.fm and 7digital pricing and minimums; whether Feed.fm would grant
written consent for TTS inserts; Adaptr's current pricing and eligibility; Mubert's Custom-tier terms;
ASCAP/SESAC/GMR minimums; PPL and PRS figures; SoundExchange per-user channel counting; whether a
liked-songs pool survives §114(j)(7) after *Launch Media*; and podcast insertion rights. **Do not budget
for "find a cheaper music API" — the obstacle is not the vendor's price list.**

### Method limitations

**WebSearch was unavailable for the entire session** (HTTP 400 validation error on every call), and
DuckDuckGo/Mojeek served CAPTCHAs. All findings rest on direct primary-source fetches, the Wayback CDX
API, the GitHub and Docker Hub APIs, the iTunes Search API, and direct inspection of the local iOS 27.0
SDK headers. **Consequence: "NO EVIDENCE FOUND" means "not found in this session", not "does not
exist".** The verdicts are robust for primary sources and archives but weaker for secondary reporting —
press interviews, conference talks, job postings and forum threads mentioning deal sizes went largely
unsearched. A follow-up pass with working search is worth doing specifically on Feed.fm contract values
and any customer disclosure of pricing. Several sites also returned 403 to automated fetches
(ascap.com, soundexchange.com, plex.tv, Justia, G2, Crunchbase), so the PRO and SoundExchange figures
above come from eCFR/Cornell and BMI rather than the collectives' own pages.

### Corrections to the scout's candidate list

1. **Tuned Global does not offer sub-licensing** and cannot license you music — it is technology plus
   consultancy. Remove it as a B2B peer of Feed.fm.
2. **Deezer's blocker is its non-commercial ToS, not its SDK.** The scout's "portal live, history
   endpoint responding" reading was over-optimistic: `/user/me/history` returns HTTP 200 with an
   OAuthException error body, and ToS Section IV forbids commercial use outright.
3. **Jellyfin is not natively Subsonic-compatible** — only via an unmaintained third-party bridge. Use
   its own API, which is better anyway and has an official Swift SDK.
4. **Funkwhale implements only a subset** of the Subsonic API, and admins can disable it.
5. **Mubert's "sub-licensing on a paid tier"** is not supported by any readable licence text; the
   readable text bars distribution via Music Streaming Services.
6. **Feed.fm's insertion/alteration ban** was not in the scout's table and is the most important
   single clause about that vendor.




