# 04 — Tidal: feasibility verdict

Type: research
Status: resolved
Blocked by: —

## Question

Produce a **feasibility verdict for Tidal**: feasible / partial / blocked, with the specific API
limitation or ToS clause. Quote verbatim with URLs.

**Entirely new ground** — the prior project did not examine Tidal. Tidal relaunched a public
developer platform, so this may be the most permissive of the four. Establish whether that is true.

1. **The developer programme.** What exists today (developer.tidal.com)? Which APIs and SDKs, on
   which platforms (iOS/Android/web)? Is access open self-serve, or application-gated with review?
   Is there a published app-approval or commercial-use process, and does a paid/revenue-generating
   app need a commercial agreement?
2. **Playback.** Is there a playback SDK that streams audio, or only metadata and remote control?
   Can an app build its own queue, pause mid-track, resume at position? Subscription tier required?
3. **The interleaving question.** Any clause on combining Tidal content with third-party audio in
   one session? Any surface for crossfade, ducking, or per-stream volume? Or is a hard cut the
   ceiling? Can Tidal playback coexist with another audio player in one session?
4. **User data.** Endpoints for listening history, favourites/liked tracks, top items. Any podcast
   data at all?
5. **Offline/caching** rules, for the commuting case.
6. **The commercial reality check.** Tidal's subscriber base is far smaller than Spotify's or Apple
   Music's. Even if it is the most permissive platform, estimate the **addressable audience**:
   published or credibly reported Tidal subscriber numbers, and what fraction plausibly overlaps with
   podcast listeners. A permissive-but-tiny platform may be technically feasible and commercially
   pointless — say so if the numbers imply it, and feed this into the revenue model.

State the verdict, plus hard blockers vs workarounds.

## Answer

**Verdict: BLOCKED — and Tidal is the *least* permissive of the four platforms, not the most.**

The ticket's premise ("Tidal relaunched a public developer platform, so this may be the most
permissive of the four") is **false**, and it is falsified by Tidal's own published Developer
Guidelines rather than by inference. Tidal has the *best* technical developer offering in the sweep
(real playback SDK on iOS/Android/Web, 256-path JSON:API) and the *worst* legal one.

Tidal Developer Guidelines v3.0 §III lists, as separately enumerated strictly prohibited offering
types, **four clauses each of which alone kills this concept** — plus a fifth in the Terms. Full
research, with verbatim quotes and URLs: **[`docs/research/04-tidal-feasibility.md`](../docs/research/04-tidal-feasibility.md)**

### Per question, in brief

1. **Developer programme** — Real and actively maintained. Self-serve signup (dashboard → Client
   ID/Secret), first-party open-source SDKs for **all three** platforms, public REST API. But
   two-stage access: self-serve "in development" with quotas, then **manual review of your source
   code** to reach Production, tied to a declared use case and revocable. No published commercial
   agreement or revenue-share; Guidelines §III bar business/commercial-setting offerings absent
   written approval, so a paid app is outside the self-serve grant. **Open to sign up, gated to ship.**
2. **Playback** — A genuine audio engine (AVPlayer / ExoPlayer), not remote control. Own queue,
   `pause()`, `seek(position)`, `assetPosition`, `setNext()` all present — mechanically the exact
   primitives the concept needs. **But third parties get 30-second previews only**; full-length
   requires `Device Login` (internal-only) or Embeds (web-only), and needs the end user to be a
   paying Tidal subscriber. **PARTIAL on paper, BLOCKED in practice.**
3. **Interleaving — HARD BLOCKER, verbatim and unambiguous.** Guidelines §III: *"Integrating TIDAL
   content with content or streams from another service"* and *"functionality that segues, overlaps,
   mixes or re-mixes any TIDAL content … or combines any TIDAL content with any other audio content
   (including without limitation other TIDAL content)"*. Note the parenthetical — it bars segueing
   Tidal into **Tidal**, so there is no narrower same-service variant. Terms §II separately bars
   *"Use TIDAL Content in podcasts"*. Crossfade **does** exist (0–15s, web + iOS) but only accepts a
   `MediaProduct` (`TRACK`/`VIDEO`), so it cannot be pointed at external audio. No ducking API
   anywhere; volume is one global Tidal-wide level. **Coexistence: contractually no; technically the
   API enforces `CONCURRENT_PLAYBACK` 403s and revokes streaming privileges.** A hard cut is the
   technical ceiling — and a hard cut is a segue, so it is barred too.
