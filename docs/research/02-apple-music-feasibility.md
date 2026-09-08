# 02 — Apple Music / MusicKit feasibility

Research resolving `issues/02-apple-music-feasibility.md`. All findings below are **new ground** —
prior work in `podcasts-notes-validation` covered Apple *Podcasts* only. Nothing here is carried over
except the App Store Guideline 5.2.3 reference, which was already known.

Tagging: **VERIFIED** = read in a primary source, URL given. **SECONDARY** = Apple Developer Forums,
StackOverflow, retired Apple sample code, or press. **INFERRED** = reasoning from primary sources.
**NO EVIDENCE FOUND** = searched, nothing located.

---

## Verdict up front

**BLOCKED — on contract, not on code.**

The technical mechanic mostly works. The licence forbids it. Two clauses in the Apple Developer
Program License Agreement §3.3.6(D) are each independently fatal to the concept as specified in
`map.md`, and neither has a workaround that stays inside the terms.

| # | Question | Verdict | Decisive finding |
|---|---|---|---|
| 1 | Playback control | **FEASIBLE** | `ApplicationMusicPlayer` gives app-owned queue, `pause()`, settable `playbackTime`. Catalog playback requires an active Apple Music subscription. |
| 2 | Interleaving / one `AVAudioSession` | **FEASIBLE technically** | `.playback` + `.mixWithOthers` lets app audio and MusicKit audio coexist. Pause → play own audio → resume works. MusicKit does *not* take exclusive control if configured correctly. |
| 3 | Crossfade / mixing / ducking | **BLOCKED** | Zero volume or gain API on `ApplicationMusicPlayer`. No decoded audio, no tap, no `AVAudioEngine` seam. `PlayParameters` is documented "opaque". Hard cut is the ceiling. |
| 4 | Reading user data | **FEASIBLE for music, BLOCKED for podcasts** | Library, ratings, heavy rotation, recommendations all readable. No listening-history endpoint — recent plays cap at 10 resources / 30 tracks. **No Apple API exposes user podcast subscriptions or history at all.** |
| 5 | The terms | **BLOCKED — fatal** | DPLA §3.3.6(D): "MusicKit Content cannot be synchronized with any other content" and use must not be "for purposes unrelated to facilitating access to Your end users' Apple Music subscriptions". Monetisation of Apple Music access separately barred. |
| 6 | Apple Music Radio / competitive threat | **PARTIAL / LOW near-term** | Stations are playable but opaque. Apple ships AutoMix (beat-matched) to itself and only duration-only crossfade to third parties. Apple has announced no AI DJ; Spotify has had one since Feb 2023. |

### The two clauses that end it

Both **VERIFIED**, both from the Apple Developer Program License Agreement, August 18 2026 edition,
§3.3.6(D) "MusicKit", PDF page 32 —
https://developer.apple.com/support/downloads/terms/apple-developer-program/Apple-Developer-Program-License-Agreement-English.pdf

> "You may not, and You may not permit Your end users to, download, upload, or modify any
> MusicKit Content and MusicKit Content cannot be synchronized with any other content, unless
> otherwise permitted by Apple in the Documentation;"

> "You agree not to call the MusicKit APIs or use MusicKit JS (or otherwise attempt to gain
> information through the MusicKit APIs or MusicKit JS) for purposes unrelated to facilitating
> access to Your end users' Apple Music subscriptions."

Programming Apple Music tracks into a timeline alongside podcast episodes, AI-generated spoken
intros and news segments *is* synchronising MusicKit Content with other content. And an app whose
purpose is "a personalised radio show" is not straightforwardly "facilitating access to Your end
users' Apple Music subscriptions" — Apple decides that, and §3.3.6(D)'s neighbouring clause reserves
determinations of relevance to "Apple's sole discretion".

**A workaround that violates a clause is not a workaround.** The sequential-handoff pattern that
solves the audio problem does not solve the licence problem: interleaving is precisely what the
no-synchronization clause names.

### What is NOT the blocker

Worth stating plainly, because it would be easy to reach the right verdict for the wrong reason:

- **The audio session is not the blocker.** The make-or-break technical question in the ticket
  resolves in the concept's favour. See §2.
- **There is no "radio station" or "broadcast" prohibition.** NO EVIDENCE FOUND for any clause in
  the DPLA, App Store Review Guidelines, or Apple Media Services Terms using the words "radio
  station", "broadcast", "rebroadcast", or "public performance" as a restriction on MusicKit apps.
  The prohibition arrives through synchronization and purpose-limitation, not through a
  broadcast-impersonation rule. Do not cite a broadcast clause; it does not exist.
- **There is no separate "Apple Music API terms" document.** NO EVIDENCE FOUND. `developer.apple.com/terms/`
  enumerates every developer agreement and none mentions Apple Music, MusicKit, or Media Services.
  The DPLA is the operative contract. Any prior analysis assuming a standalone MusicKit ToS was wrong.

---

