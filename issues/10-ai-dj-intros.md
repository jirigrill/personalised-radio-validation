# 10 — AI-generated DJ intros: cost, legality, and whether anyone wants them

Type: research
Status: open
Blocked by: 05

## Question

Blocked on the platform sweep (05) deliberately: if interleaving is blocked on every music source,
this question is moot and researching it first wastes a session.

The concept inserts AI-generated intros/outros between songs and podcast segments. Three distinct
risks:

1. **Cost.** Per-user-per-month TTS cost for a realistic usage profile — state the profile
   explicitly (e.g. a 45-minute commute twice daily, N transitions per session, M words per intro).
   Compare current published pricing for the realistic options (ElevenLabs, OpenAI TTS, Google,
   Azure, Amazon Polly, on-device `AVSpeechSynthesizer`). Note which are **cacheable** — a generic
   intro can be reused, a personalised one referencing the specific track cannot; quantify the
   difference. Also cost the **script generation** (LLM tokens), not just the speech.
   Then state the COGS per user at the $1k and $3k MRR bars and whether it threatens the margin.
2. **Voice legality.** Any voice-cloning or celebrity/real-DJ-soundalike route raises
   name-image-likeness and right-of-publicity exposure, plus the vendors' own terms. What do
   ElevenLabs and the others permit for commercial use, and is a distinctive synthetic voice
   defensible? Note the ELVIS Act and similar state statutes, and the EU AI Act's disclosure
   obligations for synthetic audio. Not legal advice; flag where a lawyer is required.
3. **Platform and store rules.** Does wrapping AI speech around licensed music breach any music
   platform's terms (cross-reference the platform tickets)? Any App Store rules on AI-generated
   content or required disclosure? Does Spotify's own AI DJ existing set a precedent or a
   prohibition?
4. **Does anyone want it?** Cross-reference demand evidence. Spotify DJ's reception is the closest
   available proxy — if users find AI DJ commentary annoying, this feature is a cost centre that
   reduces value. Be willing to conclude the feature should be cut.
5. **On-device alternative.** `AVSpeechSynthesizer` and Apple's newer on-device speech: zero
   marginal cost, no vendor terms, worse quality. Is it good enough for a radio-show illusion?

Conclude: keep, cut, or defer past MVP — and the per-user cost if kept.
