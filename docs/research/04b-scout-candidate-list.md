# 04b scout pass — candidate music sources beyond the big four

Scout research for ticket `issues/04b-wider-platform-sweep.md`. This is a **candidate list**, not a
feasibility verdict — the depth work is the ticket's job. Verified against primary sources unless
marked otherwise.

## 1. Licensed / royalty-free catalogues and B2B APIs — highest value

| Name | API/SDK | Third-party consumer radio player allowed? | Pricing |
|---|---|---|---|
| **Feed.fm** | Yes, "turnkey SDKs" + Feed Clips music API — [feed.fm](https://feed.fm/) | **YES — purpose-built for this.** Pre-cleared catalogues in three tiers (major label, indie, royalty-free), "automates all compliance and reporting while granting you full indemnification." Products: Feed Radio, Feed Clips, Feed Originals | Quote-only. `/pricing` and `/music-pricing` both 404; SDK platform list not on public pages |
| **Tuned Global** | Yes — 500+ streaming APIs, white-label apps, **AutomixIQ SDK** ("seamless song transitions, akin to a DJ mix"), Social Radio SDK — [tunedglobal.com](https://www.tunedglobal.com/) | **YES.** Licensing consultancy + **sub-licensing**; UMG/Sony/Warner/Believe/Orchard/CD Baby as partners. 150M+ tracks. SOC 2 + ISO 27001, 40 businesses / 70 countries | Quote-only, no pricing page |
| **7digital** (now **MassiveMusic**) | Yes, REST — [docs.massivemusic.com](https://docs.massivemusic.com/), endpoints still `api.7digital.com/1.2/` | Named use cases include "**Interactive radio streaming services**", subscription streaming, background music. But you become the licensee / royalty payer | Not published |
| **Mubert** (AI-generated) | Yes — [mubert.com/api](https://mubert.com/api) | **YES.** Realtime WebRTC generation, sub-second latency, "continuous background layer"; sub-licensing is a paid-tier item | **PUBLIC:** $99/mo trial, $249/mo Startup (5k generations + 5k stream-minutes), $999/mo Startup+ |
| **Epidemic Sound** | Has API | **NO — verified.** Bars exploiting recordings "for the purpose of creating a music listening experience"; AUP bars standalone use "except as synchronized with a Production"; bars "music beds" and sublicensing | Consumer subs only |
| **Artlist** | No public API found | **NO — verified.** Assets "may only be used as integrated elements within broader Projects", not "exploited as standalone content"; excluded uses name "music streaming, music channels, radio"; licence "is only yours" | — |
| **Jamendo** | Yes, free API | Free API is **non-commercial only** (ToU 3.3 — ad revenue counts). Bars caching and "replicating the overall experience of Jamendo services"; mandatory attribution + backlink. Commercial licence quote-based; no evidence of a B2B2C tier | In-store radio €89/yr, but that covers Jamendo's own apps. 283k tracks |
| **Pixabay Music** | Yes | **NO.** "You cannot sell or distribute Content… on a Standalone basis" — standalone = no creative effort applied | Free |
| **Free Music Archive** | **API shut down** — [app-developers](https://freemusicarchive.org/app-developers) | Hotlinking and scraping both banned; you must self-host and honour per-track CC terms | Free |
| **Audius** | Yes — docs.audius.co (docs render client-side, thin) | **Apparently permitted:** content "made available to third-party services via the Audius API"; artists grant a "sublicensable" royalty-free licence. But per-artist opt-outs, CC BY-NC-ND variants to filter, licence "revocable", session-only caching | Free |
| Songtradr, Soundstripe, Uppbeat, Melodie | NO EVIDENCE FOUND — all bot-blocked (403/404/429) | Unverified | Unverified |

## 2. Subscription streaming services

| Service | API/SDK | Playback + library/history | Blocker |
|---|---|---|---|
| **Deezer** | Yes, LIVE — [developers.deezer.com](https://developers.deezer.com) | Yes: JS + iOS/Android SDKs listed as current; `/user/me/history` returns 200 | REST = 30s extracts only; caching "strictly forbidden"; docs login-walled; SDK-deprecation rumour neither confirmed nor refuted |
| **Audiomack** | Yes — [audiomack.com/data-api/docs](https://audiomack.com/data-api/docs) | `POST /music/:id/play` → 10s-TTL stream URL; favourites, playlists. **No history endpoint** | Hip-hop/Afrobeats skew; docs look stale |
| **SoundCloud** | Yes | Custom player possible; no listening-history endpoint | API key needs Artist Pro; heavy geo/paywall blocking |
| **Mixcloud** | Yes | **History yes** (`/me/listens`); playback **no** | "Audio streams are not available through the API" |
| **Napster / Rhapsody** | **DEAD** — `api.napster.com` has no DNS; `developers.napster.com` is now an unrelated AI company | — | Decommissioned |
| **Qobuz** | Partner-only, `app_id` issued privately | Unknown | Closed |
| Amazon Music, Pandora, Yandex, Anghami, JioSaavn, Gaana | None / no DNS | No | No developer programme |
| Boomplay, KKBOX | Portals return 200, content unfetchable | Unverified | Region-locked |
| Bandcamp | Yes, but sales/merch only | No playback at all | — |

## 3. Self-hosted and local library

| Name | API | Library + play counts + favourites |
|---|---|---|
| **Subsonic / OpenSubsonic** — [spec](https://opensubsonic.netlify.app/docs/) | Open spec | **Best bet.** `stream` + `playCount`, `starred`, `userRating`, `played`. One client reaches Navidrome, Gonic, Airsonic, Ampache, Funkwhale, Nextcloud, Jellyfin |
| **Jellyfin** — [api.jellyfin.org](https://api.jellyfin.org/) | Full REST + SDKs | `UserData` exposes PlayCount, IsFavorite, LastPlayedDate. Richest single integration |
| **Ampache**, Navidrome, Emby, Funkwhale | Yes | Equivalent; Navidrome logs plays only via `scrobble`, not `stream` |
| **Plex** | **No official API**; python-plexapi is reverse-engineered | `viewCount` / `lastViewedAt` work in practice. Large user base, fragile integration |
| **iOS `MPMediaItem`** | First-party | `playCount`, `lastPlayedDate`, `skipCount` readable — but **`assetURL` is nil for DRM/cloud items**, so no Apple Music audio; streaming history absent |
| **Android `MediaStore`** | First-party | **No PLAY_COUNT, no LAST_PLAYED, no RATING.** Only `IS_FAVORITE` |

All require self-hosting — **no cold start**.

## 4. Taste layer, podcast metadata, and radio directories

| Name | API | Provides |
|---|---|---|
| **ListenBrainz** — [docs](https://listenbrainz.readthedocs.io/en/latest/users/api/) | Yes | **History + playlists + recommendations, open data.** Best taste-modelling layer. No audio |
| **Last.fm** — [last.fm/api](https://www.last.fm/api) | Yes | History, scrobbling, similar-artist. Commercial use needs approval |
| **Taddy** — [taddy.org/developers](https://taddy.org/developers) | Yes | Podcast metadata + **transcripts** (useful for DJ-intro generation). Free → $150/mo |
| **Podcast Index** | Yes, free | Open podcast index + enclosure URLs |
| **Radio Browser** — [api.radio-browser.info](https://api.radio-browser.info/) | Free, keyless | Best station-metadata source |
| **Icecast / Liquidsoap** | Self-host | **Inverted architecture:** you become the broadcaster and splice music + podcast + TTS + DJ into your own stream. Solves interleaving; licensing burden is entirely yours |
| TuneIn, iHeart, Radio Garden, SomaFM, NTS, vTuner, Audacy, Radio Paradise | Partner-only or none | **All linear** — cannot select a track or splice in a DJ intro. **Disqualified as a music layer** |
| NPR, BBC Sounds, Reuters / AP | Dead domains / geo-locked / enterprise sales | News audio all gated |

## Key notes

- **The three real B2B fits are Feed.fm, Tuned Global, and 7digital/MassiveMusic** — all three
  explicitly sell "build your own player" rights. Feed.fm is the only one bundling rights + SDK +
  indemnification in a single product. **None publishes pricing.**
- **Epidemic Sound and Artlist are hard dead ends**, confirmed against actual licence text rather
  than inferred — both prohibit standalone music-listening experiences by name. This **sync-only
  licensing pattern rules out most of the royalty-free tier**.
- **Linearity wall:** every radio aggregator returns a stream someone else programmes. Useless as
  the music layer for this concept.
- **Deezer is the surprise** on the subscription side — portal live, history endpoint responding,
  contrary to the assumption that it was dead.
- **Unverified / bot-blocked, needs a browser check:** Uppbeat, Melodie, Soundstripe, Songtradr,
  Qobuz, Boomplay, KKBOX, NPR.
