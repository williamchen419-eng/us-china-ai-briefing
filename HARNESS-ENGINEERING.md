# Harness Engineering — why the pipeline is built this way

Working notes on **prompt, context and harness engineering**, and how the three layers map onto this pipeline. The motivating worry: *is the Red Team a real check, or just one Claude blessing another Claude?* This document answers that honestly and turns the answer into design rules.

Two sources, kept distinct:
- **[A]** *"Harness Engineering: The Execution Layer AI Agents Actually Need"* (Min Yin, Milvus blog, April 2026). It cites Mitchell Hashimoto (who coined "engineering the harness"), OpenAI's *"Harness Engineering: Leveraging Codex in an Agent-First World"* and Anthropic's *"Harness Design for Long-Running Application Development."*
- **[B]** Applied research on this pipeline: what running it every day showed.

---

## 1. The three layers [A]

| Layer | Optimises | Scope | In this repo |
|-------|-----------|-------|--------------|
| **Prompt engineering** | What you *say* to the model | One exchange | The agent prompts (`prompts/*.md`), personas, the claim-strength ladder, the "boring explanation" rule |
| **Context engineering** | What the model can *see* | The context window | Thread and company trackers, the scout-instructions feedback file, the last-three-reports read, per-desk feed files |
| **Harness engineering** | The *world* the agent works in | Hours of autonomous execution | The orchestration (scheduler → prefetch → four agents → email), the Red Team evaluator, web-search grounding, failure alerts |

They are **a progression, not competitors**: as models get more capable the same project moves up the stack.

> **Core definition [A]:** harness engineering designs the execution environment around an autonomous agent: *which tools it can call, where it gets information, how it validates its decisions, and when it should stop.*

---

## 2. Four harness lessons from OpenAI [A], and their analogues here

| # | Their problem | Their fix | Our analogue |
|---|---------------|-----------|--------------|
| 1 | No shared understanding of the codebase | Keep the agent file to ~100 lines, a *map* not a rulebook, pointing into structured docs. "If it isn't in context at runtime, it doesn't exist for the agent." | Keep `CLAUDE.md` and `ARCHITECTURE.md` maps; the trackers and feedback files are the real runtime context. Watch for prompt bloat. |
| 2 | Human QA can't keep pace | Give the agent runtime tools and a deterministic threshold for "done" | Our deterministic threshold is external ground truth: web-search verification and the prediction ledger (did the predicted event happen?). |
| 3 | Architectural drift | Strict layering, enforced mechanically by linters | Our rules (single-source claims are never Signal, the claim ladder) start as LLM judgment; push them toward mechanical checks. |
| 4 | Silent technical debt | Principles in the repo; scheduled background scans; small continuous payments | The monthly Pipeline Consultant: continuous, one change at a time. |

---

## 3. Why agents can't grade their own work [A]

Two self-grading failure modes:

1. **Context anxiety.** As the window fills, a model senses the limit and wraps up early. Mitigations are compaction, or a context reset: a fresh instance plus a structured handoff artifact, where gaps in the artifact are gaps in understanding.
   - *Here:* every agent is a fresh instance handing off through structured files (`01-raw.md`, `03-strategic.md`, the trackers). The risk is a thin handoff file silently dropping context.
2. **Self-evaluation bias.** An agent grading its own output scores it high: it spots a problem, talks itself into "not serious", and approves work that should fail. **The fix borrows from GANs: separate the generator from the evaluator completely.**

> **The decisive principle [A]:** the evaluator must be architecturally *separate* from the generator (shared context reintroduces the bias) and should have *runtime tools* to test behaviour, not just review output.

### The honest read on this Red Team
- ✅ **Role separation:** the Red Team is a separate agent with fresh context, reading the Strategist's artifact.
- ✅ **A runtime tool:** web search. An early catch (killing an unverifiable headline figure) worked *because* the Red Team checked it against reality, not because one model out-read another. Quality control works when it touches external ground truth, and is soft when it is just another opinion from the same model.
- ⚠️ **Same model, shared blind spots.** When the Strategist and the Red Team run the same model, a plausible error that convinces one tends to convince the other. That is why the two seats now run **different models**, and why a **different vendor's** model grades every report.

### The failure shape [B]
- **Premature consensus:** agents debating qualitative material converge early on the generic, safe read. The defence is the claim-strength ladder and the "boring explanation" rule; the opposite risk is *premature cowardice*, hedging everything into "probably coincidence".
- **Re-engineer safely:** break the echo chamber with deterministic structure and human verification. The prediction ledger (reality grades us) and the human reading the email do this.
- **The rebuttal:** a robust harness does not rely on a second LLM saying "looks good". It leans on deterministic ground truth (tests, schema and property checks) and, where an LLM judge is unavoidable, on a **diverse-model oracle**: different training, different blind spots.

---

## 4. The best harness components are designed to be deleted [A]

Every harness component encodes an assumption about a current model limitation, and those assumptions expire. After any model upgrade the first question is **"what can we remove?"**, not "what can we add?" The claim ladder, the source-verification rules, even the oracle are mitigations for today's models: re-audit them on each upgrade and delete what the model now does natively.

---

## 5. The verdict

