# 01 — Spotify: can a third-party app programme an interleaved radio stream?

Type: research
Status: resolved
Blocked by: —

## Question

Produce a **feasibility verdict for Spotify**: feasible / partial / blocked, each with the specific
API limitation or ToS/Policy clause that decides it. Quote clauses verbatim with URLs.

Build on prior work (`../podcasts-notes-validation/docs/research/01-audio-access-legality.md`) —
**verify, don't re-derive** — and state explicitly which findings are carried over and which are new.

Answer in this order, because the interleaving question is the one that likely decides the map:

1. **The integration clause (highest priority).** Prior work found the Spotify Developer Policy bars
   any product "integrated with streams or content from another service", flagged INFERRED-fatal.
   Find the current clause, quote it exactly, cite it, and determine whether it prohibits an app that
   alternates Spotify music with third-party podcast and news audio in one continuous session. Is
   there any carve-out, waiver, quota-extension request, or commercial-partner route? Distinguish
   "prohibited by the letter of the policy" from "prohibited in practice / enforced".
2. **Reading user data.** Which endpoints expose recently-played / listening history, liked (saved)
   tracks, top artists and tracks, and followed or saved **podcasts**? Note rate limits, the
   extended-quota-mode application process and its approval criteria, and any restriction on
   retaining or deriving from that data.
3. **Playback control.** Confirm or refute that the iOS SDK is remote-control only and requires the
   Spotify app installed. Can a third-party app pause mid-track, play non-Spotify audio, and resume
   at position? What about the Web Playback SDK — and is it usable on iOS Safari / in a WKWebView at
   all? Note the Premium-only requirement.
4. **Crossfading and mixing.** Can Spotify audio be crossfaded with, ducked under, or overlaid by
   third-party audio (a podcast segment, an AI-generated DJ intro)? Prior work established no
   PCM/decoded-audio surface exists — confirm and cite. If audio cannot be mixed, what is the best
   achievable transition (hard cut, gap, sequential playback), and does anything prohibit even that?
5. **Offline and caching.** Re-verify ToS IV.3.b on local caching. What does this mean for
   continuous playback during a commute with poor connectivity?
6. **Attribution and UX requirements.** Any mandated branding, link-backs, "powered by Spotify"
   requirements, or prohibitions on the app appearing to be the primary player.

Then state the verdict, and separately: **hard blockers vs workarounds.** A workaround that violates
a clause is not a workaround — say so.

## Answer

**BLOCKED — not partial.** The Spotify-native variant of this concept is prohibited by the letter of the
Developer Policy, and it fails four times over for independent reasons. (1) The core mechanic is
prohibited: Policy **III.7** bars any device or system from "segue, mix, re-mix, or overlap any Spotify
Content with any other audio content (including other Spotify Content)", and **III.5** bars "any product
or service which is integrated with streams or content from another service". Spotify's own Compliance
Tips page — a primary source on developer.spotify.com — lists "**DJ/Mixes**: Using Spotify's catalog to
segue, mix, re-mix, or overlap any Spotify Content with any other audio content" among "common app use
cases that break our policy, and you should not build". The developer would not be arguing a contractual
ambiguity; they would be arguing against Spotify's own published example of the forbidden thing.
(2) The business model is prohibited: Policy **IV.2** bars all commercialisation of Streaming SDAs, and
the ToS define Streaming to include "using the Spotify Platform to control a background Spotify
application" and a Streaming SDA as one providing streaming functionality "regardless of whether it also
provides additional functionality". Compliance Tips' own disallowed example is "Charging users $5/month
for a home automation app that includes the ability to trigger music" — structurally identical to this
app's pricing plan. (3) Access is capped at **5 users**: since 15 May 2025 extended quota mode accepts
applications "from organizations (not individuals)" only, requiring "at least 250k MAUs" on an already-
launched service. A solo developer with no audience can never qualify, and Spotify states development
mode "should not be relied on as a foundation for building or scaling a business on Spotify."
(4) The commute case fails: ToS **IV.3.b** permits caching only of metadata/cover art or Conditional
Downloads, and no offline API exists in any third-party SDK. Any one of these is fatal at the map's
$1k/mo floor. **Per the map's ranking this does not end the map** — it eliminates the Spotify branch and
raises the prior that Apple Music / YouTube Music / Tidal fail similarly, since all four license from the
same rightsholders.

