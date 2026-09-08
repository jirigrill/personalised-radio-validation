# 02 — Apple Music / MusicKit: can a third-party app programme an interleaved radio stream?

Type: research
Status: resolved
Blocked by: —

## Question

Produce a **feasibility verdict for Apple Music**: feasible / partial / blocked, with the specific
API limitation or ToS clause. Quote verbatim with URLs.

**This is new ground.** Prior work covered Apple *Podcasts* (no playback SDK, `MPNowPlayingInfoCenter`
write-only) but **not** Apple **Music** / MusicKit. Do not assume the Podcasts finding transfers.

1. **Playback control.** What does MusicKit for iOS/Swift actually permit? `ApplicationMusicPlayer`
   vs `SystemMusicPlayer` — can an app build its own queue, pause mid-track, and resume at position?
   Does playback require an active Apple Music subscription, and what happens for non-subscribers?
2. **The interleaving question.** Can `ApplicationMusicPlayer` be paused so third-party audio (a
   podcast enclosure, an AI-generated intro) plays, then resumed? Critically: **can Apple Music audio
   and third-party audio coexist in one `AVAudioSession`**, or does MusicKit playback take exclusive
   control and interrupt/stop when another player starts? This is the make-or-break technical detail.
   Look for the documented interruption and audio-session behaviour, plus developer-forum evidence.
3. **Crossfading and mixing.** Is there any surface exposing decoded audio or per-stream volume
   control for ducking? Or is a hard cut the ceiling?
4. **Reading user data.** What does MusicKit expose of library, recently played, and heavy rotation?
   Is there a listening-history endpoint at all? Can it read the user's **podcast** subscriptions
   (likely no — Apple Podcasts is separate; confirm and cite).
5. **The terms.** Apple Music API / MusicKit terms and the Apple Developer Program License Agreement:
   any clause on combining Apple Music content with third-party content, on presenting it as a radio
   station, or on the app resembling a broadcast? Note any attribution requirements.
6. **Apple Music Radio / stations.** Apple already ships algorithmic stations. Can a third-party app
   programme or interleave against them, and does their existence create a competitive-feature threat
   (cross-reference the incumbent-feature ticket)?

State the verdict, plus hard blockers vs workarounds.

## Answer

**Verdict: BLOCKED — on contract, not on code.**

The technical mechanic largely works, and the ticket's designated make-or-break question resolves in the
concept's favour: Apple Music audio and third-party audio *can* coexist in one `AVAudioSession`, and
pause → play own audio → resume is supported. The concept dies on the Apple Developer Program License
Agreement instead. §3.3.6(D) states that "MusicKit Content cannot be synchronized with any other
content" — which names the interleaving mechanic exactly — and separately limits MusicKit use to
"purposes unrelated to facilitating access to Your end users' Apple Music subscriptions" being
prohibited, with relevance decided at "Apple's sole discretion". A third, easily-missed clause in the
same section bars requiring payment for or indirectly monetising Apple Music access "(e.g. in-app
purchase, advertising, requesting user info)", reinforced by App Store Review Guideline 4.10. That last
one is arguably the more decisive finding, because it survives any amount of engineering: even a variant
that satisfied the synchronization clause would have **no legal revenue model** at this map's $1–3k MRR
bar. Note that there is **no** broadcast/radio-station prohibition and **no** standalone MusicKit ToS —
both would be wrong to cite.

### Per-question findings

1. **Playback control — FEASIBLE.** `ApplicationMusicPlayer` (not `SystemMusicPlayer`, which mutates the
   user's real Now Playing and has no macOS support) gives an app-owned queue, insert/append via
   `EntryInsertionPosition`, `pause()`, `play()`, and a **settable** `playbackTime`. Genuinely better
   than Spotify's remote-control-only SDK. Catalog playback needs an active Apple Music subscription;
   the documented pattern is to pre-check `MusicSubscription.canPlayCatalogContent` and disable UI —
   NO EVIDENCE FOUND for the error thrown otherwise. No preview-only path via a MusicKit player.
   Requires paid Apple Developer Program membership plus a MusicKit identifier and ES256 key.
2. **Interleaving — FEASIBLE technically.** `AVAudioSession` is a per-process singleton and MusicKit
   plays in-process. `prepareToPlay()` is documented as "interrupting any active (**nonmixable**) audio
   sessions" — the parenthetical is the answer. Configure `.playback` + `.mixWithOthers` once at launch
   and never change it (mid-flight category changes interrupt the running player). Apple's retired
   *AddMusic* sample shipped this exact pattern. MusicKit exposes no `AVAudioSession` property and no
   session config point; the only lever is the background-audio Info.plist entry.
3. **Crossfading — BLOCKED. Hard cut is the ceiling.** Exhaustive symbol enumeration found **zero**
   occurrences of "volume", "gain", or "mute" in MusicKit. No decoded audio, no `MTAudioProcessingTap`,
   no `AVAudioEngine` seam; `PlayParameters` is documented "opaque" with no members. The iOS 18+
   `transition`/`crossfade(duration:)` is duration-only and works **only between entries inside
   MusicKit's own queue** — your podcast MP3 cannot be a queue entry (`Queue.Entry.Item` admits only
   `song` and `musicVideo`). `duckOthers` is a binary step, not a ramp, and Apple says to use it "on a
   temporary basis only". Apple keeps beat-matched AutoMix for itself.
