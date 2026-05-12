# Writing Canon

**Status:** Canonical. Part of CC-Suite™ v2.

**Scope:** All agent-produced text output in all governed fleets. Chat responses, memos, handoffs, internal documentation, external-facing copy, code comments, commit messages.

---

## Core Concept

**Every fleet has a voice. Without explicit writing rules, AI agents default to a uniform "AI voice" that is recognizable, neutral, and on-brand for no one.** The writing canon makes voice rules explicit and load-bearing.

This template starts with the highest-leverage single rule: the em dash canon. Most fleets benefit from declaring additional rules (sentence length, jargon avoidance, salutation conventions, banned vocabulary, brand-voice traits). Add those as you go.

---

## Rule 1 — Em Dash Canon

**AI writing uses em dashes several times more often than skilled human writing.** Default AI output reaches for the em dash because the mark works for many structural jobs. That is exactly why it should be used sparingly: it is a blunt instrument, and more precise punctuation exists for every job it does.

### Five jobs and their precise substitutes

| What the em dash is doing | Use instead |
|---|---|
| Introducing or expanding | Colon |
| Two complete thoughts | Period |
| True aside or parenthetical | Parentheses |
| Soft pause or addition | Comma |
| Related but distinct clauses | Semicolon |

### The test

Reach for the em dash. Stop. What structural job is this doing, and is there a more precise mark? If yes: use that mark. If no: the em dash earns its place. Cases that pass: rhythmic beats, deliberate punches, pause before payoff.

### Target frequency

Roughly 10% of default AI em-dash frequency. Not zero — intentional. Every em dash that appears was chosen over a real alternative and won.

### End-of-draft check

Search every em dash in the draft. Read each one aloud. Filler gets replaced. Keepers stay.

---

## Suggested Additional Rules (Declare As Needed)

These are not enforced by this template. Add them to your fleet's `WRITING_CANON.md` as your voice develops.

- **Sentence length.** Target a maximum sentence length appropriate for your audience. Long sentences without clauses are the second-most-common AI tic after em-dash overuse.
- **Banned vocabulary.** Words that signal AI authorship and should be removed: "leverage," "optimize," "streamline," "empower," "solution," "platform-as-hype," "ecosystem," "revolutionize," "transform," "unlock." Most fleets benefit from banning these explicitly.
- **Salutation conventions.** "I hope this finds you well," "circle back," "touch base" — most fleets ban these as artifacts of corporate-AI defaults.
- **Exclamation marks.** Some fleets ban entirely. Some allow rarely. Declare explicitly.
- **Voice / tone canon.** If your fleet has a brand voice (operator-to-operator, technical, warm, terse, formal), reference your voice spec here.

---

## Mechanics

1. **At session start.** Agents in a fleet that loads this canon must know the em dash rule before producing any output.
2. **At draft completion.** Every agent runs the end-of-draft check on any deliverable Tier B+ (chat responses can be more relaxed; memos / outreach / docs cannot).
3. **In review.** When a reviewer (auditor, human authority, peer agent) finds writing-canon violations, the producing agent updates the draft. Repeated violations after correction are subject to the same termination thresholds as any other repeat failure (back-to-back repeat, three-strike).

---

## Compose With Asymptote Protocol

The Asymptote Protocol's voice canon lens (the first lens in the outreach / memo lens set) enforces this writing canon in a single pass. When running an Asymptote cycle on text output, the voice canon lens automatically checks em dash usage plus any other rules declared here.

See `governance/ASYMPTOTE_PROTOCOL.md` and `HARNESS_CORE.md` §15.

---

## Related Files

- `HARNESS_CORE.md` §14 — the entry point for the writing canon
- `governance/ASYMPTOTE_PROTOCOL.md` — uses the voice canon lens to enforce this canon on every Tier A+ text output
- Your fleet's brand voice spec — referenced as needed

---

## Reference Implementation

Mise Inc. ratified its em dash canon on 2026-04-25 via "THIS IS THE WAY" founder trigger. The brain file is `docs/brain/042526__em-dash-writing-canon.md`. The rule applies fleet-wide to all 10 governed agent types plus the Scribe, and is enforced via the Asymptote Protocol's voice canon lens for any Tier A+ text deliverable. Mise's additional writing rules (operator-voice for outreach, sentence-length targets for marketing copy) live in separate canons that compose with the em dash rule: `docs/brain/041626__operator-to-operator-outreach-canon.md` (operator voice), `VALUES_CORE.md` (brand-voice axioms).