### Per-question findings

1. **Integration clause — BLOCKED.** III.5 confirmed verbatim and current (effective 15 May 2025);
   CARRIED OVER from the prior project and verified unchanged. NEW: it is the *weakest* of three
   applicable clauses. III.7 ("segue") is decisive and its "(including other Spotify Content)" parenthesis
   closes the obvious workaround; III.4 (non-interactive webcasting) applies weakly to a lean-back
   station. NEW and decisive: Compliance Tips is Spotify's published interpretation and names this app's
   shape. **No carve-out**: III.1 and III.11 carry explicit "written approval" escape hatches — III.4,
   III.5 and III.7 carry none, a deliberate drafting signal. Quota-extension route closed on eligibility
   before merits. The only legitimate route is a "Separate Agreement" (ToS IX.1), out of scope per the
   map. *Prohibited by the letter: unambiguously yes. Prohibited in practice: NO EVIDENCE FOUND of named
   enforcement (my web-search tool errored throughout — a real gap), but the question is largely moot
   because extended quota mode requires Spotify's review team to "test out your app" for policy
   compliance. The gate precedes enforcement.*
2. **User data — PARTIAL, toy scale only.** Endpoints exist: `/me/player/recently-played`
   (`user-read-recently-played`, max 50, cursor-paged), `/me/tracks`, `/me/top/{type}` (`long_term` ~1yr /
   `medium_term` ~6mo / `short_term` ~4wk), `/me/shows`, `/me/episodes`, `/me/following`, `/me/player`.
   Two NEW holes: recently-played "Currently doesn't support podcast episodes" — so podcast play position
   history, the key input, is absent; and **Recommendations, Audio Features, Audio Analysis and Related
   Artists are unavailable to any app registered after 27 Nov 2024**, removing the signal a music
   programmer needs. Retention constrained by ToS IV.3.a ("Do not store Spotify Content indefinitely"),
   V.7, and deletion within 5 days of disconnect. NEW and serious: Policy **III.13** bars "building
   profiles of users" — arguably describes a personalisation engine. Numeric rate limits are not published.
3. **Playback control — PARTIAL, confirmed remote-control only.** CARRIED OVER and deepened: I cloned
   github.com/spotify/ios-sdk (v5.0.1) and read the public headers. README verbatim: "the Spotify app must
   be installed since it handles the actual music playback"; Premium required for on-demand URIs.
   Pause / play third-party audio / resume-at-position **is** technically supported —
   `seekToPosition:`, `playbackPosition`, and Web API `position_ms` — a sharper affirmative than the prior
   project gave. But it will gap and re-buffer, and iOS backgrounding is documented as fragile ("always
   disconnect App Remote when your app enters a background state"), which is a reliability risk in the
   commute scenario specifically. Contractually, doing it *as a programmed sequence* is precisely III.7.
   CORRECTION to a common assumption: the Web Playback SDK **does** claim iOS/mobile Safari support (with
   an autoplay-gesture caveat) and **does** expose `setVolume`; WKWebView is not mentioned anywhere and is
   untested.
4. **Mixing — BLOCKED, and even a hard cut is barred.** No PCM surface: upgraded from the prior project's
   INFERRED to VERIFIED by exhaustive grep of the iOS SDK headers for `volume|pcm|sampleBuffer|AVAudio|duck`
   — **zero matches**. Web playback runs through EME. NEW: `getCrossfadeState:` exists but
   `SPTAppRemoteCrossfadeState` is **read-only** (`enabled`, `duration`, no setter) — Spotify lets you
   observe its user's crossfade setting and nothing more. Best *achievable* transition is a hard cut with
   an audible gap; that is prohibited by III.7's "segue", a verb distinct from "overlap". **The best
   achievable and the best permitted transition do not overlap at all.**
5. **Offline — BLOCKED, and architecturally, not just contractually.** IV.3.b re-verified verbatim.
   "Conditional Downloads" means "time-limited offline syncing" and is a Spotify-app feature: the iOS docs
   name "offline caching" as offloaded *to* the Spotify app, and there is no download or prefetch API in
   the public SDK. Result: podcast audio caches freely (own player, open RSS), music does not. In a tunnel
   the podcast plays and the music silently fails — the worst failure mode in the developer's own primary
   use case.
6. **Attribution — FEASIBLE but constraining, plus one independently fatal clause.** II.4 mandates
   attribution via the Spotify Marks and link-backs; II.5 bars "playback of Spotify Content without
   showing relevant cover art and metadata" (satisfiable, but the app can never be purely audio — CarPlay
   must carry it); VI governs naming; II.1 and ToS IX.7 bar implying endorsement. "Powered by Spotify" as
   a phrase: NO EVIDENCE FOUND. No primary-player prohibition found, though III.11 (mimicking a core
   Spotify experience) and the Design Guidelines' "Spotify content should never be seated next to content
   from similar services" both bite. **The clause that matters here is IV.2**: monetisation of a Streaming
   SDA is banned outright — subscription, in-app purchase, and in-app advertising each named.