4. **User data — FEASIBLE for music, BLOCKED for podcasts.** Library, ratings (read via
   `/v1/me/ratings/...`, *not* the write-only favorites endpoint), heavy rotation, recommendations and
   Replay are all readable. But there is **no listening-history endpoint** — recent plays cap at 10
   resources / 30 tracks with no documented lookback, so you must accumulate your own record with a
   cold start per user, and `ApplicationMusicPlayer` playback doesn't even update Apple's play counts.
   **No Apple API exposes user podcast subscriptions, follows, or history** — zero podcast symbols in
   MusicKit; iTunes Search API is public catalog only; `MPMediaQuery.podcasts()` "only returns podcasts
   downloaded to the user's library". This independently breaks map step 2 on iOS.
5. **The terms — BLOCKED, fatal.** See headline. DPLA §3.3.6(D) quoted in full in the research file.
   Apple Music Identity Guidelines compliance is mandatory, so the app must wear Apple Music branding
   and cannot present as a neutral station. The "standard media controls… not misrepresent the
   functionality" clause additionally conflicts with a continuously auto-advancing station.
6. **Apple Music Radio — PARTIAL; Apple threat LOW near-term, strategic risk HIGH.** `Station` is a
   `PlayableMusicItem`, so you can play Apple's stations — but they are opaque, with no API to inspect
   or manipulate the queue. Apple has announced **no** AI DJ; its Music AI spend is on transitions and
   lyrics, its hosts are human, and it moved spoken shows out of Music into Podcasts in 2023. The real
   incumbent is **Spotify**: AI DJ since Feb 2023 (voice requests since May 2025), and *Your Daily
   Drive* (June 2019) already shipped music+news interleaving — direct prior art for this concept's core
   mechanic, viable only because Spotify owns both rails. NO EVIDENCE FOUND of any third-party app
   interleaving Apple Music with podcasts; that absence is evidence of the constraint.

### Hard blockers vs workarounds

**Hard blockers (no workaround):**
- DPLA §3.3.6(D) no-synchronization clause — names the core mechanic.
- DPLA §3.3.6(D) purpose-limitation, at Apple's sole discretion.
- DPLA §3.3.6(D) + ASRG 4.10 no-monetisation — kills the revenue model independently.
- No podcast user data on any Apple API.
- No volume/gain/decoded-audio surface — true crossfade impossible.

**Partial constraints:** no listening history; standard-media-controls clause; mandatory Apple Music
branding; platform gaps (crossfade iOS 18+ iPhone/iPad only, playback macOS 14+, no watchOS playback, no
automatic user token on Android).

**Not blockers — do not cite:** the audio session; playback control; a broadcast/radio prohibition (does
not exist); a standalone MusicKit ToS (does not exist).

**Non-workarounds, stated explicitly:**
- *Sequential handoff* solves the audio problem and **not** the licence problem — interleaving is what
  the clause names. A workaround that violates a clause is not a workaround.
- *Going free/ad-supported* fails: the clause names advertising and "requesting user info", and a free
  app cannot clear a $1k/mo floor.
- *RSS/OPML podcast import* genuinely routes around the podcast-data blocker but touches none of the
  others.
- *Requesting an entitlement*: NO EVIDENCE FOUND of any MusicKit entitlement path. §3.3.6(B) and (C)
  describe entitlement applications for other APIs; (D) conspicuously does not. INFERRED: no door.

The only unexplored escape hatch is §3.3.6(D)'s thrice-repeated "unless otherwise permitted by Apple in
the Documentation". NO EVIDENCE FOUND that such a permission exists, and the sealed audio surface is the
opposite of a carve-out. Treat as closed unless someone finds the clause.

### Consequence for the map

The platform-native variant is **dead on Apple Music**, and stays dead however the engineering is
arranged. Per `map.md` this does not end the map — licensed/royalty-free catalogue and local-library
variants remain live. Two findings transfer: the podcast-data gap is Apple-wide (any iOS-first variant
needs RSS/OPML import regardless of music source), and the "suitable moment to interrupt" problem remains
unpriced. Flag to the incumbent-feature ticket: Spotify's *Your Daily Drive* is direct prior art.

Full findings, with verbatim quotes and URLs: [`docs/research/02-apple-music-feasibility.md`](../docs/research/02-apple-music-feasibility.md)
