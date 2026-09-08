# 01 — Spotify: can a third-party app programme an interleaved radio stream?

Type: research
Status: open
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