4. **User data** — Favourites/collection **yes** (`collection.read`, `THIRD_PARTY` tier),
   algorithmic mixes **yes** (daily/discovery/new-release). **Listening history: no endpoint at
   all** (0 hits across 256 paths) and Terms §II expressly names "listening history" as off-limits
   to mining; all-user-data access is reserved to the `INTERNAL`-only `r_usr` scope. **Podcasts:
   zero** — no podcast/episode resource in the API, `ProductType` is `{TRACK, VIDEO}` only, and
   Tidal **discontinued all podcasts on 24 Jul 2024**.
5. **Offline/caching — BLOCKED.** The SDK has a full Offline Engine, but Terms §II limits third
   parties to *"the temporary caching of metadata and cover art"* and *"Do not store TIDAL Content
   indefinitely"* — the same formula as Spotify ToS IV.3.b. The developer's own commuting use case
   is the worst-served case on this platform.
6. **Commercial reality — pointless even if permitted.** Tidal has **never disclosed subscriber
   counts** and Block does not either; the last verifiable figure is **2.1M (2020, court
   documents)** and **no credible estimate exists for any year after 2020**. Share is **under 1%**
   and Tidal is not tracked as a named service by MIDiA/Statista. Block has **written off ~$266M of
   the $297M** purchase price (~89%) and said it is *"scaling back our investment in Tidal"*; a
   Delaware judge called the acquisition *"a terrible business decision"*. No free tier
   ($11.99/mo individual), so users would need two subscriptions. **Do not put a current subscriber
   number in the revenue model — there isn't one.** Tidal cannot plausibly reach the $1k/mo floor.

### Hard blockers vs workarounds

**Hard blockers (10, any one sufficient):** HB1 integrating with another service's streams;
HB2 combining Tidal audio with any other audio *including Tidal's own*; HB3 "Use TIDAL Content in
podcasts" (no approval carve-out); HB4 AI/ML prohibition (appears twice, once with no carve-out);
HB5 "Generating news media"; HB6 previews-only playback; HB7 caching limited to metadata/cover art;
HB8 no podcast content exists on Tidal; HB9 source-code review gate; HB10 undisclosed sub-1%
audience with a disinvesting owner.

**Not workarounds:** crossfading podcast into music (API only accepts Tidal `MediaProduct`s);
two parallel players with a hard cut (a hard cut is a segue → HB2; also HB1, plus
`CONCURRENT_PLAYBACK` revocation and a process-wide `AVAudioSession` fight); Tidal-music-only
interleaving (HB2's parenthetical covers it, and it is no longer the concept); shipping unapproved
in "in development" mode (quota-limited, preview-only, monitored); using the raw `url` from
`/trackFiles/{id}` with your own mixer — Guidelines §II explicitly anticipates and refuses this:
*"TIDAL will reject any quota extension requests for any Offering that attempts to circumvent
this."* Seeking express written approval is legitimate in principle but is a negotiated licence the
map puts out of scope, and HB3/HB4 have no approval carve-out at all.

### For the map

- **Tidal is eliminated, and must not be treated as a fallback** if Spotify/Apple/YouTube Music
  fail — it is strictly worse than all three. The map's note on Tidal should be corrected, not
  softened.
- **This strengthens the inherited Spotify finding.** A near-identical "another service" clause now
  appears, independently drafted, in a second platform's terms — evidence it is a standard
  label-licensing requirement rather than one platform's idiosyncrasy. **INFERRED
  (moderate-to-high):** the platform-native variant is likely blocked everywhere, and the surviving
  concept lives in the licensed-catalogue and local-library variants.
- **Two constraints generalise:** the "metadata and cover art only" caching formula now appears
  verbatim in both Spotify's and Tidal's terms (treat the offline/commuting case as blocked on any
  platform-native variant until proven otherwise); and explicit AI/ML prohibitions on catalogue
  content are appearing in developer terms, which bears on the AI-intro feature everywhere.
- **All findings here are new.** Nothing was carried over from `podcasts-notes-validation`, which
  did not examine Tidal.
