# personalised-radio-validation

Research-based **go/no-go validation** of a "personalised radio" app concept: an app that reads a
user's music listening history, liked songs and followed podcasts, then programmes a continuous
radio-style stream — podcast episode, pause at a suitable moment, liked songs, back to the episode —
with optional news segments and AI-generated DJ intros.

Target success bar: **$1k/mo MRR floor, $3k/mo target.** Research and validation only; no product
implementation in this phase.

Methodology and document conventions mirror the prior
[`podcasts-notes-validation`](https://github.com/jirigrill/podcasts-notes-validation) project.

## Where things live

- **The map** — the canonical index of the effort:
  [issue #18](https://github.com/jirigrill/personalised-radio-validation/issues/18)
  (labelled `wayfinder:map`).
- **Tickets** — GitHub issues, sub-issues of the map, labelled `wayfinder:research` /
  `wayfinder:grilling` / `wayfinder:task`. Blocking uses native GitHub issue dependencies, so the
  frontier (open, unblocked work) is visible in the tracker UI.
- **Research findings** — [`docs/research/`](docs/research), one file per resolved ticket, with
  every claim tagged VERIFIED / SECONDARY / INFERRED / NO EVIDENCE FOUND.

## Status

The platform feasibility sweep is **complete, and every music platform examined is BLOCKED** — each
by a different clause, drafted by a different legal team:

| Platform | Verdict | Deciding reason |
|---|---|---|
| Spotify ([#1](../../issues/1)) | **BLOCKED** | Policy III.7 bars segue/mix/overlap; IV.2 bars monetising a Streaming SDA; quota caps at 5 users pre-launch; Compliance Tips names "DJ/Mixes" as a disallowed use case |
| Apple Music ([#2](../../issues/2)) | **BLOCKED** | DPLA §3.3.6(D) — content cannot be synchronized with other content, and no legal revenue model (ASRG 4.10 / 4.5.2(i)) |
| YouTube Music ([#3](../../issues/3)) | **BLOCKED** | Policies III.I.1 + III.I.9; listening-history API removed in 2016/2020 |
| Tidal ([#4](../../issues/4)) | **BLOCKED** | Guidelines v3.0 §III bars the mechanic *including Tidal-into-Tidal*; 30-second previews only; zero podcasts |
| Deezer ([#5](../../issues/5)) | **BLOCKED** | ToS §IV "NON-COMMERCIAL USE" — private/family scope only |

Two clause formulas appear **independently drafted** in both Spotify's and Tidal's terms, which reads
as a standard label-licensing requirement rather than one platform's idiosyncrasy — so there is no
friendlier platform to look for.

The deeper finding ([#5](../../issues/5)) is that **the blocker was never price**:
17 U.S.C. §114(d)(2)(C)(ii) bars a statutorily-licensed webcaster from announcing specific titles or
featured artists in advance, which is the AI-DJ feature. Three routes can play familiar music; none is
familiar **and** affordable **and** DJ-compatible.

Next: [#6](../../issues/6) — the platform-sweep decision, which asks which corner of that trilemma to
give up.
