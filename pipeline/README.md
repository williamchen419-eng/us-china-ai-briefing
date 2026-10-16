# pipeline/

Per-day working files, one folder per date (`pipeline/YYYY-MM-DD/`). Not included in this edition.

| File | Written by | Read by |
|------|-----------|---------|
| `feeds/china-feeds.md`, `feeds/us-feeds.md` | the prefetch job | the matching desk ("scan the entire file") |
| `china/01-raw.md`, `us/01-raw.md` | China Desk, US Desk | the US Desk (China file), the Strategist, the Red Team |
| `03-strategic.md` | Strategist | Red Team, oracle |
| `04-redteam.md` | Red Team | the monthly review |
| `usage.md` | every agent appends its section; deterministic scripts append token and tally estimates | the ops check, the monthly review |
| `ops-status.md` | the ops check | prepended to the email |

`pipeline/state/seen-urls.json` holds cross-day deduplication state. Feed files are deleted after 3 days and day folders after 14.
