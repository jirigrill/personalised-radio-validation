# 05b — Indie-friendly licensed-music API sweep

**Question**: Is there any music-licensing API, other than Feed.fm/Adaptr, that (1) a solo/indie
developer can sign up for self-serve or at small monthly cost, (2) supplies music with recognisable
artists, and (3) permits distribution inside a paid consumer app with radio-style shuffle interleaved
with podcasts?

**Context**: solo Czech developer, US market, paid consumer iOS app. Supplement to
`04b-wider-platform-sweep.md` and `05a-feedfm-market-check.md`.

**Method caveat**: `WebSearch` returned HTTP 400 on every call. All evidence via `WebFetch` of
primary pages and the HN Algolia API. Several sites blocked automated fetching (noted per row).

## Conclusion

**No "Stripe for mainstream music" exists for indie developers.** Nothing changed for services
already covered in 04b. Closest thing remains **Feed.fm / Adaptr** (sales-gated, see 05a).

The **only self-serve legal path found** is **Live365** — Broadcast 1 plan $65/mo, US/CA/MX, bundles
ASCAP/BMI/SESAC/GMR/SoundExchange. But it is **one linear, non-interactive station you programme
yourself**; per-user shuffle plus podcast interleaving breaks the licence model. Whether you may
embed your own Live365 station in your own paid app is **NOT verifiable** (terms page is JS-only).

Interleaving with other audio and DJ pre-announcement: **no service addresses it permissively**;
the only mentions are prohibitions (SoundCloud, Musicbed, DI.fm).

## Sweep

| Candidate | Verdict | Evidence |
|---|---|---|
| **SoundCloud API** | BLOCKED (VERIFIED) | API ToS forbids "webcasting radio app", in-app purchase for content, aggregating "with content from other services"; Artist Pro subscription required. <https://developers.soundcloud.com/docs/api/terms-of-use> |
| **Pandora developer** | BLOCKED (VERIFIED) | Prohibits "Charge Pandora Listeners a fee"; no app that "replicates the core Pandora user experience"; US-only; certification required. <https://developer.pandora.com/terms-and-conditions> |
| **Napster API** | UNVERIFIABLE | Developer portal down; napster.com/terms Annex A now describes an AI-agent API, not music playback |
| **Audius** | Free API, wrong catalogue | "Permissionless" indie/web3 catalogue; terms endpoint returned 500 |
| **Bandcamp** | Eliminated | No playback API |
| **Amazon Music** | Eliminated | Alexa-inbound skills only; no outbound playback API |
| **Qobuz** | Partner-only | No public ToS or signup |
| **Audiomack** | Unclear | Docs exist; no signup path or ToS found |
| **iHeart / Anghami / Boomplay developer** | NO EVIDENCE FOUND | 404 / DNS failures |
| **Lickd** | BLOCKED (VERIFIED) | Forbids "'radio' style content" and streaming; sync licence only. <https://lickd.co/music-license> |
| **Musicbed** | BLOCKED (VERIFIED) | Excludes "listening and discovery" platforms; "may not... combine" |
| **Track Club / Marmoset** | Wrong scope | $12–58/mo library music; "Web, social, podcast use only" |
| **Cyanite / Musiio / Musixmatch** | Not licensors | Tagging / lyrics services. Musical.ai dead |
| **Uppbeat / Slip.stream / Pretzel / Songtradr** | Fetch-blocked | INFERRED creator/sync licences, not consumer-app distribution |
| **MediaNet / Beat Fund** | UNVERIFIABLE | Site down; HN hearsay (2018) of ~$1k/mo minimum. The only wholesale candidate; needs direct outreach |
| **Rightsify / Mixtape** | Eliminated | AI-generated catalogue; Mixtape domain dead |
| **CloudCover ($18.95/loc/mo), Rockbot, Soundtrack Your Brand, SiriusXM for Business ($26.95/mo), Vibenomics** | Business-premises only (VERIFIED) | Licences cover physical locations; APIs control own zones only |
| **DI.fm** | BLOCKED | ToS bans third-party use |
| **Radio.co ($35–199/mo), Shoutcast** | No licence included | Hosting only; you bring your own rights |
| **Live365** | Closest self-serve | $65/mo Broadcast 1, US/CA/MX, licences bundled; single linear station; own-app embedding unverifiable |
| **Radio Browser API** | No rights | Free directory API usable in paid apps; grants NO stream rights |
| **TuneIn** | Device-partner only | Legacy OPML undocumented |
| **iHeart / Cumulus streams** | BLOCKED (VERIFIED) | Non-commercial personal use; no retransmission. Audacy returned 403 |
| **NPR** | BLOCKED | "Personal noncommercial use" only; audio must stream from NPR servers |
| **Voice of America** | PARTIAL | VOA-produced content is public domain (17 U.S.C. §105); embedded music and agency material is not; 2025–26 operational status NO EVIDENCE FOUND |
| **§114 relay of a licensed station** | INFERRED blocked | SoundExchange licence attaches to the transmitting service; proxying or mixing means you need your own licence. Counsel needed |

## Implications for ticket 05

- Confirms 04b: recognisable-artist music is available to apps only through sales-gated B2B
  contracts (Feed.fm/Adaptr, possibly MediaNet). Self-serve exists only for library/production music
  or a single linear webcast.
- Relaying existing licensed radio (NPR, iHeart, commercial streams) into the app's mix is not a
  route: every stream checked forbids retransmission or commercial use.
- Free news audio: only VOA-produced content is clean on copyright, with an unverified operational
  status. Feeds into ticket 06.