## 1. Playback control — FEASIBLE

### ApplicationMusicPlayer vs SystemMusicPlayer

**VERIFIED** — https://developer.apple.com/documentation/musickit/applicationmusicplayer

`ApplicationMusicPlayer` is the one to use. It owns its own queue, independent of the Music app.
`SystemMusicPlayer` shares state with the Music app — so using it would mutate the user's real
Now Playing and queue, which is wrong for this product and also unavailable on macOS.

Complete member list of `ApplicationMusicPlayer` (**VERIFIED** by enumerating the DocC symbol tree):
`Queue` (class), `queue`, `transition`, `shared`. Nothing else. It inherits the rest from `MusicPlayer`.

Complete member list of `MusicPlayer` (**VERIFIED**): `Queue`, `State`, `isPreparedToPlay`,
`playbackTime`, `state`, `beginSeekingBackward()`, `beginSeekingForward()`, `endSeeking()`,
`pause()`, `play()`, `prepareToPlay()`, `restartCurrentEntry()`, `skipToNextEntry()`,
`skipToPreviousEntry()`, `stop()`, `PlaybackStatus`, `RepeatMode`, `ShuffleMode`, `Transition`.

Complete member list of `MusicPlayer.State` (**VERIFIED**): `audioVariant`, `playbackRate`,
`playbackStatus`, `repeatMode`, `shuffleMode`.

**Availability gotcha (VERIFIED):** `MusicPlayer` and every playback symbol are **macOS 14.0+**,
while the framework and its model/subscription layer are macOS 12.0+ — playback arrived on macOS two
major versions late. `SystemMusicPlayer` has **no macOS** at all. No playback symbol lists **watchOS**.

### Queue building, pause, seek, resume — all supported

- **App-owned queue: YES.** `ApplicationMusicPlayer.shared.queue` — **VERIFIED**,
  https://developer.apple.com/documentation/musickit/applicationmusicplayer/queue-swift.property
- **Insert next / append: YES.** `MusicPlayer.Queue.EntryInsertionPosition` with `.afterCurrentEntry`
  and `.tail` — **VERIFIED**, https://developer.apple.com/documentation/musickit/musicplayer/queue/entryinsertionposition
- **Arbitrary mid-queue insert/remove: YES** via mutating `queue.entries` — but the `Entries` type is
  listed as a member with an **empty abstract and no page of its own** (404). Functionally present,
  documentationally unsupported. Tag: VERIFIED-present, undocumented-behaviour.
- **Pause mid-track: YES.** `pause()` — **VERIFIED**, https://developer.apple.com/documentation/musickit/musicplayer/pause()
- **Resume at pause point: YES.** `play()` — **VERIFIED**, https://developer.apple.com/documentation/musickit/musicplayer/play()
- **Seek to arbitrary position: YES.** `playbackTime` is `{ get set }` — **VERIFIED**,
  https://developer.apple.com/documentation/musickit/musicplayer/playbacktime
- **Per-entry start/end trim: YES at construction only.** `MusicPlayer.Queue.Entry.init(_:startTime:endTime:)`;
  the properties are get-only afterwards — **VERIFIED**,
  https://developer.apple.com/documentation/musickit/musicplayer/queue/entry/init(_:starttime:endtime:)
- **Playback rate: YES**, settable `Float`, supports reverse — **VERIFIED**,
  https://developer.apple.com/documentation/musickit/musicplayer/state-swift.class/playbackrate

So the raw programming primitives the concept needs are present. This is a genuinely better position
than the Spotify iOS SDK (remote-control only, per carried-over prior work).

### Subscription requirement

**VERIFIED**, WWDC session, verbatim: catalog playback requires an active subscription; content in the
user's own library or purchased works without one.

`MusicSubscription.canPlayCatalogContent` — **VERIFIED**,
https://developer.apple.com/documentation/musickit/musicsubscription/canplaycatalogcontent — the
documented pattern is to **pre-check and disable UI**, not to catch an error.

**NO EVIDENCE FOUND** for the specific error thrown when a non-subscriber attempts catalog playback.
Apple documents the pre-check pattern and does not document failure semantics.

`MusicSubscriptionOffer` and `.Options` exist to upsell — **VERIFIED** — but are
**iOS/iPadOS/Mac Catalyst 15.0 and macOS 12.0 only. No tvOS, visionOS, or watchOS**, so you cannot
present a subscription offer on those platforms even though `canBecomeSubscriber` is readable there.

