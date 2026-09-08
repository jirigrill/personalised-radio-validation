# 11 — Unit economics, including music licensing as a hard input

Type: research
Status: open
Blocked by: 05

## Question

What does it cost to run this per user per month, and does cost threaten viability?

**Important divergence from prior work.** `podcasts-notes-validation` concluded that costs were
irrelevant (99.9%+ net margins; Apple's cut dwarfed all infrastructure). **That conclusion does not
carry over.** Transcription of short clips is cheap; streaming music is not. Cost the following
separately and state which variant each applies to:

1. **Music licensing — the section that matters most.** For the non-platform variants:
   - **Royalty-free / directly-licensed catalogues** (Epidemic Sound, Artlist, Soundstripe, Jamendo,
     Free Music Archive, ccMixter, Audius) — do their licences permit **redistribution via streaming
     in a commercial app**, as opposed to synchronisation into video? This is the usual fatal
     distinction; check it explicitly.
   - **B2B music-licensing APIs** built for exactly this (Feed.fm, Tuned Global, 7digital, Songtradr,
     and any others surfaced by the platform scout). Published pricing, minimum commitments, and
     per-stream or per-user rates. **Minimum annual commitments are the likely killer at $1–3k MRR** —
     if the floor is $10k/yr, say so plainly.
   - Statutory/blanket licensing (US: SoundExchange/DMCA §114 webcasting; equivalents elsewhere) —
     what a non-interactive personalised radio service would owe per performance, the minimum annual
     fees, and whether the **interactivity rules** (skip limits, no advance track disclosure, the
     sound-recording performance complement) are compatible with "plays your liked songs" — which
     sounds **interactive by definition** and may therefore be ineligible for statutory rates. This
     could be the single most important finding in the ticket.
   - **Local library variant**: user's own files, so zero licensing cost. Note it as the floor case.
2. **Podcast and news audio**: bandwidth if proxied vs zero if streamed directly from the publisher's
   CDN. Note that proxying may also breach analytics expectations (cross-reference the podcast ticket).
3. **Segmentation / "suitable moment" detection**, if it needs transcription or analysis: per-episode
   cost, and whether it is cacheable across all users of the same episode (it should be — quantify
   the saving).
4. **TTS and LLM** — take from the AI-DJ ticket rather than recomputing.
5. **Infrastructure**: servers, storage, egress for a continuously streaming session. Note that a
   streaming app has materially higher egress than a note-taking app.
6. **Store cut**: Apple/Google 15–30%, and the Small Business Program threshold.

Output: **COGS per user per month for each surviving variant**, gross margin at candidate price
points, and a clear statement of whether any variant is **structurally unprofitable** at the
$1–3k MRR bar. Unlike the prior project, "cost is irrelevant" is a conclusion that must be earned.
