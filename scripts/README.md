# scripts/

Deterministic code (Node.js, no LLM). Not included in this edition; described here so the prompts' references make sense.

| Script | What it does |
|--------|--------------|
| `fetch-feeds.mjs` | the prefetch job: fetches RSS and news-API items, splits them per desk, tags items seen on earlier days |
| `oracle-review.mjs` + `gemini-ladder.mjs` | the cross-vendor oracle and its model fallback ladder |
| `ops-check.mjs` | per-run health: artifacts present, feeds found, search budget |
| `numeric-lint.mjs` | flags quantified superlatives with no provenance tag |
| `tracker-consistency.mjs` | flags thread files whose body contradicts their status header |
| `ledger-tally.mjs` | copies the ledger's committed tally into the day's usage log (it never writes the ledger) |
| `estimate-usage.mjs` | a file-size-based estimate of each agent's token use |
| `quality-trend.mjs` | trend lines for the monthly review |
| `render.mjs` | renders a report as HTML for the email, which the Worker then sends |