### Hard blockers vs workarounds

**Hard blockers** (each independently sufficient; none waivable):
- **B1** Interleaving Spotify with third-party audio — Policy III.7. *No written-approval clause.*
- **B2** Being integrated with another service's streams/content — Policy III.5. *No written-approval clause.*
- **B3** Any monetisation — Policy IV.2 + ToS "Streaming SDA" definition. **Kills the $1–3k MRR bar by itself.**
- **B4** More than 5 users — dev-mode cap; extended quota is organizations-only, ≥250k MAU. *Eligibility, not discretion.*
- **B5** Offline music for the commute — ToS IV.3.b + no offline API. *Architectural.*
- **B6** Any mixing/ducking/crossfade — no PCM surface; ToS IV.1.a.ii bars circumvention.
- **B7 (probable)** Policy III.13 "building profiles of users" — arguably describes the personalisation engine. *Lawyer question.*
- **B8 (probable)** Policy III.14 "otherwise ingest Spotify Content into a machine learning or AI model" — hits the AI-DJ feature. *Lawyer question.*

**Not workarounds** (each violates a clause, or is not the product):
- Hard cut instead of crossfade → III.7 "segue".
- Sequence only Spotify tracks → III.7 "(including other Spotify Content)", and loses the concept.
- Stay in development mode → complies, caps at 5 users; Spotify says dev mode is not a foundation for a business.
- Free app monetised externally → IV.2 bars "sale of access"; and doesn't touch B1/B2/B4.
- `setVolume` on the Web Playback SDK to duck → a capability, not a permission. III.7 "overlap".
- Ship quietly and hope → extended quota review requires Spotify to test the app against the policy.
- Spotify for metadata only, music from a licensed catalogue → arguably compliant, but that *is* the
  licensed-catalogue variant; the Spotify login adds III.13 exposure and the 5-user cap while adding
  little product value over asking the user their taste directly.
- Get a Separate Agreement (ToS IX.1) → the only legitimate route; gated behind organizations-only /
  ≥250k MAU, and out of scope per the map.

**The one genuinely feasible Spotify thing**: a *Non-Streaming* companion that reads saved shows and
liked tracks to build/export a Spotify playlist or deep-link into Spotify, while your own player handles
only open-RSS podcast audio and never programmes music. Monetisable (IV.3; Spotify's own permitted
example is a $5/month playlist manager). But it is a playlist tool, not a radio station — it cannot
interleave. And still capped at 5 users.

**Full findings, with verbatim clause quotes and URLs:**
[`docs/research/01-spotify-feasibility.md`](../docs/research/01-spotify-feasibility.md)
