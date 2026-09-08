# 15 — Enquiry emails: Feed.fm and Adaptr pricing and permitted use

Type: task
Status: open
Blocked by: —

## Question

**HITL — the developer must send these. Nothing else can resolve them.**

Ticket 04b established that the licensed-catalogue route turns on facts no amount of research can
reach: none of the B2B music vendors publishes pricing, and their public terms do not settle whether
this specific use case is permitted. `WebSearch` was also down for that session, so "NO EVIDENCE
FOUND" is honest but not final. Two emails close the highest-value remaining unknown in the map.

**Do not let this ticket be resolved by inference.** If no reply arrives, the answer is "no reply
arrived", and ticket 05 must decide without it.

### 1. Feed.fm — the single highest-value unknown remaining

Feed.fm is the only vendor confirmed to bundle rights + SDK + major-label catalogue +
indemnification (04b hypothesis (b), CONFIRMED; iOS SDK v5.8.0, 2026-06-02). Ask:

- **The minimum annual commitment**, and the pricing model (per-user, per-stream, flat tier). This is
  the decisive number: at a $1–3k MRR bar, a five-figure annual floor ends the route.
- Whether a **consumer-facing, subscription-priced personalised radio app** is an eligible use case
  at all, or whether their licensing is aimed only at B2B/in-venue/fitness/games contexts.
- **Whether the stream may be interleaved with third-party podcast and news audio**, given T&C
  §5.6/§5.5(a) appear to bar insertion into or alteration of the stream. Ask directly: may our app
  play a podcast segment, then Feed.fm music, then return to the podcast, in one session?
- **Whether AI-generated TTS DJ announcements may name the track or artist**, before or after it
  plays. Note that §114(d)(2)(C)(ii) bars advance announcement on statutory routes — establish
  whether Feed.fm's licences are statutory or direct, because that determines whether the bar applies.
- Whether **crossfading or ducking** their audio under speech is permitted.
- iOS SDK: can their audio **coexist with another player in one `AVAudioSession`**?

### 2. Adaptr — the second live candidate

`app.adaptr.com` registration is live. Ask the same core questions:

- Current pricing and any minimum commitment.
- Whether a personalised consumer radio app satisfies **Exhibit B eligibility**.
- Interleaving with third-party spoken audio, and DJ announcement of titles/artists.

## Output

Record, for each vendor: whether a reply arrived, the pricing and minimum commitment if disclosed,
and a yes/no on each permitted-use question — quoting their words. If a vendor declines to disclose
pricing without a sales call, record that as the finding.

Then state plainly whether the **licensed-catalogue variant is affordable at the $1–3k MRR bar**, or
whether it is closed. That answer is a required input to ticket 05.
