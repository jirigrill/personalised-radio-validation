# 03 — YouTube Music: feasibility verdict

Research for ticket `issues/03-youtube-music-feasibility.md`. **Not legal advice.** Every claim is
tagged VERIFIED (read in a primary source, URL given), INFERRED (my reasoning from those sources),
or NO EVIDENCE FOUND. Each is also marked CARRIED OVER (from
`../podcasts-notes-validation/docs/research/01-audio-access-legality.md`, re-verified in this pass)
or NEW.

## Verdict up front

**BLOCKED — both by policy and by absence of API. Redundantly, and by more clauses than prior work
found. This is the cleanest dead end in the platform sweep.**

| Requirement of the concept | Status | Governing clause / absence |
|---|---|---|
| Third-party playback of YouTube Music tracks | **Blocked by absence of API** | No YouTube Music playback SDK exists (VERIFIED by absence, NEW confirmation) |
| Background playback (screen off, commuting) | **Blocked by policy** | Developer Policies III.I.9 (VERIFIED, CARRIED OVER) |
| Audio-only music (no video on screen) | **Blocked by policy** | III.I.7, III.I.8 (VERIFIED, CARRIED OVER) |
| Programmed sequencing: podcast → song → podcast | **Blocked by policy** | III.I.7 (mixing/altering), III.I.1 substitute-service ban (VERIFIED, **NEW**) |
| Buffering ahead / offline for commute | **Blocked by policy** | III.E.1(a) and (b) (VERIFIED, CARRIED OVER) |
| Read user's YouTube Music library / liked songs | **Blocked by absence of API** (partial: YouTube *channel* likes only) | Data API `channels.contentDetails.relatedPlaylists.likes` exists but is YouTube video likes, not a Music library (VERIFIED, **NEW**) |
| Read listening / watch history | **Blocked by absence of API** | `watchHistory` / `watchLater` fully deprecated and removed (VERIFIED, **NEW**) |
| Non-API route to the audio (yt-dlp etc.) | **Blocked by policy** | III.I.14, III.E.6 (VERIFIED, CARRIED OVER) — a violation, not a workaround |
| IFrame Player API as a route | **Blocked by policy** | Visible-viewport minimums + no-overlay/no-obscure rules in Required Minimum Functionality, plus III.I.9 (VERIFIED, **NEW**) |
| YouTube podcast hosting as a route | **Changes nothing** | Podcast episodes are ordinary YouTube videos in a Studio container; identical clauses apply (VERIFIED, **NEW**) |

Characterisation: **both**. The two failures are independent — even if YouTube shipped a playback
SDK tomorrow, III.I.7 and III.I.9 would still bar audio-only and background use; and even if those
clauses were relaxed, there would be no API to call. Neither branch is a workaround for the other.

**Currency of the clauses.** VERIFIED, NEW — the YouTube API Services Developer Policies page states
"Last updated 2026-06-24 UTC" and the API Services Terms of Service page states "Last updated
2026-04-28 UTC". All four clauses prior work relied on (III.E.1(a), III.I.7, III.I.9, III.I.14) are
present and unchanged in the current text. Nothing has loosened.

