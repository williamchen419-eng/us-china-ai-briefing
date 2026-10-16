# Reading the prompts

| File | Agent |
|------|-------|
| `01-china-desk.md` | China Desk (scout) |
| `02-us-desk.md` | US Desk (scout) |
| `03-strategist.md` | Strategist |
| `04-redteam-editor.md` | Red Team + Editor |
| `05-consultant.md` | Pipeline Consultant (monthly) |
| `06-oracle.md` | Oracle (a Gemini model, run by a GitHub Action) |

These are the prompts the live pipeline runs, lightly edited for publication (personal details, dates and incident references removed). A few things to know when reading them:

- **Personas are stylistic lenses only.** Prompts ask an agent to think "like" a newsroom, a bank, a fund, a consultancy, an intelligence analysis unit, a think tank or an advisory firm. That describes a way of reading the news; it implies no affiliation with, or endorsement by, any of those organisations.
- **The Watch List is a calibration tool, not advice.** The Red Team is told to make bold, specific, falsifiable calls so that the prediction ledger can grade them. Calls concern observable events; nothing here is a recommendation to buy, sell or hold anything.
- **Internal files they mention are not included:** `INCIDENTS.md` (the failure log), `SUGGESTIONS.md` (the backlog), `CHANGELOG.md`, `usage.md` (per-run logs), `audit/consultant/`, `feedback/archive/`, `feedback/scout-instructions-archive.md`, `feedback/watchlist-ledger-archive-*.md`, and the deterministic `scripts/*.mjs` (described in [`../scripts/README.md`](../scripts/README.md)).
- **`feedback/scout-instructions.md` has three sections:** (A) follow-ups on known stories, (B) "bring something new": a short list of live directions, and (C) standing rules, which only the owner edits.
- **D2 / D4 / D5 / D7** are the quality-rubric dimensions the oracle grades: kill-decision quality, analytical honesty, coverage honesty and decision-usefulness (defined in `06-oracle.md`).
- **"CLAUDE.md rule N"** refers to the numbered pipeline rules in [`../CLAUDE.md`](../CLAUDE.md).
- **Step 0 in each prompt** syncs the agent's checkout to the latest `main` before it reads anything, because a cloud checkout can be stale.