**Preview playback for non-subscribers: NOT via a MusicKit player.** `PreviewAsset` /
`Song.previewAssets` expose preview URLs (**VERIFIED**, https://developer.apple.com/documentation/musickit/previewasset)
but you would play them yourself with AVFoundation. **NO EVIDENCE FOUND** that a MusicKit player will
play previews, and **NO EVIDENCE FOUND** that Apple sanctions a preview-only unauthenticated product —
a developer asked exactly this on the forums in Jul 2026 and got **zero replies**
(SECONDARY, https://developer.apple.com/forums/thread/836910).

Runtime gates beyond the subscription: `MusicAuthorization.request()` consent and the
`NSAppleMusicUsageDescription` Info.plist string.

**Cost floor (VERIFIED):** paid Apple Developer Program membership is required —
https://developer.apple.com/documentation/applemusicapi/generating-developer-tokens states you must
be "a trusted developer and **member of the Apple Developer Program**". Plus a MusicKit identifier and
ES256 private key, and each end user needs their own active Apple Music subscription.

---

## 2. The interleaving question — TECHNICALLY FEASIBLE

This was flagged as the make-or-break detail. **It resolves in the concept's favour.** MusicKit does
not take exclusive control of the audio session, and pause → play own audio → resume works.

### AVAudioSession is per-process and shared

**VERIFIED** — https://developer.apple.com/documentation/avfaudio/avaudiosession — the session is a
singleton for the process. Your app and `ApplicationMusicPlayer` are in the same process, so there is
one session to configure.

### The documented mixing mechanism

**VERIFIED** — `AVAudioSession.CategoryOptions.mixWithOthers`:

> "your app mixes its audio with audio playing in background apps, such as the Music app"

**VERIFIED** — https://developer.apple.com/documentation/musickit/musicplayer/prepareToPlay() —
abstract, verbatim:

> "Prepares the current queue for playback, **interrupting any active (nonmixable) audio sessions**."

The parenthetical is load-bearing and is the single best primary-source answer to the ticket's
question. MusicKit interrupts **nonmixable** sessions. Configure yours as mixable and it does not
interrupt you.

### Apple's own sanctioned pattern for exactly this

**SECONDARY but strong** — Apple's retired *AddMusic* sample code demonstrated mixing app audio with
music-player audio, with `applicationMusicPlayer` as its default player —
https://developer.apple.com/library/archive/samplecode/AddMusic/Listings/Classes_MainViewController_m.html

> ```objc
> // The AmbientSound category allows application audio to mix with Media Player
> // audio.
> [[AVAudioSession sharedInstance] setCategory: AVAudioSessionCategoryAmbient error: nil];
> ```

Its ReadMe: *"AddMusic also demonstrates how to mix application audio with iPod library audio."* It ran
an `AVAudioPlayer` concurrently with an `MPMusicPlayerController`. 2009-era and retired, so SECONDARY —
but it was Apple shipping this as the approved pattern.

**VERIFIED** — https://developer.apple.com/documentation/mediaplayer/mpmusicplayercontroller, on the
pre-MusicKit-Swift equivalent:

> "An application music player **plays music locally within your app.** It isn't aware of the Music
> app's Now Playing item, nor does it affect the Music app's state."

### Recommended configuration

Set `.playback` with `.mixWithOthers` **once at launch and never change it**; sequence Apple Music and
your own audio with explicit `pause()` / `play()` calls rather than relying on session state; on
finishing your own audio, deactivate with `.notifyOthersOnDeactivation`.

Changing the category mid-flight interrupts whichever player is currently running (SECONDARY,
StackOverflow 76907591) — so decide the configuration up front.

### What does NOT work

- **`.ambient`** mutes on the Ring/Silent switch — wrong for a commuting product (SECONDARY, SO 63138976).
- **`duckOthers` is not a fade.** **VERIFIED**: "Ducking begins when you activate your app's audio
  session and ends when you deactivate the session" and "Set this option on a temporary basis only.
  Don't use it to duck the audio of other apps for more than a few seconds." Binary step, not a ramp.

### Honest gap in the evidence

**NO EVIDENCE FOUND** for any Apple engineer or DTS statement on what session `ApplicationMusicPlayer`
uses internally, and MusicKit exposes **no `AVAudioSession` property and no session-configuration
point** (VERIFIED by exhaustive symbol sweep of the framework — zero matches for `session`,
`audio session`, `avaudio`). The only documented lever over its session behaviour is the
`UIBackgroundModes`/`audio` Info.plist entry:

**VERIFIED** — https://developer.apple.com/documentation/musickit/applicationmusicplayer — "If your
app includes a background audio mode in your `Info.plist` file, the application music player continues
playing the current music item when your app moves to the background."

Also note: Apple Developer Forums thread pages return Cloudflare 403 to automated fetching; forum
evidence here came from tag listing pages and StackOverflow. Forum coverage is therefore thinner than
ideal. This does not change the verdict, because §5 is dispositive regardless.

### Two practical caveats found in the wild (SECONDARY)

- Queues above roughly 150–300 entries trigger `Code=9 "Remote call timed out"` — page in ~100-item
  batches (https://stackoverflow.com/questions/78397898/).
- Library tracks played via `ApplicationMusicPlayer` **do not update last-played date or play count**
  in Music.app / iCloud Music Library (FB17675148). Relevant: the app would not feed the very signals
  it reads in §4.

---

## 3. Crossfading and mixing — BLOCKED

**A hard cut is the ceiling.** Three independent reasons, none of them soft.

### No volume or gain API exists

**VERIFIED by exhaustive enumeration** of every symbol in the MusicKit framework DocC index
(`developer.apple.com/tutorials/data/documentation/musickit.json`), filtering titles and abstracts for
`volume`, `gain`, `fade`, `mixer`, `tap`, `session`. **Zero occurrences of "volume", "gain", or "mute"**
anywhere in `musicplayer.json` or `applicationmusicplayer.json`. There is no per-entry level, no mixer,
no attenuation of any kind.

You cannot fade what you cannot attenuate.

The nearest thing is `playbackRate` — a speed multiplier, not gain.

### The sanctioned substitutes are deprecated or broken

- `MPMusicPlayerController.volume` — **deprecated since iOS 7** (VERIFIED from doc JSON
  `"deprecated": true`). Apple redirects to `MPVolumeView`, which is system-wide volume, not per-player,
  and is reported not to work with `ApplicationMusicPlayer` (SECONDARY, FB21042385).
- **VERIFIED**, iPod Library Access Guide: "Only one music player can play audio at a time." No two-deck
  pattern.

### No decoded audio, no tap, no AVAudioEngine seam

**NO EVIDENCE FOUND** for any API on `ApplicationMusicPlayer`, `MusicPlayer`, `MusicPlayer.Queue`,
`MusicPlayer.Queue.Entry`, `PlayParameters`, or `PlayableMusicItem` exposing decoded audio buffers, an
`MTAudioProcessingTap`, an `AVAudioNode`/`AVAudioEngine` connection point, an `AVPlayerItem`, or an
`AVAsset` for catalog content.

The surface is deliberately sealed. **VERIFIED** —
https://developer.apple.com/documentation/musickit/playparameters:

> `struct PlayParameters` — "**An opaque object** that represents parameters to initiate playback of a
> playable music item using a music player."

The page lists **no members at all**. And `PlayableMusicItem`'s sole requirement is
`var playParameters: PlayParameters? { get }`. You hand the player an opaque token and get back only
state properties. There is no seam for audio processing. This is DRM working as intended.

`AudioVariant` (`dolbyAtmos`, `lossless`, etc.) reports format only — read-only, no processing hook.

### The one crossfade that exists is queue-internal only

**VERIFIED** — `ApplicationMusicPlayer.transition` / `MusicPlayer.Transition.crossfade(duration:)`,
**iOS 18.0 and iPadOS 18.0 only** — not macOS, Mac Catalyst, tvOS, visionOS, or watchOS.
https://developer.apple.com/documentation/musickit/musicplayer/transition

The entire surface is: on/off plus one `TimeInterval`. No curve shape, no per-side control, no
fade-in/fade-out separation, no mix point.

Documented constraints, **VERIFIED** —
https://developer.apple.com/documentation/musickit/applicationmusicplayer/transition:

> "By default, the `transition` is `.none` where there is no transition between playing items."
> "Your application should set the desired transition before setting the queue."
> "The player cannot apply transitions in all scenarios. For example, the player does not apply a
> transition between two consecutive tracks in an album."

Critically: this crossfades Apple Music track → Apple Music track **inside MusicKit's own queue**. It
cannot crossfade Apple Music → your podcast MP3, **because your MP3 cannot be a MusicKit queue entry.**

### The asymmetry worth naming

Apple ships itself **AutoMix** — beat-matched, time-stretched DJ transitions — and gives third parties
duration-only crossfade. **VERIFIED**,
https://www.apple.com/newsroom/2025/06/apple-services-deliver-powerful-features-and-intelligent-updates-to-users-this-fall/
(June 10, 2025):

> "AutoMix, which mixes one song into the next, just like a DJ… Using AI to analyze audio features, it
> crafts unique transitions between songs with time stretching and beat matching."

AutoMix is Music-app-only. It is **not** in MusicKit. Do not plan on parity with the Music app's polish.

### SECONDARY corroboration

A developer asked precisely this on the forums —
https://developer.apple.com/forums/thread/843627, "Apple Music real-time DJ mixing: is there an API or
entitlement beyond MusicKit?" — **zero replies, no Apple response**, and reached the same conclusion
independently:

> "ApplicationMusicPlayer manages the playback queue and crossfade internally. I cannot independently
> control the outgoing and incoming Apple Music tracks as two decks… I understand that Apple Music
> content is protected and that direct access to decoded audio samples may intentionally not be
> available through the public MusicKit APIs."

Unanswered user post, not an Apple statement — corroborating, not authoritative.

### Consequence for the product

The join between a podcast segment and a song is a **hard cut with a perceptible gap**. `duckOthers`
gives a binary volume step under a voice cue, not a ramp, and only for a few seconds by Apple's own
instruction. If radio-like production polish is part of the value proposition, that is a product risk,
not an implementation detail. It would have been the thing to prototype first — were §5 not fatal anyway.

---

## 4. Reading user data — FEASIBLE for music, BLOCKED for podcasts

### What is readable (all VERIFIED)

| Signal | Endpoint / API | Note |
|---|---|---|
| Library songs/albums/playlists | `/v1/me/library/*`; MusicKit `MusicLibraryRequest`, `MusicLibrarySectionedRequest` | Filterable and sortable |
| Recently played | `/v1/me/recent/played`, `/v1/me/recent/played/tracks`; `MusicRecentlyPlayedRequest` | **Severely capped — see below** |
| Heavy rotation | `/v1/me/history/heavy-rotation` | Most-played content |
| Ratings (like/dislike) | `/v1/me/ratings/...` | Readable per-ID |
| Personal recommendations | `/v1/me/recommendations`; `MusicPersonalRecommendationsRequest` | |
| Replay | Yearly summaries | |

All `/v1/me/*` calls require both an ES256-signed **developer token** and a **`Music-User-Token`**.
**VERIFIED** — https://developer.apple.com/documentation/applemusicapi/user-authentication-for-musickit:

> "Apple Music API requires the inclusion of a Music User Token for **any requests for data specific to
> an Apple Music subscriber**, such as to fetch content from the user's library."

**Platform trap (VERIFIED, same page):** "Automatic Music User Token management is not available for
Android." A server-side or Android target must obtain the token client-side on an Apple platform or via
MusicKit JS and relay it.

### The weakest link: there is no listening history

**NO EVIDENCE FOUND** for any full listening-history endpoint. Recent plays are hard-capped:
**10 resources / 30 tracks per request**, with **no documented lookback window**.

This matters more than it first appears. The concept in `map.md` step 2 reads "listening history" as a
personalisation input. You cannot reconstruct one from Apple. You would have to poll and accumulate your
own longitudinal record from day one — which means a cold-start problem for every new user, and a server
cost the unit-economics ticket has to carry. Compounding it: §2 found that tracks played through
`ApplicationMusicPlayer` **do not update Apple's own play counts** (SECONDARY, FB17675148), so your app's
own listening does not feed back into the signals you read.

### Favorites are write-only

Read "loved" state via `/v1/me/ratings/...`, **not** `/v1/me/favorites`. Easy trap.

### Podcasts: a hard no

**NO EVIDENCE FOUND**, after sweeping the full DocC payloads of both frameworks and the entire
technologies index. Specifically:

- The `musickit` symbol tree contains **zero podcast symbols**. Music Items are Album, Artist, Curator,
  Genre, MusicVideo, Playlist, RadioShow, RecordLabel, Song, Station, Track — **no Podcast, no Episode,
  no Show** (VERIFIED, https://developer.apple.com/documentation/musickit).
- The single occurrence of "podcast" on each framework's top-level page is incidental boilerplate in a
  cross-link to MediaPlayer. Not an endpoint.
- No `developer.apple.com/documentation/podcasts`, `/applepodcasts`, or `/podcastsconnect` exists — all
  404 on the DocC endpoint.

The three things that do exist, and must not be confused with user data:

| Surface | Scope | User-specific? |
|---|---|---|
| iTunes Search API (`itunes.apple.com/search?media=podcast`) | **Public catalog only** — titles, feed URLs, artwork, genre. Documented under Affiliate/Performance Partners, *not* developer.apple.com | **No.** No auth, no user identity — structurally incapable of returning user data |
| MediaPlayer `MPMediaQuery.podcasts()` | On-device library query | Barely — see quote |
| Podcasts Connect | Publisher analytics for shows **you own** | No — aggregate, own-content only |

The MediaPlayer caveat is decisive. **VERIFIED** —
https://developer.apple.com/documentation/mediaplayer/mpmediaquery/podcasts():

> "This query only returns podcasts **downloaded to the user's library**."

So even that sees only downloaded episode *files* — not the subscription/follow list, not Apple Podcasts
play history, not progress. Native iOS only, no REST path.

**Conclusion: no Apple API exposes user podcast subscriptions, follows, or listening history.** This
independently breaks `map.md` step 2 ("followed/liked podcasts") on the Apple platform. The only route
would be RSS/OPML import — i.e. asking the user to hand over their subscription list manually — which is
a materially worse onboarding story and should be priced as such if any variant survives.

---

## 5. The terms — BLOCKED, fatal

### The operative contract

There is **no separate Apple Music API or MusicKit terms of service**. **NO EVIDENCE FOUND** —
`developer.apple.com/terms/` enumerates every developer agreement and none mentions Apple Music,
MusicKit, or Media Services. `apple.com/legal/internet-services/terms/site.html` is website terms of use
only and contains no Apple Music content.

The binding document is the **Apple Developer Program License Agreement**, August 18 2026 edition,
**§3.3.6(D) "MusicKit"**, PDF page 32 —
https://developer.apple.com/support/downloads/terms/apple-developer-program/Apple-Developer-Program-License-Agreement-English.pdf

### §3.3.6(D) in full, verbatim (VERIFIED)

> **D. MusicKit**
>
> You agree not to call the MusicKit APIs or use MusicKit JS (or otherwise attempt to gain information
> through the MusicKit APIs or MusicKit JS) for purposes unrelated to facilitating access to Your end
> users' Apple Music subscriptions. If You access the MusicKit APIs or MusicKit JS, then You must follow
> the Apple Music Identity Guidelines. You agree not to require payment for or indirectly monetize access
> to the Apple Music service (e.g. in-app purchase, advertising, requesting user info) through Your use
> of the MusicKit APIs, MusicKit JS, or otherwise in any way. In addition:
>
> - If You choose to offer music playback through the MusicKit APIs or MusicKit JS, full songs must be
>   enabled for playback, and users must initiate playback and be able to navigate playback using
>   standard media controls such as "play," "pause," and "skip", and You agree to not misrepresent the
>   functionality of these controls;
>
> - You may not, and You may not permit Your end users to, download, upload, or modify any MusicKit
>   Content and MusicKit Content cannot be synchronized with any other content, unless otherwise
>   permitted by Apple in the Documentation;
>
> - You may play MusicKit Content only as rendered by the MusicKit APIs or MusicKit JS and only as
>   permitted in the Documentation (e.g., album art and music-related text from the MusicKit API may not
>   be used separately from music playback or managing playlists);
>
> - Metadata from users (such as playlists and favorites) may be used only to provide a service or
>   function that is clearly disclosed to end users and that is directly relevant to the use of Your
>   Application, website, or web application, as determined in Apple's sole discretion; and
>
> - You may use MusicKit JS only as a stand-alone library in Your Application, website, or web
>   application and only as permitted in the Documentation (e.g., You agree not to recombine MusicKit JS
>   with any other JavaScript code or separately download and re-host it).

Definitions, **VERIFIED**, DPLA page 9:

> "**MusicKit APIs**" means the set of APIs that enable Apple Music users to access their subscription
> through Your Application or as otherwise permitted by Apple in the Documentation.

> "**MusicKit Content**" means music, video, and/or graphical content rendered through the MusicKit APIs.

### Clause-by-clause against the concept

| Clause | Concept element it hits | Severity |
|---|---|---|
| "MusicKit Content cannot be synchronized with any other content" | The entire interleaving mechanic — podcast episodes, AI intros/outros, news segments programmed against songs | **FATAL** |
| "for purposes unrelated to facilitating access to Your end users' Apple Music subscriptions" | The app's purpose is a personalised radio show, not Apple Music access | **FATAL** |
| "You agree not to require payment for or indirectly monetize access to the Apple Music service (e.g. in-app purchase, advertising…)" | The subscription revenue the $1–3k MRR bar depends on | **FATAL to the business model** |
| "users must initiate playback and be able to navigate playback using standard media controls… not misrepresent the functionality of these controls" | A continuously auto-advancing station with repurposed skip semantics | **HIGH RISK** |
| "You may play MusicKit Content only as rendered by the MusicKit APIs" | Any attempt to route audio through your own mixer | Reinforces §3 |
| "Metadata from users… as determined in Apple's sole discretion" | Using library/ratings to programme the station | Discretionary — Apple decides |

The **monetisation clause deserves separate weight** and is easy to miss. Even a variant that somehow
satisfied the synchronization clause still cannot charge for the product if Apple Music playback is part
of its value proposition. **App Store Review Guideline 4.10** reinforces this independently — you may not
monetise built-in capabilities such as Apple Music access
(https://developer.apple.com/app-store/review/guidelines/). Since `map.md` sets $1k/mo MRR as the floor,
and this is a paid-subscription product, **the Apple-Music-native variant has no legal revenue model even
if every technical problem were solved.** That is arguably the more interesting finding than the
synchronization clause, because it survives any amount of engineering.

### Attribution requirements (VERIFIED)

**Apple Music Identity Guidelines** compliance is mandatory per §3.3.6(D) —
https://marketing.services.apple/apple-music-identity-guidelines — badge usage, attribution, and
credit-line rules. Not a blocker, but a non-trivial design constraint: the product cannot present itself
as a neutral radio station; it must wear Apple Music branding.

### What the terms do NOT say

- **No "radio station", "broadcast", "rebroadcast", or "public performance" prohibition.**
  **NO EVIDENCE FOUND** in the DPLA, the App Store Review Guidelines, or the Apple Media Services Terms.
  Do not cite one — it does not exist. The prohibition arrives entirely through synchronization,
  purpose-limitation, and monetisation.
- Also relevant, carried over from prior work: **App Store Review Guideline 5.2.3**, plus 5.2.2, 5.2.5,
  4.5.1, 4.5.2, 2.3.6, 4.2.2 — and Apple Media Services Terms §F, §H, §Q, §T on personal
  non-commercial use — https://www.apple.com/legal/internet-services/itunes/us/terms.html

### The one unexplored escape hatch

§3.3.6(D) defers to "the Documentation" three times — "unless otherwise permitted by Apple in the
Documentation". If a permission for synchronization exists anywhere, it is there. **NO EVIDENCE FOUND**
that it does; the MusicKit documentation surveyed for §1–§3 contains no such permission, and §3 found the
audio surface deliberately sealed, which is the opposite of a carve-out. Treat this as closed unless
someone finds the clause.

There is no entitlement-request path documented for MusicKit comparable to the Single Sign-On API or
TV App API entitlements in §3.3.6(B) and §3.3.6(C) — those sections explicitly describe applying to
Apple for an entitlement; §3.3.6(D) does not. **INFERRED** from the structural contrast: Apple did not
build a negotiation door for MusicKit.

---

## 6. Apple Music Radio / stations — PARTIAL; competitive threat LOW near-term

### What Apple ships

Three human-hosted live stations — **Apple Music 1** (formerly Beats 1), **Apple Music Hits**,
**Apple Music Country** — plus algorithmic artist and "Made for You" personal stations. Hosts are human
(Zane Lowe, Becky G, Honey Dijon). Note that Apple moved its *spoken* shows **out** of Music and into
Podcasts in June 2023 — a signal about how Apple itself separates the two, and about how it would view an
app that recombines them.

### API surface: playable but opaque

`Station` is a first-class `PlayableMusicItem` (**VERIFIED**) — you can search the catalog for stations,
fetch `/v1/me/recent/radio-stations`, and hand a station to `ApplicationMusicPlayer`.

But you get **zero visibility into or control over what a station plays**. **NO EVIDENCE FOUND** for any
API to inspect or manipulate a station's forthcoming queue. Apple's own user documentation states that on
live radio users "can't rewind or skip songs."

So: you can *play* Apple's stations, you cannot *programme against* them. Interleaving your own content
into a station is doubly impossible — mechanically (opaque stream) and contractually (§5).

`Queue.Entry.Item` admits only `song` and `musicVideo` (**VERIFIED**) — reinforcing §3: there is no
container in the MusicKit queue for your own audio, whatever its source.

### Does Apple already do what the concept proposes?

No. Apple's AI investment in Music is entirely in **transitions and lyrics**, not generative spoken
hosting:

- **June 10, 2025** (iOS 26): AutoMix, Lyrics Translation, Lyrics Pronunciation —
  https://www.apple.com/newsroom/2025/06/apple-services-deliver-powerful-features-and-intelligent-updates-to-users-this-fall/
- **June 9, 2026**: more lyrics pairings, AutoMix improvements, Hi-Res Lossless on tvOS; AutoMix extended
  to tvOS and HomePod — https://www.apple.com/newsroom/2026/06/apple-unveils-innovative-features-and-intelligence-experiences-across-services/
  Explicitly **no radio, station, DJ, or spoken-word feature.**
- **May 2025**: Sound Therapy with UMG — functional audio, not DJ.

**NO EVIDENCE FOUND** for any Apple AI-DJ or personalised-radio announcement, after sweeping the Apple
Music newsroom archive Oct 2021 → Feb 2026 plus both WWDC services releases.

### The real incumbent is Spotify, not Apple

**VERIFIED** — Spotify AI DJ has a three-and-a-half-year head start:

| Date | Event |
|---|---|
| Feb 22, 2023 | AI DJ launch — "a personalized AI guide that knows you and your music taste so well that it can choose what to play for you", spoken commentary between tracks (https://newsroom.spotify.com/2023-02-22/spotify-debuts-a-new-ai-dj-right-in-your-pocket/) |
| May 16, 2023 | UK + Ireland |
| Aug 8, 2023 | 50 markets |
| Jul 17, 2024 | Spanish-language DJ |
| **May 13, 2025** | **DJ takes voice requests** (https://newsroom.spotify.com/2025-05-13/dj-voice-requests/) |
| Oct 15, 2025 | Spanish + text requests, personalized prompts |

And Spotify proved the *exact* music+spoken interleaving mechanic six years ago:

- **Your Daily Drive**, June 12, 2019 — "Music and News That'll Brighten Your Commute", personalised
  music with news segments spliced in —
  https://newsroom.spotify.com/2019-06-12/your-daily-drive-music-and-news-thatll-brighten-your-commute/
- **Daily Sports**, Aug 17, 2020 — explicitly a "mixed-media playlist" —
  https://newsroom.spotify.com/2020-08-17/daily-sports-a-new-mixed-media-playlist-fans-of-any-team-can-root-for/

Note *why* those work: Spotify owns both the music and podcast rails in one app. That is precisely the
position a third party does not occupy, on any platform.

Amazon Music **Maestro** is playlist generation only — prompt-driven, no spoken commentary or DJ hosting
(https://www.aboutamazon.com/news/entertainment/amazon-music-maestro-ai-playlist-generator). Date widely
reported as April 2024 — **SECONDARY, unverified**.

### Threat assessment

**Near-term threat from Apple: LOW.** Three-plus years after Spotify, Apple has shipped no AI DJ, its
radio hosts are human, and it moved spoken content out of Music.

**Strategic risk: HIGH.** Apple holds every building block — AutoMix, on-device intelligence, hosted
stations, the listening data — *and* controls the API the concept depends on. It ships itself
beat-matched AutoMix and third parties a duration-only crossfade. Building a product whose core
differentiator Apple could ship as a first-party feature, on Apple's own API, under terms Apple
interprets at "sole discretion", is the textbook platform-dependency trap.

### Prior art on Apple Music specifically

**NO EVIDENCE FOUND** for any third-party app interleaving Apple Music with podcasts or spoken audio.
Consistent with §3 and §5 — there is no mechanism and no permission. The absence of prior art here is
evidence of the constraint, not of an unexploited opportunity.

**Caveat on this section:** WebSearch was unavailable for much of the research session, so non-Spotify
competitor coverage and the App Store sweep are thinner than ideal. Read those NO EVIDENCE FOUND tags as
"not searched adequately", not "does not exist."

---

## Consolidated blockers vs workarounds

### Hard blockers — no workaround

1. **DPLA §3.3.6(D): "MusicKit Content cannot be synchronized with any other content."** The concept's
   core mechanic is named and prohibited. VERIFIED.
2. **DPLA §3.3.6(D): purpose-limitation** to "facilitating access to Your end users' Apple Music
   subscriptions", with relevance determined at "Apple's sole discretion". VERIFIED.
3. **DPLA §3.3.6(D) + ASRG 4.10: no monetisation** of Apple Music access, direct or indirect, including
   in-app purchase and advertising. Kills the revenue model at the $1–3k MRR bar independently of
   everything else. VERIFIED.
4. **No podcast user data on any Apple API.** No subscriptions, no follows, no history. Breaks `map.md`
   step 2 on this platform. NO EVIDENCE FOUND, searched exhaustively.
5. **No volume/gain API, no decoded audio, no AVAudioEngine seam.** True crossfade between Apple Music
   and your own audio is impossible. VERIFIED by exhaustive symbol enumeration.

### Real but partial constraints

6. **No listening history.** Recent plays cap at 10 resources / 30 tracks, no lookback window. You must
   build your own longitudinal store, with a cold start per user. And `ApplicationMusicPlayer` playback
   does not update Apple's play counts, so your app does not feed its own signal.
7. **Standard-media-controls clause** conflicts with a continuously auto-advancing station.
8. **Apple Music Identity Guidelines** mandate Apple Music branding — the app cannot present as a neutral
   station.
9. **Platform gaps**: crossfade iOS 18+ iPhone/iPad only; playback macOS 14+; no watchOS playback; no
   automatic user-token path on Android.

### Things that are NOT blockers — do not cite these

- **The audio session.** `.playback` + `.mixWithOthers` works; MusicKit interrupts only "nonmixable"
  sessions. The ticket's make-or-break technical question resolves in the concept's favour.
- **Playback control.** Queue building, pause, seek, resume all work. Better than Spotify's SDK.
- **A broadcast/radio-station prohibition.** It does not exist in any Apple document.
- **A standalone MusicKit ToS.** It does not exist.

### Non-workarounds — stated explicitly

- **Sequential handoff** (pause music, play podcast, resume) solves the audio problem and **does not
  solve the licence problem.** Interleaving is exactly what the no-synchronization clause names. A
  workaround that violates a clause is not a workaround.
- **Free/ad-supported to dodge the monetisation clause** fails: the clause names advertising and
  "requesting user info" alongside in-app purchase, and a free app cannot clear a $1k/mo MRR floor.
- **RSS/OPML podcast import** genuinely does route around blocker 4 — but it does nothing for 1, 2, 3,
  or 5, and worsens onboarding.
- **Requesting an entitlement**: NO EVIDENCE FOUND of any MusicKit entitlement path. §3.3.6(B) and (C)
  describe entitlement applications for other APIs; (D) conspicuously does not. INFERRED: no door.

## Consequence for the map

The **platform-native variant is dead on Apple Music** — on contract, not on capability, and the
monetisation clause means it stays dead however the engineering is arranged.

Per `map.md`, this does not end the map: licensed/royalty-free catalogues and the user's own local
library remain live variants. Two findings here transfer to them and should be carried forward rather
than re-derived:

- **The podcast-data problem is Apple-wide, not MusicKit-specific.** No Apple API exposes user podcast
  subscriptions or history. Any iOS-first variant needs RSS/OPML import regardless of music source.
- **The "suitable moment to interrupt" problem** (`map.md`, Not yet specified) is untouched by this
  ticket and remains unpriced.

Also worth flagging to the incumbent-feature ticket: Spotify's *Your Daily Drive* (2019) is direct prior
art for the concept's core mechanic, shipped by a platform owner, and its existence is a demand signal
and a competitive fact that ticket should weigh.
