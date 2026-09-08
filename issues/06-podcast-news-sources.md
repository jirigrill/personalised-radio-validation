# 06 — Which podcast feeds and news audio can be legally used and mixed?

Type: research
Status: open
Blocked by: —

## Question

The music layer has its own tickets. This one covers the **non-music audio**: what can legally be
programmed into the stream, and under what terms.

Prior work established that **open RSS podcast feeds have no ToS layer** and the binding constraint
is copyright. Carry that over and verify; then push into what is new here — this concept does not
clip or store audio, it **re-broadcasts a full episode inside a curated stream**, which is a
different legal question from the prior project's excerpt-capture.

1. **Open RSS podcasts.** Playing a full episode from its public enclosure URL in a third-party
   player is the normal, expected use of a podcast feed. Confirm that. Then find the edges:
   - Do major feeds carry licence terms, `<podcast:license>` tags, or robots/ToS restrictions?
   - **Dynamic ad insertion**: if the app fetches the enclosure directly, are ads served, and does
     interrupting an episode mid-roll or skipping a segment breach anything or break the
     publisher's monetisation? Prior work flagged DAI drift as a real problem — carry that over.
   - Does **interrupting an episode to play music and returning** constitute a derivative work or an
     unauthorised modification? Look for the actual doctrine and any precedent, and flag where a
     lawyer is required. Not legal advice.
   - Are analytics/prefix services (Chartable, Podtrac, OP3) or IAB-compliant download reporting
     affected in a way publishers would object to?
2. **Walled-garden podcasts.** Spotify exclusives, Apple Podcasts subscriptions, Amazon/Wondery.
   Confirm these are unreachable, and estimate **what fraction of typical listening they represent** —
   if a user's favourite shows are exclusives, the product is broken for them regardless of licensing.
3. **Podcast discovery/metadata APIs.** Podcast Index, Listen Notes, Taddy, iTunes Search. Licensing,
   cost, rate limits. (Note: prior work found Podcast Index stats endpoints returning 403.)
4. **News audio — the actual question.** The brief says "freely broadcastable news segments". Test
   that premise; it may be false. Investigate concretely:
   - Public broadcasters with explicit reuse terms: **NPR** (API and terms — note NPR One's
     retirement and what replaced it), **BBC**, **Deutsche Welle**, **VOA** (US-government works,
     public domain), **Al Jazeera**, **AP/Reuters** audio licensing, **Euronews**, **ABC/CBC**.
     For each: is there a machine-readable audio feed, and do its terms permit a **commercial**
     third-party app to include the audio in a paid subscription product? Commercial use is the
     crux — many public-broadcaster terms permit non-commercial reuse only.
   - Creative-Commons-licensed news audio, and whether NC (non-commercial) clauses bite.
   - **AI-generated news read-outs** from text sources as an alternative — does that dodge audio
     licensing, and what does it create instead (text licensing, factual-accuracy risk, and note
     that news publishers are actively litigating AI summarisation)?
5. **Verdict.** State plainly whether a legally clean podcast + news layer exists, and whether news
   is worth including in an MVP at all or should be cut.
