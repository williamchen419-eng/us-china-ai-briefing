# US–China AI Briefing — agent notes

The map and the rules an agent working in this repository follows. Architecture edition: prompts and design docs only; the live data and deployment are not included.

## Docs map

| Doc | What it's for |
|-----|---------------|
| [README.md](README.md) | The overview: what it does, the agents, the layout |
| [ARCHITECTURE.md](ARCHITECTURE.md) | How it's built: diagram, schedule, data flow, harness, monitors |
| [HARNESS-ENGINEERING.md](HARNESS-ENGINEERING.md) | Why it's built this way |
| [sources.md](sources.md) | The annotated source list (owner, incentive, what to watch for) |
| `prompts/` | The agent prompts. Each agent reads its prompt from here at run time, so editing one changes the next run |

## Pipeline rules

1. **Scouts never kill items.** They tag (🟢 Signal / 🟡 Uncertain / 🔴 Likely noise) and pass everything downstream.
2. **The Strategist makes kill decisions** with logged reasoning. Killed items go to a discard pile, not the void.
3. **The Red Team audits the discard pile** every run. Wrongly killed items are resurrected.
4. **Feedback flows upstream.** The Red Team writes `feedback/scout-instructions.md` for tomorrow's scouts.
5. **Thread tracker persists** across days in `tracker/threads/`. The Strategist updates it.
6. **Company tracker persists** in `tracker/companies/`. The Strategist updates it.
7. **Provenance gate.** Single-source, analyst-estimate `[A]` or incentive-window management `[M]` claims are never Signal until second-sourced; every headline number is verified or caveated.
8. **Structural facts are grounded.** `reference/landscape.md` holds standing facts; don't contradict them without new evidence.
9. **Monitors, not targets.** The prediction ledger and the oracle grade the pipeline from outside. Never write "improve the score" into a prompt.
10. **No secrets in the repository or the prompts.** Credentials live in the platform's secret store and are referenced by name only.

## Key principles

- Filter late, not early. It is cheap to pass 50 headlines downstream and expensive to miss the one that mattered.
- Never restate a press release without saying it is a press release.
- Track running threads across days; the absence of news can be signal.
- Flag when a story's framing shifts across sources; that is often the real story.
- If nothing meaningful happened, say so. Don't manufacture importance.
