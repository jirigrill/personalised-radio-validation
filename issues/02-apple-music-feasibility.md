# 02 — Apple Music / MusicKit: can a third-party app programme an interleaved radio stream?

Type: research
Status: open
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