The pipeline is not broken, but a same-model auditor is the textbook anti-pattern, while the parts that genuinely work (web-search grounding, the prediction ledger, the provenance gate) are the harness-correct ones. The fix is not a rewrite: it is shifting weight from *"a second opinion from the same model"* to *"external ground truth plus one diverse oracle"*, and turning judgment calls into mechanical rules wherever possible.

**There is no unit test for "is this the right read on US–China AI."** The domain is irreducibly qualitative, so the realistic strategy is:
1. **Maximise the surface that touches external ground truth.**
2. **Add one diverse oracle**, so the qualitative core is checked by different weights.
3. **Keep the structural devices**, and watch for over-correction into cowardice.

---

## 6. What was built, by layer

**Harness layer**
1. **Prediction ledger.** Reality grades the pipeline's own calls. Scored by the Strategist, a different agent from the Red Team that writes the calls (an independence guardrail), and re-scored by the oracle. It tracks hit rate, unresolved rate and the share of *non-obvious* hits, which is the cowardice detector. Monitor, never a target.
2. **Cross-vendor oracle.** A GitHub Actions job runs a Gemini model on every published report. It grades the judgment dimensions, lists unsupported claims, flags where it disagrees with the Claude agents, and re-scores the predictions. It never edits the report, and nothing optimises against it. A fallback ladder of models from the same vendor keeps it running through outages; falling back to a Claude model is excluded, because that would remove the diversity.
3. **Mandatory source verification.** The provenance tags plus a hard gate (single-source, analyst and management claims are capped below Signal until second-sourced), every headline number verified or caveated, and an internal-consistency check. Its first live test caught an internal contradiction that the same-model Red Team had missed.

**Context layer**
4. **Provenance tags travel with each item** from the scouts onward, instead of being re-derived at the end.
5. **Retrieval as trackers grow.** Trackers are read wholesale today; as they grow, each agent should pull only the relevant threads (semantic plus keyword retrieval). Note the principle and defer the tooling until it is needed.
6. **Discovery quota.** A horizon-scanning budget so the pipeline keeps finding new threads instead of only servicing known ones, which guards against premature consensus on the same safe themes.

**Prompt layer** (mostly done: re-audit, don't add)
7. The claim-strength ladder and the "boring explanation" rule.
8. On each model upgrade, re-check whether these rules are still needed.

---

## 7. Governing the build: explore and exploit

Once the harness works, the risk flips from complacency to over-improvement: touching the working core until it breaks. The answer is to run explore and exploit in separate structures.

- **The core (the agent chain and the email) is exploit.** Touch it only on a pre-committed trigger: a real miss the backstops did not catch, or a degradation measured over several runs. Not on a hunch.
- **The monitor layer is explore.** Experiments go there; monitors cannot edit a report, so they cannot break production.
- **A trigger-gated backlog resolves the tension.** Every idea is written down with the trigger that would justify it, and acted on only when the trigger fires.
- **Monitors are never optimisation targets.**

Two examples of disciplined *non*-changes:
1. **Compliance, not a missing rule.** The Strategist labelled weak-provenance items correctly but kept them as Signal anyway. More prompt text was the lever that had already failed, so instead the Red Team backstop was trusted and a deterministic numeric lint was built as a quarantined monitor, with a stated trigger for wiring it into the core. With only one documented miss, the trigger was later closed: the lint stays a monitor.
2. **Delegation.** Splitting the Strategist's ten steps across new agents was tested and rejected: each new agent pays the context-reload cost and adds handoff drift. The sound delegations were deleting duplication and moving narrow, independence-sensitive tasks to the diverse model.

---

## 8. Closing the loop from monitor to Red Team

The echo-chamber worry stopped being abstract when the oracle caught a causal-attribution overreach (a company's valuation said to "validate" a market it barely depended on) that the same-model Red Team passed. It was the third oracle catch the Red Team had missed: a recurring blind spot. The Red Team's personas are verification lenses ("is it sourced? is the number right?"), so they do not fire on a confounded causal leap where every individual fact is true.

**The bug it exposed:** the oracle was producing catches that went nowhere. Its output was written "for the next Red Team", but the Red Team's read list never included it. The evaluator existed; the loop was open.

**What changed:**
- The Red Team now reads the latest oracle, numeric-lint and tracker-lint output before it starts, so a catch from one run feeds the next.
- A causal-attribution gate in the Red Team prompt: for every "X validates Y" claim, name X's single largest alternative driver. A "designed to be deleted" rule that patches a current blind spot.
- A tracker-consistency lint: flags thread files whose body contradicts their status header, with no LLM.
- The monthly review now counts oracle-caught / Red-Team-missed errors by class, the one metric that directly sizes the echo-chamber gap.

A later calibration study treated the causal-attribution gate as a natural experiment: catches in that class fell relative to other classes while factual and arithmetic catches rose, so the blind spot partly moved rather than closed. That suggests it is at least partly addressable by prompt, but the evidence is not decisive: the gate shipped together with the loop closure, so the Red Team may simply be reading the oracle's earlier catches. Small sample; a monitor, never a target.

---

*A living document: recalibrate on every model upgrade, and always ask first what can be removed.*
