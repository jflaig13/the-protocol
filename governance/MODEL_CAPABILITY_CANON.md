# Model Capability Canon

**Status:** Canonical. Part of CC-Suite™ v2.

**Scope:** All fleets running on LLMs. Mandatory whenever the underlying model is updated, replaced, or extended with new primitives.

---

## The Rule

**Every fleet running on LLMs MUST maintain a canonical, version-stamped declaration of which model is currently in production, what changed at the last upgrade, and what every agent must do differently as a result.**

A model upgrade without an explicit capability canon is an implicit drift event — deprecated API parameters silently fail, new primitives go unused, hidden cost regressions land without warning, and individual agents end up calibrated to different baselines. The canon turns each upgrade into a deliberate, auditable operational event.

---

## Why It Exists

LLM upgrades are the highest-leverage drift class in any AI-based fleet. A single underspecified upgrade can:

- Land deprecated request parameters into Tier-S production code, where they return HTTP 400 the next time the path is exercised.
- Change effective per-workload cost by 30%+ with no change to per-token pricing (via tokenizer revisions that re-encode the same text at different token counts).
- Silently regress a capability the fleet depends on (for example, a model that improves on coding benchmarks but regresses on agentic web search).
- Introduce new primitives (effort levels, slash commands, vision resolutions, memory tools, hosted runtimes) that go unused for months because no agent was told they exist.

Without a canon, every agent learns about the upgrade at a different time, by accident, in a different way. The fleet calibrates to fragments.

---

## The Six-Point Declaration

Every model upgrade canon MUST declare these six things. Missing any one is an incomplete canon.

| # | Field | What to record |
|---|-------|----------------|
| 1 | **Model ID + effective date** | Exact API model identifier (not the friendly name) and the cutover date. Example: `claude-opus-4-7`, effective 2026-04-16. |
| 2 | **Breaking API changes** | Request parameters that now return HTTP 400. Assistant-prefill patterns that no longer work. Thinking modes that have been removed. Deprecated tool schemas. |
| 3 | **New primitives** | New effort levels, new tool types, new context-window sizes, new vision / PDF / memory capabilities, new slash commands or runtime features. |
| 4 | **Benchmark deltas** | Improvements AND regressions vs. the previous baseline. Regressions matter most — they signal capabilities the fleet should no longer lean on. |
| 5 | **Hidden cost shifts** | Tokenizer changes that re-encode text at different token counts. Pricing-tier changes. New beta-header costs. Compute multiplier changes for premium effort levels. |
| 6 | **Binding operating changes** | Concrete actions every agent must take. Example: "Set `/effort xhigh` at session start. Purge deprecated `temperature`, `top_p`, `top_k` from every code path. Evaluate the new memory tool for stateful agents." |

---

## Mechanics

1. **Each upgrade gets its own canon file.** Naming convention: date-prefixed, descriptive — `MMDDYY__<model>-capability-canon.md` (Mise) or your equivalent.
2. **Immutability.** Once written, the canon file is not edited in place. Successor upgrades get a new file; the old file remains valid context for code paths that have not yet migrated.
3. **HARNESS_CORE pointer.** The canonical entry point for "what model are we on" is `HARNESS_CORE.md` §13. When a new canon ratifies, §13 is updated to point to the new file.
4. **Per-role briefs.** For non-trivial upgrades (anything beyond a drop-in), each governed role gets a per-role brief stating role-specific impacts. Example: a verification role cares about vision-resolution improvements; a financial role cares about long-context Elo; a marketing role cares about adaptive thinking behavior; a tech role cares about every breaking API change.

---

## Compose With Risk Classification

A model upgrade is itself a change with a Tier × EDG. Classify before adopting.

| Upgrade shape | Likely classification |
|---------------|----------------------|
| Drop-in compatible, minor benchmark improvement, same pricing, same context window | Tier B × EDG-1 |
| New primitives added, no breaking changes, slight cost impact | Tier B × EDG-2 |
| Breaking API changes affecting non-Tier-S code paths | Tier A × EDG-2 |
| Breaking API changes affecting Tier-S financial / verification code paths | Tier S × EDG-3 |
| New context-window class, new effort level, new memory tool, AND breaking API changes | Tier S × EDG-3 or higher |

Tier S × EDG-3 upgrades require design-first review before rollout. The canon itself is part of that design.

---

## Enforcement

1. **No silent upgrades.** Agents must NOT switch model baselines without ratifying a capability canon for the new model. Doing so silently is a Tier S violation in fleets that handle financial / verification / legal output.
2. **Pre-flight check.** Before any major fleet rollout, the auditor role (Scribe or equivalent) confirms: canon file exists; per-role briefs distributed; deprecated-param sweep run across the codebase; benchmark regressions communicated.
3. **Post-rollout monitor.** For ~30 days after a cutover, the auditor watches for: deprecated-param HTTP 400 errors; tokenizer-driven cost spikes; regression-on-regressed-benchmark incidents.

---

## Related Files

- `HARNESS_CORE.md` §13 — the entry point that names the current canon file
- `governance/RISK_CLASSIFICATION.md` — Tier × EDG classification system
- `governance/LIFECYCLE.md` — how upgrades interact with agent hire / operate / review phases

---

## Reference Implementation

Mise Inc. ratified its first capability canon on 2026-04-16 for the Anthropic Opus 4.7 upgrade. The brain file lives at `docs/brain/041626__opus-47-capability-canon.md` and declared: model ID `claude-opus-4-7`, five hard API breaking changes (`temperature`, `top_p`, `top_k`, `thinking.type="enabled"` + `budget_tokens`, assistant-message prefill all returning HTTP 400), eight new primitives (`xhigh` effort level, adaptive thinking, 2576px vision, 600-page PDFs, improved memory tool, Managed Agents hosted runtime, `/ultrareview` slash command, Task Budgets beta), nine benchmark deltas (SWE-bench Verified +6.8pp, SWE-bench Pro +10.9pp, XBOW visual +44pp, OfficeQA Pro +23.5pp, with BrowseComp regressing −4.4pp), one hidden cost (new tokenizer inflates same text 1.0-1.35×), and seven binding fleet-wide operating changes. Per-role briefs distributed via `cc_execs/memos/{role}/Scribe_to_{ROLE}__opus-47-capability-brief-041626.md`. The brain file itself is treated as immutable; future Opus / Sonnet / Haiku revisions get their own capability canon files.
