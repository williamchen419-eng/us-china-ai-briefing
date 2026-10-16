# .github/workflows/

GitHub Actions jobs. Not included in this edition.

| Workflow | Trigger | Job |
|----------|---------|-----|
| `fetch-feeds.yml` | dispatched by the Worker's cron | runs the prefetch job and the cleanup |
| `oracle-review.yml` | a push to `reports/` | runs the oracle and the ops check, commits the verdict, requests the email |
| `numeric-lint.yml` | a push to `reports/` | numeric lint and tracker consistency |
| `report-watchdog.yml` | scheduled, late afternoon UTC | alerts only if the chain died (desk output but no report) |
| `ref-guard.yml` | a force-push to `main` | restores a rewound branch fast-forward-only and alerts |

Credentials are referenced as `${{ secrets.NAME }}`, never written into a workflow.
